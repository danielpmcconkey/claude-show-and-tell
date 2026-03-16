# Mock ETL Framework

A from-scratch replica of an enterprise ETL platform. Originally written in C#, then ported entirely to Python.

| | C# | Python |
|---|---|---|
| Lines of code | 12,080 | 10,080 |
| Source files | 109 | 104 |
| Unit tests | 156 | 158 |
| Job configurations | 110 | 103 |
| External modules | — | 73 |
| Documentation markdowns | 20 | 20 |

**Capabilities:**
- Job dispatching from a PostgreSQL queue
- JSON job configurations represented as modular tasks to execute
- Data sourcing, transformation, Parquet output, CSV output (vanilla and with trailing records)
- Dynamically loaded external modules as Python interface classes
- Shared DataFrame state between execution modules (represented as pandas DataFrames)
- Append and overwrite output modes
- Ability to queue multiple ETL date ranges at once

This tool was built through a traditional SDLC process. It was designed to be a faithful mock of a production ETL platform — close enough that techniques proven against it would transfer to the real thing.

[MockEtlFramework (C#)](https://github.com/danielpmcconkey/MockEtlFramework) ・ [MockEtlFrameworkPython](https://github.com/danielpmcconkey/MockEtlFrameworkPython)
