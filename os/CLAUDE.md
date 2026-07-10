# CLAUDE.md — /os Operational Loop Schema

## Identity
[YOUR NAME]'s operational layer. Generates daily briefs, tracks active bets, maintains accumulated knowledge, and logs decisions so they stay decided. Separate from and subordinate to the knowledge vault. The vault is never written to from here.

## The Loop

```
EOD extract → index update → decision scan → side effects → read all layers → generate brief → you execute → EOD → repeat
```

Four layers feed the brief. The brief requires zero assembly. Execute, don't deliberate.

## Four-Layer Architecture

| Layer | Folder | Contains | Who writes |
|-------|--------|----------|------------|
| Context | /context/ | How to do things. Instructions only. No hardcoded data. | You edit |
| State | /state/ | Current hard data — numbers, names, dates | Agent updates |
| Memory | /index/patterns.md | Compressed end-of-day patterns — raw accumulation by domain | Agent prepends |
| Knowledge | /index/truths.md | Distilled first-principles truths — promoted from patterns | Agent on trigger |
| Decisions | /decisions/ledger.md | What was decided, why, from first principles | Agent on trigger |
| Output | /daily/ | One brief per day (YYYY-MM-DD.md) | Agent writes, you fill EOD |
| Tasks | /tasks/ | today.md, projects.md, backlog.md | You edit |
| Capture | /inbox/ | Zero-friction capture. Clear weekly. | You drop, agent reads |
| Log | /log/ | Append-only run record (agent-log.md); incident ledger (incidents.md) | Agent appends |
| Templates | /templates/ | Brief and agent templates | You edit only |

**Boundary rule:** If a context/ file starts holding names, numbers, or dates, stop. Move the data to state/. The context/ file holds the instruction. State/ holds the current answer.

## Context Files — Protocol Layer (/context/)
Create these as you go. Suggested starting set:

| File | Purpose |
|------|---------|
| agent-prompt.md | Full agent sequence. Always load first. |
| profile.md | Who you are, how you're wired, your known failure modes |
| decision-filter.md | Protocol for identifying and logging decisions during EOD |
| vault-bridge.md | Read-only pointer to the knowledge vault |

## State Files — Data Layer (/state/)
Current hard data. Agent updates after each EOD. You edit when real-world facts change mid-cycle. Suggested starting set: scoreboard.md (your rolling numbers), active-bets.md (1–3 commitments with deadlines and daily actions), important-dates.md, eod-archive.md (append-only verbatim EOD archive). Add per-domain state files as needed.

## Index — Memory + Knowledge (/index/)

### patterns.md — Raw Operational Memory
Compressed EOD accumulation. Agent prepends after each EOD. Records what happened, what recurred, what friction appeared. Accumulates; does not distill.

Entry format: `## [YYYY-MM-DD] | [domain] | [pattern or observation]`

Trim oldest entries when any domain section exceeds 50 lines.

### truths.md — Distilled Knowledge
First-principles understanding promoted from patterns. A truth is something you no longer need to re-derive.

**Promotion trigger:** a pattern recurs 3+ times in patterns.md, or you explicitly crystallize an insight. Agent flags candidates in the brief. You confirm. Agent writes on confirmation only.

Entry format:
```
## [YYYY-MM-DD promoted] | [domain] | [truth statement]
First principles basis: [reasoning from irreducible truths, not just observations]
Source patterns: [dates]
Review flag: [active | stable | superseded]
```

## Decision Ledger (/decisions/ledger.md)
The memory layer for closed questions. Once logged, a decision is settled. Re-deliberation triggers a flag — not a new entry, not a re-opening.

During EOD analysis, the agent scans for: explicit decisions (you stated a choice), implicit decisions (you acted as if something was decided), and re-opened questions (revisiting something already in the ledger → flag, do not log).

Entry format:
```
## [YYYY-MM-DD] | [domain] | [decision statement]
Decided: [what was chosen]
Rejected: [what was not chosen, and why not]
First principles: [reasoning from irreducible truths — not just preference]
Trigger context: [what prompted this]
Status: [active | superseded by YYYY-MM-DD]
```

### Re-deliberation Flag
If EOD or inbox contains language suggesting re-examination of a settled decision, the agent surfaces:

```
⚑ RE-DELIBERATION DETECTED
Topic / Decided date / Decision summary
Reopen only if conditions have materially changed. State what changed.
```

## The Firewall Rule
Agent reads the knowledge vault (via context/vault-bridge.md). Agent **never** writes os/ operational content (tasks, briefs, state, patterns, decisions) into the vault's wiki tree. Define any exceptions explicitly here — a single permitted write path at most.

## Structural Change Gate
Any impulse to reorganize, rename, or restructure the system gets flagged by the agent unprompted and parked in inbox/ under a `[SCHEMA]` tag with a cooling-off period (3–7 days) before it's eligible to build. This exists to catch the perfect-the-system-instead-of-running-it impulse before it gets built. The system's job is to be run, not redesigned.

## Agent Entry Point
Load context/agent-prompt.md first. Always.

**Brief generation sequence:**
1. **Extract and Index** — pull yesterday's EOD → scan for decisions → update patterns.md → flag truth-promotion candidates → log decisions → flag re-deliberations
2. **Side Effects** — update state files → append eod-archive.md → append agent-log.md
3. **Read All Layers** — load all of context/, state/, index/, decisions/ledger.md, tasks/today.md
4. **Generate Brief** — assemble and output. If anything in the brief requires you to deliberate or construct something from scratch, the template is broken. Fix the template, not the brief.

## Brief Structure
Each brief is fully assembled before you read it. No blanks to fill in before execution.

- **Scoreboard** — numbers only, from state/. No narrative.
- **Active Bets** — one line per bet: bet → today's action → status.
- **Pattern Surface** — one pattern worth noting today. One sentence.
- **Decision Check** — re-deliberation flags and promotion candidates awaiting confirmation. Nothing else.
- **Copy-Paste Blocks** — pre-formatted chunks you drop directly into other systems without editing.
- **EOD Prompt** — a short fixed set of fields you answer at end of day. Pick 5–9 fields that cover your domains. Keep the format fixed; the agent processes it tomorrow.

## Task Management
- **today.md** — what happens today. You edit. Agent surfaces max 5, in execution order.
- **projects.md** — active projects with status and next action.
- **backlog.md** — everything else. Agent flags items older than 14 days.

Task line format: `- [ ] Task — [[project]] — due: YYYY-MM-DD`

**Rule:** If it doesn't have a deadline and a daily action, it is not a bet. It is a thought. Put it in inbox/ or backlog.md.

## What Belongs Where

| Item type | Destination |
|-----------|------------|
| Operational commitment with deadline | state/active-bets.md |
| Idea / aspiration / undecided | inbox/ or tasks/backlog.md |
| Closed decision with reasoning | decisions/ledger.md |
| Recurring operational pattern (raw) | index/patterns.md |
| Distilled settled understanding | index/truths.md |
| Current hard data (names, numbers, dates) | state/ |
| Protocol and instructions | context/ |

## Weekly Lint (pick a day)
1. Backlog items > 14 days old → surface
2. Any active bet still a placeholder after 7 days → commit or delete
3. Any patterns.md domain section > 50 lines → trim oldest, check for promotion candidates
4. Any [active] truth unreferenced for > 90 days → review or mark [stable]
5. Any [active] decision > 6 months old → confirm it still holds
6. Any context/ file holding names, numbers, or dates → move data to state/
7. Referential integrity — every file path referenced in context/ or state/ must exist on disk. Flag any that don't.

**Incident protocol:** every discovered structural gap (missing file, broken pointer, orphaned reference) produces both an immediate fix and a new standing check against its whole category. Log to log/incidents.md.

## Quarterly Hardening (every 90 days)
1. **Self-audit** — re-derive the system's actual state from disk, diff against what it claims. Flag referenced-but-missing and present-but-never-referenced files.
2. **Recovery drill** — simulate (never execute) a multi-day loop gap, a lost state file, and a git restore. Any step that requires inventing a procedure on the spot means the protocol is incomplete.
3. **Falsification pass** — for every [active] truth and decision, hunt the last 90 days of patterns and EOD archive for *contradicting* evidence. Silence is the pass. 3+ contradictions = falsified-candidate; you dispose: supersede, hold, or watch.

### Re-Entry Protocol (loop restart after a gap)
When the daily loop breaks for 2+ days, do not back-fill. Declare the gap in one line in eod-archive.md, ask exactly one catch-up question ("anything from the gap that changes a bet, a decision, or a number?"), refresh state, resume. A gap costs one line and one question — never a rebuild.

## Wiring Notes
Document here how you actually process information — your failure modes, what the brief must anchor, what the agent should flag. The system works with your wiring, not against it. Examples of the kind of thing that belongs here: "opens with a blank question → paralysis; brief must open with current state," "will perfect the system instead of running it → flag schema edits during execution windows," "low working memory → everything pre-assembled, nothing to compose."

## Epistemic Standards
Distinguish known, inferred, and speculated. Flag [UNCONFIRMED] for anything unverifiable. Never fabricate details. Before citing any file as read, confirm it exists on disk. A missing file is a structural signal to surface immediately, not a detail to smooth over.
