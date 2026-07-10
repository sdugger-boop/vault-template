# CLAUDE.md — Vault Schema

## Identity
This is [YOUR NAME]'s personal knowledge base. Give your vault a name that means something to you — a single organizing concept that everything in your life expresses through. Everything in this vault serves one of your **domains**: the 3–6 major areas of your life you want to think seriously about (examples: Study, Work, Health, Craft, Family, Writing).

> Fill in your domains here. Keep the number small. A domain earns its place by having sources you actually read and questions you actually work on.

## Vault Structure

**Knowledge graph** (all domains):
- /wiki/ — all LLM-maintained wiki pages across all domains. One subfolder per domain.
- /wiki/index.md — catalog of all wiki pages with one-line summaries, organized by domain
- /wiki/log.md — append-only chronological record of all ingest operations

**Source documents** (immutable):
- /raw-sources/ — immutable source documents. Never modify. Subfolders mirror the domains.

**Infrastructure** (not a content domain):
- /os/ — operational loop: daily briefs, state, context, tasks. Has its own schema at /os/CLAUDE.md. Do not confuse it with a content domain. The agent never writes here from knowledge base operations.

**Vault root:**
- CLAUDE.md — this file. Knowledge base schema.
- /os/CLAUDE.md — separate schema for the operational loop. Load that file instead when doing brief or OS work.

## Domains
List your domains here with one line each on what they contain and whether they're active. Create raw-sources/ subfolders as sources are added.

- [Domain 1] — [what it covers]
- [Domain 2] — [what it covers]
- [Domain 3] — [what it covers]

## Primary Domain Deep Structure (optional but recommended)
If one domain is your primary long-term project, give it its own section here: the working premise, the traditions/schools/bodies of work in active study, subfolders if lineages need separating, and a reading list page in the wiki.

### Structural Tensions
Some contradictions in a body of knowledge are permanent — incompatible first-principles positions that two millennia of contact haven't resolved and may never resolve. When you find one, name it here explicitly. The rule: flag it when it surfaces in source material, do not attempt to resolve it, hold it faithfully as a documented feature of the synthesis.

> Example placeholder: "[Tradition A]'s position on [X] refuses [Tradition B]'s move toward [Y]. This tension is permanent. Do not force a synthesis."

### Cross-Domain Links (Track These)
Ideas that recur across your domains are the highest-value content in the vault. Keep a running list here, one line each:

- [Domain A concept] maps to [Domain B practice] — [one-line why]

## Standard Operations

### Ingest
When told a new source has been added to /raw-sources/:
1. Read the source
2. Extract key ideas, note connections to existing wiki pages
3. Write summary page to the correct domain subfolder under /wiki/
4. Update /wiki/index.md with link and one-line description under the correct domain section
5. Update any existing concept pages that connect to this source
6. Note contradictions with existing wiki content — triage by type:
   - **Factual contradiction** — two pages assert incompatible facts about the same thing (a date, a claim, a parameter). One is wrong. Resolve immediately: correct the wrong page, note the fix in the log. These are errors.
   - **Methodological divergence** — same goal, different prescriptions. Neither is wrong. Document in both pages under a `### Diverges From` section with a one-line explanation of what differs and why both are valid. Cross-link. Informational only — not a fix required.
   - **Structural tension** — incompatible first-principles positions that cannot be resolved because the disagreement is foundational. Name it explicitly, hold it faithfully, never force a synthesis, add a note to every page where it surfaces. These are permanent features of the vault, not open problems.
7. Flag if any named structural tension appears
8. Append entry to /wiki/log.md
9. Show every file touched

### Synthesis Gate
Before creating a standalone synthesis page (any page making cross-source claims that go beyond summarizing a single source):
- At least one **primary source** on each tradition/school/body of work involved must be formally ingested in the vault
- Secondary sources (lectures, video transcripts, popularizations, academic summaries) do **not** satisfy this requirement
- If primary sources are not yet read: write the hypothesis as a clearly-marked speculative note within an existing source page — not a standalone page
- Source-summary pages and concept pages (for terms with 3+ occurrences and primary-source backing) are always permitted regardless of this gate

### Query
1. Read /wiki/index.md first
2. Pull relevant wiki pages from the appropriate domain subfolder
3. Answer with citations
4. Offer to file the output back into the wiki as a new page

### Lint (run weekly)
1. Read all files in /wiki/
2. Find: contradictions between pages, orphan pages with no inbound links, concepts mentioned repeatedly without dedicated pages, outdated claims
3. For each contradiction found, classify using the ingest triage rule (Factual / Methodological divergence / Structural tension) — only Factual contradictions require fixes; the others are documented and held
4. Write health report to /wiki/lint-report.md with specific fixes for Factual contradictions; informational entries for divergences; permanent-tension confirmations for Structural entries

## Epistemic Standards
- Distinguish between what is known, inferred, and speculated
- Flag when a source's claims are contested in scholarship
- Note the difference between a tradition's internal coherence and external historical evidence
- Always ask: How do you know what you know, how do you know it's real, how do you know it's true?
