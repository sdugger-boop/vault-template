# Vault Template — An LLM-Maintained Personal Knowledge System

A framework for running a personal knowledge base and daily operational loop with an AI agent (Claude, via Cowork or Claude Code). The agent reads the `CLAUDE.md` files in this repo as its operating manual — you point it at the folder and the system runs itself.

## What this is

Two connected systems in one vault:

**The knowledge base** (root + `/wiki/` + `/raw-sources/`) — you drop in source documents, the agent ingests them into wiki pages, maintains an index, tracks contradictions between sources, and enforces epistemic standards (what's known vs. inferred vs. speculated). Governed by the root `CLAUDE.md`.

**The operational loop** (`/os/`) — a daily brief system with four memory layers: raw patterns accumulate, recurring ones get promoted to settled truths, decisions get logged once and stay decided, and each morning's brief arrives fully assembled with nothing left for you to compose. Governed by `/os/CLAUDE.md`.

The two are firewalled: the knowledge base holds what you're learning; the operational layer holds what you're doing. The agent reads across but never lets operational clutter leak into the knowledge graph.

## Design principles worth keeping even if you change everything else

1. **Sources are immutable.** `/raw-sources/` is never edited. The wiki is derived; sources are ground truth.
2. **Contradictions are triaged, not flattened.** Factual errors get fixed. Methodological differences get documented on both sides. Foundational tensions get named and held permanently — never force-resolved.
3. **Synthesis is gated.** No cross-source synthesis pages until you've actually read a primary source on each side. Keeps the wiki honest.
4. **Decide once.** The decision ledger exists so settled questions stay settled. Reopening requires stating what materially changed.
5. **Instructions and data never share a file.** Context files hold protocol; state files hold current numbers. When a protocol file grows a phone number, something's wrong.
6. **The system is run, not redesigned.** Schema-change impulses get parked with a cooling-off period.

## Setup

1. Clone or download this repo, rename the folder to whatever you want your vault called.
2. Open the folder in Claude (Cowork: select the folder; Claude Code: `cd` into it).
3. Edit the root `CLAUDE.md`: name yourself, define your 3–6 domains, delete what doesn't apply.
4. Edit `/os/CLAUDE.md`: fill in the Wiring Notes and pick your EOD fields (or delete `/os/` entirely if you only want the knowledge base).
5. Make it a git repo (`git init`) and commit — the quarterly recovery drill assumes git history exists.

## Your first week

- **Day 1:** Define your domains in `CLAUDE.md`. Create `/wiki/<domain>/` folders for the active ones.
- **Day 2:** Drop your first source (a PDF, an article, book notes) into `/raw-sources/<domain>/` and tell the agent: "New source added — ingest it." Watch what it produces.
- **Day 3–4:** Ingest two or three more sources. Ask the agent a question and watch it cite wiki pages.
- **Day 5:** If using `/os/`: write `state/active-bets.md` (1–3 commitments with deadlines) and ask for your first brief.
- **Day 7:** Run your first lint: "Run the weekly lint." Read `/wiki/lint-report.md`.

## Structure

```
CLAUDE.md          — knowledge base schema (the agent's operating manual)
wiki/              — agent-maintained knowledge pages, one subfolder per domain
  index.md         — catalog of all pages
  log.md           — append-only ingest record
raw-sources/       — immutable source documents, one subfolder per domain
os/                — optional operational loop (own schema: os/CLAUDE.md)
  context/         — protocol: how the agent operates
  state/           — current hard data
  index/           — patterns.md (raw memory) + truths.md (distilled knowledge)
  decisions/       — ledger.md (settled decisions)
  daily/           — one brief per day
  tasks/ inbox/ log/ templates/
```

## License / provenance

Template extracted from a working personal vault. Use it, fork it, rename everything. The value is the architecture, not the name.
