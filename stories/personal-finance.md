# PersonalFinance

A Monte Carlo retirement simulator Dan has been building since before Claude existed. 27,500 lines of C#, 391 tests, version 0.18.1 when Claude entered the picture.

It combines ML principles with financial Monte Carlo simulation — running probabilistic retirement projections incorporating investment portfolio growth, inflation modelling, federal tax calculations, withdrawal strategies, and spending patterns across thousands of simulated lifetimes.

**What Claude did:** Ripped out the old pricing model and replaced it with a VAR(3) vector autoregression engine. Added dividend reinvestment for mid-term positions. Rewrote the treasury rate model using Ornstein-Uhlenbeck mean reversion. Added ~1,900 lines of new tests and ran a full SDLC experiment — BRD through implementation plan — on the existing codebase.

~6,000 lines of Claude's work inside a mature, human-built application. A feature that would have taken Dan months, delivered in a weekend.

[PersonalFinance](https://github.com/danielpmcconkey/PersonalFinance)
