# Proofmark

An independent output equivalence engine. Given two sets of files — CSV or Parquet — it determines whether they're functionally identical.

The name comes from proof marks on firearms: the stamp from an independent proof house certifying that a weapon has been pressure-tested. The proof house doesn't care who built the gun. It cares whether it passes.

This tool was built through a traditional SDLC process, deliberately kept separate from the ETL work, so it could serve as an unbiased validator.

- **4,646 lines** of Python (1,812 source + 2,834 test)
- **206 tests**
- **8-stage comparison pipeline:** Config → Read → Schema Validate → Header/Trailer Compare → Hash → Diff → Correlate → Report

**Capabilities:**
- Dispatching comparison runs through a PostgreSQL queue
- CSV-to-CSV comparison
- Parquet-to-Parquet comparison
- Schema validation (Parquet only)
- Variably defined header and trailer rows (CSV only)
- Variably defined column-level strictness (strict, fuzzy, excluded)
- Order-independent row hash comparison via MD5
- Fuzzy numeric tolerances with configurable thresholds
- Null handling with sentinel values
- Mismatch correlation via greedy column similarity matching
- PostgreSQL queue-based serving mode with idle shutdown

[proofmark](https://github.com/danielpmcconkey/proofmark)
