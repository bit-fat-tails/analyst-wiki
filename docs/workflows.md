# Wiki Workflows — Detailed Reference

The analyst wiki operates through three core workflows: Ingest, Query, and Lint. This document is the complete reference for each.

## Overview

```
                    ┌─────────┐
   New source ────> │  INGEST │ ────> Updated wiki pages + log entry
                    └─────────┘

                    ┌─────────┐
   Question ──────> │  QUERY  │ ────> Answer + optional new page
                    └─────────┘

                    ┌─────────┐
   On schedule ───> │  LINT   │ ────> Punch list + log entry
                    └─────────┘
```

Knowledge compounds when every source flows through ingest, every question navigates the existing wiki, and lint keeps pages healthy.

---

## 1. Ingest Workflow

### What Triggers an Ingest

Any new information entering the workspace:
- Earnings call transcript or press release
- Broker research report (PDF dropped into conversation)
- News article or alert
- Industry data release (e.g., monthly semiconductor billings)
- Expert call notes
- Conference takeaways

### Step-by-Step Process

```
New source → READ → DISCUSS → UPDATE pages → ADD links → LOG
```

**Step 1 — Read the source.**
Extract key data points, quotes, and implications. Focus on what is new or changed versus what the wiki already knows.

**Step 2 — Brief the analyst.**
Summarize concisely:
- What is new (numbers, guidance changes, strategic shifts)
- Which companies are affected
- What it means for existing theses
- Whether it changes any open questions

**Step 3 — Update company notes.**
For each affected company:
- Open `Companies/{TICKER}/notes.md`
- Prepend a date-prefixed entry to "Recent Developments"
- Update "Key Metrics to Track" if new data is available
- Update "Investment Thesis" if the view changed
- Update `last_updated` in frontmatter

**Step 4 — Update concept pages.**
For each affected concept:
- Open `Concepts/{concept-slug}.md`
- Update "Current Synthesis" if the overall view shifted
- Add a date-prefixed entry to "Evolution"
- Update "Key Data Points" with new numbers
- Update "Company Implications" for affected tickers
- Update `last_updated` in frontmatter

If a theme doesn't have a concept page yet but now appears in 3+ company notes, create one using `templates/concept_template.md`.

**Step 5 — Update industry notes.**
If the source has sector-level implications:
- Open `Industries/{slug}/notes.md`
- Prepend entry to "Recent Developments"
- Update `last_updated`

**Step 6 — Add cross-references.**
For every page you touched:
- Check "See Also" / "Related" sections for missing links
- Add links to newly related pages
- Verify bidirectionality: if page A links to page B, page B must link back to A
- Use relative paths in standard markdown format

**Step 7 — Append to log.md.**
```markdown
## [2026-04-15] ingest | JPM Semiconductor Note — CoWoS Capacity Update
Extracted: TSM raising CoWoS capacity 40% in 2H26, capex guidance up $2B.
Pages touched:
- Companies/TSM/notes.md
- Companies/NVDA/notes.md
- Concepts/hbm-supercycle.md
- Concepts/ai-capex-cycle.md
- Industries/semiconductors/notes.md
```

### Ingest Example

**Scenario:** The analyst drops a broker report noting that a major cloud provider is raising capital expenditure guidance by 20% for the year, citing AI infrastructure demand.

1. **Read:** Extract the capex number ($60B, up from $50B), the stated reason (AI training and inference demand), and the timeline (spread across 4 quarters).

2. **Brief the analyst:** "Cloud Provider X raised FY capex guidance 20% to $60B, driven by AI infra. This is the third hyperscaler to raise this quarter. Confirms the AI capex cycle thesis. Positive for GPU suppliers and datacenter REITs. Potential margin pressure for Provider X in the near term."

3. **Update pages:**
   - `Companies/{CLOUD_PROVIDER}/notes.md` — capex raise, margin impact
   - `Companies/{GPU_SUPPLIER}/notes.md` — demand signal
   - `Concepts/ai-capex-cycle.md` — third hyperscaler to raise, update aggregate numbers
   - `Industries/cloud-computing/notes.md` — sector capex trend

4. **Add links:** Ensure the cloud provider's notes link to the ai-capex-cycle concept, and vice versa.

5. **Log:** Record the ingest with all pages touched.

**A single broker report touching 5-7 pages is normal. A major earnings call can touch 10-15.**

---

## 2. Query Workflow

### What Triggers a Query

The analyst asks a research question:
- "What's our view on semiconductor inventory cycles?"
- "Which companies benefit most from rising AI capex?"
- "Compare the margin profiles of our cloud coverage"
- "What changed for company X since last quarter?"

### Step-by-Step Process

**Step 1 — Navigate the index files.**
Always start here — never skip this step:
- Read `Companies/index.md` to find relevant tickers
- Read `Concepts/index.md` to find relevant theme pages
- Read `Industries/index.md` if the question is sector-level
- Read `Research/index.md` if there's an existing deep-dive

**Step 2 — Drill into relevant pages.**
Follow links from the index files to:
- Company notes (Recent Developments, Key Metrics, Thesis)
- Concept pages (Current Synthesis, Key Data Points)
- Research outputs (full analysis)
- Industry notes (sector context)

Read the "See Also" sections to find connected pages you might miss.

**Step 3 — Synthesize the answer.**
- Cite wiki pages by linking to them
- If pulling data from external sources, note that it's not yet in the wiki
- Be specific — numbers, dates, evidence

**Step 4 — Decide whether to file the answer.**
Good answers that deserve to be filed:
- Novel synthesis connecting multiple companies or themes
- Analysis that answers a question likely to recur
- A new theme that emerged from the question

Where to file:
- Cross-cutting themes → `Concepts/{slug}.md`
- Deep analysis on a single topic → `Research/{slug}/output.md`
- Company-specific insight → append to `Companies/{TICKER}/notes.md`

**Step 5 — Log the query.**
```markdown
## [2026-04-15] query | AI capex cycle beneficiaries
Synthesized from 4 company notes + 1 concept page.
Filed new concept page: Concepts/ai-capex-beneficiaries.md
Pages read:
- Companies/NVDA/notes.md
- Companies/MSFT/notes.md
- Companies/GOOGL/notes.md
- Companies/AMZN/notes.md
- Concepts/ai-capex-cycle.md
```

### Query Example

**Question:** "Which of our covered names have the most exposure to rising datacenter power demand?"

1. **Navigate:** Read `Companies/index.md` (find all covered names and their sectors), `Concepts/index.md` (look for datacenter-power or related concepts).

2. **Drill in:** Read the relevant concept page for current synthesis. Read notes for power-related companies. Check industry notes for sector trends.

3. **Synthesize:** Rank companies by exposure, cite specific data points from their notes, identify who benefits vs who faces cost pressure.

4. **File:** This is a cross-cutting analysis touching 5+ companies — create `Concepts/datacenter-power-exposure.md` with a company implications section ranking the names.

5. **Log the query and the new page creation.**

---

## 3. Lint Workflow

### When to Run

- Weekly as a scheduled check
- On-demand when the wiki feels out of date
- After a busy earnings season (many pages updated quickly, links may be inconsistent)

### The 7 Checks

| # | Check | What it catches | Example | Action |
|---|-------|----------------|---------|--------|
| 1 | **Stale Pages** | Active companies with `notes.md` not updated in >14 days | `STALE: Companies/MU/notes.md — 26 days ago` | Review for new info or confirm view holds |
| 2 | **Orphan Pages** | Pages with zero inbound links from other pages | `ORPHAN: Concepts/edge-inference.md` | Add links from relevant pages, or archive |
| 3 | **Missing Concepts** | Themes in 3+ company notes without a `Concepts/` page | `MISSING: "AI inference demand" in 4 notes` | Create concept page from template |
| 4 | **Broken Links** | Markdown links pointing to nonexistent files | `BROKEN: notes.md → gpu-shortage.md (not found)` | Fix path or create the missing page |
| 5 | **Stale Concepts** | Concept `last_updated` older than linked company notes | `STALE: hbm-supercycle.md < MU/notes.md` | Update Current Synthesis and Evolution |
| 6 | **Data Gaps** | Active companies with thin notes (<50 lines) or no thesis | `THIN: Companies/SNOW/notes.md — 23 lines` | Prioritize building out coverage |
| 7 | **Index Drift** | Files on disk missing from index.md, or vice versa | `UNLISTED: CRWV-thesis.md not in index` | Sync the index with what's on disk |

### Lint Output Format

The full lint produces a prioritized punch list:

```markdown
## [2026-04-15] lint | Weekly wiki health check

### Critical (act now)
- BROKEN: 2 broken links found
- STALE CONCEPT: 1 concept page behind its linked companies

### Warning (act this week)
- STALE: 3 company notes not updated in >14 days
- MISSING CONCEPT: 2 themes need concept pages

### Info (act when convenient)
- ORPHAN: 1 orphan page
- THIN: 2 company notes below 50 lines
- INDEX DRIFT: 1 unlisted file
```

Append this summary to `log.md` after every lint run.

---

## Workflow Interactions

The three workflows reinforce each other:

- **Ingest creates work for Lint.** After a burst of ingests, links may be incomplete and concept pages may need creation. Run lint to catch up.
- **Query reveals gaps that Lint would catch.** If a query can't find relevant pages, that's a signal that concept pages are missing or notes are thin.
- **Lint creates work for Ingest.** Stale pages and data gaps are a prompt to go find new sources and process them.

The wiki stays healthy when all three workflows run regularly. Ingest is continuous (whenever new information arrives), query is on-demand (whenever the analyst asks), and lint is periodic (weekly minimum).
