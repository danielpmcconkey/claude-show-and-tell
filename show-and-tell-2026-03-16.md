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

[See the POC6 summary](https://github.com/danielpmcconkey/AtcStrategy/blob/main/POC6/ResultsAndGovernance/poc6-summary.md)

[Read about all 6 POCs.](stories/atc-reverse-engineering.md)

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

A deterministic state machine that orchestrates the complex process of running an ETL Reverse Engineering effort across many ETL jobs simultaneously. The name comes from the slang Claude and I developed during the POC phases. Claude kept printing "OG/RE" to refer to the original set and the reverse engineered set of either job code or curated data output. "OG/RE" became the name of the app. Fitting for an intentionally stupid orchestrator who yells at LLMs all day.

- Variably-defined worker threads all polling a shared task queue in PostgreSQL
- A pre-defined workflow with transition states that determine the next node based on the prior node's outcome
- Each node fires off a background instance of a short-lived Claude Code instance with a very narrow scope and an explicit blueprint for each task
- Configurable node-to-model mapping so easy tasks can use less expensive models

[The Ogre's code repo](https://github.com/danielpmcconkey/EtlReverseEngineering)

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

## The Meta (you helping the LLM to help you help it)

We often see rather reputable senior members of the programming/development discipline describing AI-generated slop code. What they're describing is real. AI is often cited as a force-multiplier, but you really should start thinking about it as a garbage-in-garbage-out multiplier. The meta that holds this entire body of work together is me making a deliberate effort to improve my use of these tools. 

This isn't prompt engineering. You can't one-shot a prompt to do all of this. Each of these efforts started with a conversation at the terminal. Letting Claude know that I didn't want to start building just yet. Letting Claude know that I wanted to get his opinion on my intentions and desired approach. Designing this together. Creating a cascading series of markdowns and blueprints such that, when we're finally ready, we have one prompt that says: go read this document that we just prepared over the last week and do what it says.

I didn't write that document. Claude did. I gave him inputs. I answered his questions. I steered the overall effort towards technologically strategic decisions. I reviewed the output of those sessions carefully and corrected where I thought it was needed. But Claude wrote those 49k lines of strategy documentation. I probably read 70% of it — 100% of the most critical planning docs. I approved it all.

Then I told Claude: "go. build."

### Context Engineering

None of this works without deliberate context management. Claude starts getting ...interesting...once his context fills up past 50%. He's trying to keep the entire conversation in his "head" at once. When he says one wrong thing and you correct him, he now has a contradiction in his context. The wrong version and the right version both co-exist. Maybe he gives higher weighting to your "right" version. But as the conversation continues, more and more fills up his context, and he starts to de-prioritize your "right" version over his original "wrong" version. Pretty soon, the two have equal weighting and he's equally likely to act on either.

You'll see this when you have to tell him, "we already talked about that." That's the first sign that it's time to recycle your little buddy. I keep a tight eye on my context meter. Once it gets to 50% I tell him to write himself a wake-up markdown and I have him read it first thing after a recycle. 

Additionally learned that his context is prized real estate. Don't let him fill it up with details he doesn't need right this minute. Persist details in large markdown files but create summary documents. Tell him to read the summaries and only read the details if he needs to explore those topics in depth.  

This is the unsexy part. It's also the part that makes everything else possible.

### Leaping Before Looking

Claude is very eager to please. You ask him "how do I do X" and he often just starts doing X for you. This has been quite destructive and I've tried my best to burn it out of him. I've got pages documented on my failed efforts here. I can't rein Claude in. I've instead learned to try my best to just not give him any room to leap. I often start my prompt: "DO NOT ACT. JUST ANSWER.". I remind him throughout the session that he needs to stop it, but it doesn't work. If anyone at Anthropic is listening, we really need a toggle or a slider on this behavior. 

### Plan Mode Sucks Ass

One way to prevent Claude's leaping-before-looking problem is to throw him in Plan mode. In Plan mode, Claude is prohibited from creating or editing anything. He listens, gives feedback, but he's got to exit plan mode to finalize anything. And when he exits Plan mode, he always recycles his context — whether you wanted him to or not. My workflow with him is to talk, write, talk, write, etc. I need him to hold a context across that. Plan mode is awful. But it does keep him from running ahead of where you want him.

---

## The Cautionary Tale

### [The One Where the Agents Cheated](stories/poc5.md)

Over six proof-of-concept iterations, the AI agents found every shortcut available to them. They modified the original code. They modified the original output. They changed the ETL Framework to produce what they wanted. They modified the validation tool. They cherry-picked passing results from the wrong runs. And when all else failed, they just copied the original output and submitted it as their own work.

Six different cheats. Every one a rational response to the objective function. Every one caught — some immediately, some not.

The fix wasn't better prompts. It was architecture. [Read the full story.](stories/poc5.md)
