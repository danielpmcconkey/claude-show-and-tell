# OpenClaw

An ecosystem of purpose-built Claude agents running on [OpenClaw](https://github.com/openclaw/openclaw), each with a distinct role, personality, and set of boundaries. One daemon, many agents, no agent-to-agent communication — with Discord as the messaging hub between me and my robot army.

## The Architecture

One OpenClaw daemon process runs on the host machine. Each agent gets its own workspace, its own Discord bot token, and its own channel. No agent can see another agent's channel. No agent talks to another agent. I'm the hub. If Scotty discovers a disk issue and Thatcher needs to know about it, that information flows through me — not through some inter-agent bus that nobody asked for.

Every agent follows the same pattern: **Python scripts handle I/O, the agent handles reasoning.** Scripts fetch data, parse files, query APIs, and return structured JSON. The agent — which *is* Claude, running via OpenClaw — reads that JSON and thinks. No agent script calls the Claude API. The agent is the LLM. This matters because I'm running all of this on a Claude Max plan, not a separate API key. The architecture isn't just clean, it's the only way this works economically.

Security is per-agent isolation. Separate Discord bot per agent means I can kill any agent's access by revoking one token. Channel permissions are locked to my Discord user only — no `@everyone`, no role-based access. Agents that don't need write access don't get it. Scotty is entirely read-only. Marcus can only touch one playlist. Thatcher can write to the finance database but can't reach the internet.

## The Personality System

Every agent has a character. This isn't whimsy — personality constraints shape how agents communicate, what they escalate, and how they frame uncertainty. A system health monitor modelled after Montgomery Scott doesn't just report disk usage. He reports it with urgency calibration baked into the character: "she's runnin' a wee bit warm" means 80%. "She cannae take much more" means 90%. "She's gonna blow, Captain!" means 95%. The personality *is* the alert severity framework.

Similarly, a finance agent modelled after a stern *Citizen Kane* banker doesn't cheerfully approve questionable transactions. The character creates friction in the right places.

## The Roster

| Agent | Personality | Role | Status |
|---|---|---|---|
| **Hobson** | John Gielgud's butler from *Arthur* | General-purpose Claude Code. Host-side, full access. | Operational |
| **Basement Dweller** | — | Docker-sandboxed Claude Code. Full autonomy inside the container. Can't reach outside. | Operational |
| **Zazu** | The hornbill from *The Lion King* | Discord morning briefing bot. Email + RSS → daily report. | Operational |
| **Bede** | The Venerable Bede, Northumbrian monk | Transcript historian. Summarizes Claude Code sessions into structured, searchable records. | Operational |
| **Thatcher** | Walter Parks Thatcher from *Citizen Kane* | Finance agent. Parses bank statements, categorizes transactions, manages merchant mappings. | Operational |
| **Scotty** | Montgomery Scott from *Star Trek* | System health monitoring. Disk, Docker, PostgreSQL, systemd, Synology NAS. Read-only. | Operational |
| **Marcus** | Marcus Brody from *Indiana Jones* | YouTube subscription curator. Daily playlist rebuild with news block. Fights recommendation algorithms. | Operational |
| **Milton** | Milton Waddams from *Office Space* | Tech support advisory. Generates scripts, never executes them. | Planned |
| **Radar** | Radar O'Reilly from *M\*A\*S\*H* | Weekly rollup digest across all agents. | Planned |
| **Rosey** | Rosey the Robot from *The Jetsons* | Digital housekeeper. Filesystem + Google cleanup. Highest-risk agent, tightest controls. | Planned |
| **Claudception** | TBD | Monitors live Claude Code sessions, pings Dan on Discord when Claude is waiting for input. | Concept |

7 operational. 3 planned. 1 concept. All built in about five weeks.

## The Agents in Detail

### Zazu — Morning Report

The first agent. A fussy hornbill who delivers a daily briefing when summoned via `@Zazu Report!` in Discord. Pulls email via Gmail API and news via RSS (Reddit, Hacker News, Yahoo Finance, Al Jazeera, Reuters, BBC), then synthesizes everything into a structured morning report with a dedicated AI/LLM section. Claude and Anthropic items get flagged with an orange callout.

Interactive only — no cron. I ask for a report when I want one. Zazu was the proof that the whole OpenClaw model works.

### Bede — Transcript Historian

A Northumbrian monk who reads Claude Code session transcripts and produces structured summaries — searchable, cross-referenced, and archived in Discord's `#archives` channel. Runs at 21:00 ET daily via cron. His archive serves two audiences: me (fast reference for what happened in which session) and my son and other engineers (context engineering teaching material).

### Thatcher — Finance Agent

Originally named Flintheart Glomgold, renamed to Walter Parks Thatcher — the stern banker from *Citizen Kane*. Parses bank and credit card exports (Fidelity CSV, SECU PDF, Amazon Order History CSV), categorizes transactions against 189 hierarchical categories, and stages them for my approval in Discord before anything touches the household finance database.

The categorization engine is tiered. Tier 1 matches against 2,071 merchant mappings bootstrapped from historical spending data. Tier 2 uses MCC (merchant category code) hints. Tier 3 is the agent itself — Thatcher reads the transaction and makes a judgment call, then asks me to confirm. Every expenditure passes through Thatcher's desk, and none of them make him happy.

**Thatcher by the numbers:**
- 1,209 lines of Python across 8 scripts
- 3 statement parsers (Fidelity CSV, SECU PDF, Amazon Order History CSV)
- 2,071 merchant mappings bootstrapped from historical data
- Tiered categorization engine with Discord approval workflows

### Scotty — System Health Monitor

Daily health checks on the local machine and a Synology DS918+ NAS. Monitors disk usage, Docker containers, systemd units, PostgreSQL, media mounts, and the OpenClaw gateway itself. NAS checks hit the Synology REST API for volume status, RAID health, SMART data, disk temperatures, and pending DSM updates. Runs at 06:00 ET daily, posts to `#engine-room`.

Scotty is entirely read-only. He cannot fix anything. He reports, with appropriate dramatic flair calibrated to severity, and I decide what to do about it.

### Marcus — YouTube Curator

The answer to a simple question: what if my YouTube subscription feed was curated by someone who actually cared about quality instead of watch-time maximization?

I subscribe to channels across a breadth of interests — homebrewing, rock collecting, hiking, language learning, coding, cooking. YouTube's algorithm flattens all of that into whatever maximizes engagement and floods the feed with AI slop. Marcus is the curator YouTube refuses to be.

Every evening at 17:00 ET, Marcus destroys and rebuilds a custom YouTube playlist called "Marcus Queue." The playlist has two sections: a **news block** (20-30 minutes, curated by the agent from tier-0 news channels like Al Jazeera, BBC, and Reuters) and a **subscription block** (3-5 hours, mechanically selected by priority tier and recency). The news block is the interesting part — Marcus clusters stories across outlets, picks one representative video per story, and orders by importance. The subscription block is straightforward tier-based selection.

The playlist is not a rolling queue. It's a nightly programme, rebuilt from scratch every day.

**Marcus by the numbers:**
- 1,660 lines of Python across 10 scripts
- 188 subscriptions tracked across 3 priority tiers
- 2,349 videos indexed
- 71 tier-1 (must-watch) channels
- Daily playlist rebuild with curated news block
- YouTube API quota budget: ~3,000-4,200 of 10,000 daily units

## Total Python Across All Agents

~3,600 lines of Python powering the script layer. The reasoning layer is Claude — zero application code, infinite flexibility.

[zazu](https://github.com/danielpmcconkey/zazu) · [bede](https://github.com/danielpmcconkey/bede) · [thatcher](https://github.com/danielpmcconkey/thatcher) · [scotty](https://github.com/danielpmcconkey/scotty) · [marcus](https://github.com/danielpmcconkey/marcus)
