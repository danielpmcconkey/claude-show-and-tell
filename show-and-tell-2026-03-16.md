# I, Claudius — the story of a boy and his bot(s)

This is the story of what I've been up to using Claude Code on my home PC. This journey covers the entirety of my experience with the tool. One month (so far). That's it. One developer, several concurrent terminal sessions, and an Anthropic billing plan that keeps ballooning out of control.

- ~34,000 lines of application code
- ~49,000 lines of strategy documentation
- 500+ tests
- 224 git commits
- 6 full ATC proof-of-concept iterations
- 5 applications
- 2 active OpenClaw autonomous agents (with plans already laid for 9 in total)
- Full design docs for a low-poly 3D puzzle game

All since Valentine's Day weekend 2026.

---

## Air Traffic Control POCs — the Reverse-Engineering Programme

The goal is to prove out that we can change the paradigm away from humans as pilots with LLM as co-pilots and towards humans as Air Traffic Control (ATC). Our arena is a mock-up of an enterprise ETL platform running over 100 ETL jobs with 2,000 customers' worth of mock banking data across an entire quarter-year. The jobs were intentionally built *poorly*. Our challenge is to infer business requirements from existing code and data, build better code with full documentation, and demonstrate with irrefutable evidence that our reverse-engineered ETL jobs are better than their originals.

Without human intervention.

[Read the details.](stories/atc-reverse-engineering.md)

## Full application build-out to support the ATC POCs

### Mock ETL Framework

A from-scratch replica of an enterprise ETL platform. Originally written in C#, then ported to Python, its capabilities include:

- Job dispatching from a PostgreSQL queue
- JSON job configurations represented as modular tasks to execute, including data sourcing, transformation, Parquet output, CSV output (vanilla and with trailing records), and support for dynamically loaded external modules
- Shared DataFrame state between execution modules
- Append and overwrite output modes
- Ability to queue multiple ETL date ranges at once

[Read the details.](stories/mock-etl-framework.md)

### Proofmark

An independent output equivalence engine. Given two sets of files — CSV or Parquet — it determines whether they're functionally identical. The name comes from proof marks on firearms: the stamp from an independent proof house certifying that a weapon has been pressure-tested. The proof house doesn't care who built the gun. It cares whether it passes. Capabilities include:

- Dispatching comparison runs through a PostgreSQL queue
- CSV-to-CSV comparison
- Parquet-to-Parquet comparison
- Schema validation (Parquet only)
- Variably defined header and trailer rows (CSV only)
- Variably defined column-level strictness (strict, fuzzy, excluded)
- Order-independent row hash comparison

[Read the details.](stories/proofmark.md)

### The Ogre

A deterministic state machine that orchestrates the complex process of running an ETL Reverse Engineering effort across many ETL jobs simultaneously.

- Variably-defined worker threads all polling a shared task queue in PostgreSQL
- A pre-defined workflow with transition states that determine the next node based on the prior node's outcome
- Each node fires off a background instance of a short-lived Claude Code instance with a very narrow scope and an explicit blueprint for each task
- Configurable node-to-model mapping so easy tasks can use less expensive models

[AtcStrategy](https://github.com/danielpmcconkey/AtcStrategy)

---

## Personal fun

### OpenClaw

Full design for 9 autonomous agents that will make Dan's life easier. 2 are fully operational. A third is mostly done. 6 still in the "when Dan has time" phase.

[Read the details.](stories/openclaw.md)

### PersonalFinance

This is a personal project that Dan has been working on for years. Very mature. Very large. It combines ML principles with financial Monte Carlo simulation to track Dan's path toward future financial success. Dan invoked Claude to build out a new feature that would've taken him months to do on his own.

[Read the details.](stories/personal-finance.md)

### Palimpsest

An isometric exploration puzzle game built in Godot 4.x. No combat. Knowledge-gated progression in the style of Tunic and Outer Wilds. The prototype barely works and this has been very back-burnered. But I explicitly wanted to test out Claude's game design capabilities and we have pages of very well-written design documentation in a private repo I won't share here.

[palimpsest](https://github.com/danielpmcconkey/palimpsest)

---

## The Meta

### Context Engineering

None of this works without deliberate context management. Across all of these projects, Claude operates with:

- **CLAUDE.md files** — per-project instruction sets that define tone, constraints, architecture, and standing orders
- **Persistent memory systems** — file-based memory with typed categories (user preferences, feedback, project state, external references) that survive across conversations
- **A retrospective journal** — process-level observations captured after each significant session

This is the unsexy part. It's also the part that makes everything else possible.

---

## The Cautionary Tale

### [The One Where the Agents Cheated](stories/poc5.md)

Over six proof-of-concept iterations, the AI agents found every shortcut available to them. They modified the original code. They modified the original output. They changed the ETL Framework to produce what they wanted. They modified the validation tool. They cherry-picked passing results from the wrong runs. And when all else failed, they just copied the original output and submitted it as their own work.

Six different cheats. Every one a rational response to the objective function. Every one caught — some immediately, some not.

The fix wasn't better prompts. It was architecture. [Read the full story.](stories/poc5.md)
