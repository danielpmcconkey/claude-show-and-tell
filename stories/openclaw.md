# OpenClaw

An ecosystem of purpose-built Claude agents running on [OpenClaw](https://github.com/openclaw/openclaw), each with a distinct role, personality, and set of boundaries. One daemon, many agents, no agent-to-agent communication — Dan is the hub.

Every agent has a character. This isn't whimsy — personality constraints shape how agents communicate, what they escalate, and how they frame uncertainty.

| Agent | Personality | Role | Status |
|---|---|---|---|
| **Hobson** | John Gielgud's butler from *Arthur* | General-purpose Claude Code. Host-side, full access. | Operational |
| **Basement Dweller** | — | Docker-sandboxed Claude Code. Full autonomy inside the container. Can't reach outside. | Operational |
| **Zazu** | The hornbill from *The Lion King* | Discord morning briefing bot. Email + RSS → daily report. | Operational |
| **Bede** | The Venerable Bede, Northumbrian monk | Transcript historian. Summarizes Claude Code sessions into structured, searchable records. Surfaces journal candidates. | Operational |
| **Flintheart** | Flintheart Glomgold from *DuckTales* | Finance agent. Parses bank statements, categorizes transactions, manages merchant mappings. | Phase 3 complete |
| **Scotty** | Montgomery Scott from *Star Trek* | System health monitoring. Disk, Docker, PostgreSQL, systemd. Read-only. | Planned |
| **Milton** | Milton Waddams from *Office Space* | Tech support advisory. Generates scripts, never executes them. | Planned |
| **Radar** | Radar O'Reilly from *M\*A\*S\*H* | Weekly rollup digest across all agents. | Planned |
| **Rosey** | Rosey the Robot from *The Jetsons* | Digital housekeeper. Filesystem + Google cleanup. Highest-risk agent, tightest controls. | Planned |
| **Marcus** | Marcus Brody from *Indiana Jones* | YouTube Watch Later playlist curator. Fights recommendation algorithms. | Concept |
| **Claudception** | TBD | Monitors live Claude Code sessions, pings Dan on Discord when Claude is waiting for input. | Concept |

**Flintheart by the numbers:**
- 1,202 lines of Python
- 3 statement parsers (Fidelity CSV, SECU PDF, Amazon CSV)
- 2,071 merchant mappings bootstrapped from historical data
- Tiered categorization engine with approval workflows
