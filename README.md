# Analyst Wiki

Most analysts' research lives in scattered notes, broker PDFs, and chat history.
Every earnings season you re-derive the same context from scratch.
This is a system that compounds instead.

**Analyst Wiki** is a ready-to-fork markdown workspace where an LLM agent
(Claude Code, or any agent with file access) maintains a persistent, interlinked
knowledge base for equity research. Instead of retrieving raw chunks on every
query (like RAG), the LLM incrementally builds and maintains wiki pages --
company notes, concept pages, industry views -- with cross-references,
a chronological log, and periodic health checks.

Based on Andrej Karpathy's
[LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

---

## Table of Contents

- [What Is This?](#what-is-this)
- [How It Works](#how-it-works)
- [Quick Start](#quick-start)
- [Directory Structure](#directory-structure)
- [Templates](#templates)
- [Example Workflow](#example-workflow)
- [Automations](#automations)
- [Customization](#customization)
- [Philosophy](#philosophy)
- [FAQ](#faq)
- [Credits](#credits)
- [License](#license)

---

## What Is This?

Karpathy's insight is simple: an LLM can maintain a wiki the way a diligent
research associate would. Three layers make this work:

1. **Raw sources** -- broker reports, earnings transcripts, news articles,
   Slack messages, your own observations. These are inputs. They flow through
   the wiki but don't live there permanently.

2. **The wiki** -- markdown files that the LLM reads, updates, and
   cross-references every time a new source arrives. Company notes get a new
   bullet. Concept pages get revised. Links get wired. The wiki is the
   persistent artifact.

3. **The schema** -- templates, index files, frontmatter conventions, and a
   log that give the LLM enough structure to file things consistently without
   you micromanaging every update.

The result: each source you ingest makes every future query cheaper and better.
After six months, your wiki knows which companies are exposed to an HBM
supercycle, what the consensus expects for TSMC capex, and where your
view diverges from the street -- all without you manually maintaining a
spreadsheet of cross-references.

---

## How It Works

The wiki operates through three core workflows.

### 1. Ingest

You feed a source to the LLM. It reads the source, extracts key points, and
propagates changes across the wiki.

**What happens on every ingest:**

- Affected company `notes.md` files get a new entry under Recent Developments
- Affected concept pages get their Current Synthesis revised
- Industry notes get updated if there's sector-level impact
- Cross-reference links are added or verified (bidirectional)
- An entry is appended to `log.md`

A single broker report might touch 5-15 wiki pages. That's the point -- the
LLM does the tedious cross-referencing work so the knowledge compounds.

### 2. Query

You ask a question. The LLM reads the relevant index files to find the right
pages, synthesizes across company notes, concept pages, and research outputs,
and gives you an answer grounded in your accumulated knowledge -- not just
whatever a search engine returns.

Because the wiki already contains synthesized views with cross-references and
variant views articulated, queries that would take 30 minutes of re-reading
broker notes take seconds.

### 3. Lint

A periodic health check that catches drift and decay. The LLM audits the wiki
for:

- **Stale pages** -- active companies not updated in 14+ days
- **Orphan pages** -- wiki pages with zero inbound links
- **Missing concepts** -- themes mentioned across 3+ company notes but lacking
  a dedicated concept page
- **Broken links** -- markdown links pointing to files that don't exist
- **Data gaps** -- active companies with thin notes or no thesis document
- **Index drift** -- files on disk not listed in their parent index

Output: a prioritized punch list. Run it weekly.

---

## Quick Start

### Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (or any LLM
  agent with file read/write access)
- Git
- A text editor (VS Code recommended -- you'll want the markdown preview)

### Setup

1. **Fork and clone the repo**

   ```bash
   # Fork on GitHub first, then:
   git clone https://github.com/YOUR_USERNAME/analyst-wiki.git
   cd analyst-wiki
   ```

2. **Make your first commit**

   ```bash
   git add -A
   git commit -m "Initial wiki scaffold"
   ```

3. **Add your coverage**

   Open `Companies/index.md` and add your companies. For each company, create
   a folder:

   ```bash
   mkdir -p Companies/NVDA
   ```

   Copy the templates into place:

   ```bash
   cp templates/note_template.md Companies/NVDA/notes.md
   cp templates/index_template.md Companies/NVDA/index.md
   ```

   Fill in the ticker, name, and sector in the YAML frontmatter.

4. **Open the workspace in Claude Code**

   ```bash
   claude
   ```

5. **Start ingesting**

   Paste a news article, broker snippet, or earnings quote into the
   conversation. Tell Claude:

   > "Ingest this. Update the relevant company notes, concept pages, and log."

   Claude will read the source, update the affected wiki pages, add
   cross-references, and log the activity.

6. **Ask your first query**

   > "What's our current view on HBM demand and which companies in our coverage
   > are most exposed?"

   Claude will navigate the index files, read the relevant company notes and
   concept pages, and synthesize an answer.

7. **Run your first lint**

   > "Lint the wiki. What's stale, what's missing, what needs attention?"

That's it. The wiki gets richer with every session.

---

## Directory Structure

```
analyst-wiki/
├── Companies/                  # Company-level research
│   ├── index.md                # Master list: all companies, sectors, status
│   └── {TICKER}/
│       ├── index.md            # Company metadata + file manifest
│       ├── notes.md            # Running notes (the LLM maintains this)
│       ├── {TICKER}-{thesis}.md # Thesis documents
│       ├── broker-reports/     # PDFs from brokers (gitignored)
│       ├── models/             # Excel models, notebooks (gitignored)
│       └── transcripts/        # Earnings call transcripts
│
├── Concepts/                   # Cross-cutting synthesis pages
│   ├── index.md                # Manifest of all concept pages
│   └── {concept-slug}.md       # One page per theme (e.g., hbm-supercycle.md)
│
├── Industries/                 # Industry-level views
│   ├── index.md                # All industries and their key tickers
│   └── {industry-slug}/
│       ├── index.md            # Industry metadata
│       └── notes.md            # Running industry notes
│
├── Research/                   # Deep-dive research outputs
│   ├── index.md                # All research topics
│   └── {topic-slug}/
│       └── output.md           # Completed research
│
├── Alerts/                     # Time-sensitive signals (news, filings, broker notes)
├── Daily/                      # Daily logs and market notes
├── Weekly/                     # Weekly summaries
├── Meetings/                   # Meeting prep and notes
├── earnings/                   # Earnings season tracking
├── ideas/                      # New investment ideas
├── Portfolio/                  # Portfolio-level views
├── Macro/                      # Macro research
│
├── tools/                      # Custom tooling (scripts, search, automation)
├── projects/                   # Research-related side projects
├── personal/                   # Career development, travel, learning
├── hiring/                     # Hiring materials, candidate evaluation
│
├── templates/                  # File templates
│   ├── note_template.md        # Company notes template
│   ├── thesis_template.md      # Investment thesis template
│   ├── concept_template.md     # Concept page template
│   └── index_template.md       # Company index template
│
├── log.md                      # Chronological activity log (append-only)
├── archive/                    # Archived materials
├── tmp/                        # Scratch space (gitignored)
└── output/                     # Scratch output (gitignored)
```

### The Four Index Files

These are the navigation layer. The LLM reads them first to find what it needs:

| File | Purpose |
|------|---------|
| `Companies/index.md` | Every company: ticker, name, sector, themes, status |
| `Industries/index.md` | Every industry: key tickers, themes |
| `Concepts/index.md` | Every concept page: linked companies, status |
| `Research/index.md` | Every research topic: companies, file count, status |

When you add a new company, industry, or concept page, update the relevant
index file. When you create a new file in a company folder, update that
company's `index.md`.

---

## Templates

Every new file starts from a template. This gives the LLM consistent structure
to work with.

**`templates/note_template.md`** -- Company notes with YAML frontmatter
(ticker, sector, status), sections for thesis, business model, key metrics,
competitive position, risks, recent developments, and cross-references.

**`templates/thesis_template.md`** -- Investment thesis with direction
(long/short), conviction level, the setup, the insight, supporting evidence,
key assumptions, what proves you wrong, and catalysts with timelines.

**`templates/concept_template.md`** -- Cross-cutting concept page with current
synthesis, key data points, per-company implications, an evolution log showing
how the view has changed, and bidirectional links.

**`templates/index_template.md`** -- Company folder index with file manifest,
models, broker reports, and cross-references.

---

## Example Workflow

You just read a Morgan Stanley note arguing that Apple's services revenue is
decelerating because App Store take rates are under regulatory pressure in the
EU. Here's what happens when you ingest it:

**You say:**

> "Ingest this Morgan Stanley note on AAPL services. Key points: Services
> revenue growth decelerating from 14% to 9% y/y. App Store take rate under
> pressure from EU DMA. MS cuts AAPL PT from $245 to $230."

**Claude does the following:**

1. **Updates `Companies/AAPL/notes.md`** -- adds a dated entry under Recent
   Developments with the MS downgrade, services deceleration data, and EU
   regulatory context.

2. **Updates or creates a concept page** -- if `Concepts/app-store-regulation.md`
   exists, Claude revises the Current Synthesis to incorporate the MS data
   point. If it doesn't exist but regulatory risk appears in 3+ company notes
   (AAPL, GOOGL, META), Claude creates the concept page.

3. **Checks related companies** -- if GOOGL notes mention Play Store take rate
   risk, Claude adds a cross-reference between the AAPL note and the GOOGL
   note, and links both to the concept page.

4. **Updates `Industries/consumer-electronics/notes.md`** -- if the MS note has
   sector-level implications.

5. **Updates index files** -- any new concept page or research topic gets
   registered in its parent index.

6. **Appends to `log.md`:**
   ```
   ## [2026-05-01] ingest | MS AAPL services downgrade
   - Companies/AAPL/notes.md
   - Concepts/app-store-regulation.md (created)
   - Companies/GOOGL/notes.md (cross-ref added)
   - log.md
   ```

**Next week**, when you ask "What's our exposure to EU tech regulation?",
Claude reads `Concepts/app-store-regulation.md`, follows the links to AAPL and
GOOGL notes, and synthesizes an answer that incorporates everything you've
ingested -- not just the MS note, but every source that touched this theme.

That's the compounding.

---

## Customization

This scaffold is a starting point. Customize it for your coverage universe and
workflow:

**Adding sectors.** Create new industry folders under `Industries/`. Wire them
into `Industries/index.md`. Link company notes to their industry.

**Changing templates.** Edit the files in `templates/`. Add sections that
matter for your coverage (e.g., a "Regulatory" section for healthcare, a
"TAM Model" section for software). The LLM will follow the template structure
for new files.

**Adding folders.** Common additions: `Macro/` for macro themes, `Portfolio/`
for portfolio-level views, `earnings/` for earnings season tracking. The wiki
pattern works the same way -- the LLM updates pages and cross-references
regardless of folder structure.

**CLAUDE.md configuration.** For Claude Code specifically, create a `CLAUDE.md`
file in the project root with instructions for how the LLM should behave in
this workspace. Tell it about the wiki workflows (ingest, query, lint), point
it to the index files, and specify your writing style preferences. See
`docs/getting-started.md` for a walkthrough.

**MCP integrations.** If you use Claude Code, you can wire in MCP servers for
earnings transcripts (Quartr), financial data (Visible Alpha, FMP), web search
(Exa), and more. The ingest workflow stays the same -- the source just comes
from an MCP tool instead of a paste.

See `docs/getting-started.md` for detailed setup instructions and
`docs/philosophy.md` for the reasoning behind the design.

---

## Automations

The wiki becomes dramatically more useful when you automate the routine
ingestion and monitoring work. A well-run wiki has 4-6 daily automations
and 3-4 weekly ones:

| Automation | Cadence | What It Does |
|-----------|---------|-------------|
| Morning Briefing | Daily, pre-market | Overnight news, calendar, market moves → `Daily/` |
| News Monitor (AM + PM) | 2x daily | Material news → `Alerts/` |
| Email/Research Extractor | Daily | Broker notes, newsletters → company notes + `Daily/` |
| Weekly Review | Monday | Synthesize week into themes → `Weekly/` |
| Earnings Tracker | Monday (daily in season) | Upcoming dates + prep gaps |
| Memory/Context Update | Weekly | Keep LLM long-term memory current |

Start with morning briefing + news monitor + weekly review. Add the rest
once those are solid.

See `docs/automations.md` for detailed descriptions, output contracts,
and setup instructions for Claude Code, Codex, and custom schedulers.

---

## Philosophy

The short version:

- **You can outsource your thinking but you cannot outsource your
  understanding.** The LLM does the thinking — summarizing, cross-referencing,
  filing, monitoring. You do the understanding — forming conviction,
  identifying variant views, making investment decisions. The wiki makes your
  understanding deeper, not optional.
- **Knowledge should compound, not be re-derived.** RAG retrieves chunks. A
  wiki accumulates understanding.
- **Concept pages are the alpha.** Company notes are table stakes. Pages that
  synthesize themes across companies -- "HBM supercycle", "AI inference cost
  curves", "app store regulation" -- are where the compounding happens.
- **Markdown + git = version history for free.** Diffs show how your thinking
  evolved. No vendor lock-in. No subscription. Works with any LLM.

Read the full essay: [docs/philosophy.md](docs/philosophy.md)

---

## FAQ

**Do I need Claude Code specifically?**

No. Any LLM agent that can read and write local files works. Claude Code is
what this was built with, but Cursor, Windsurf, Copilot Workspace, or a custom
agent with file tools would all work. The key requirement is persistent file
access within a conversation.

**How is this different from Obsidian / Notion / Roam?**

Those are note-taking tools where you do the writing and linking. This is an
LLM-maintained wiki where you provide sources and the LLM does the writing,
linking, and maintenance. You still read and direct the wiki, but you're not
the one typing bullets into notes.md.

**How is this different from NotebookLM / ChatGPT with file uploads?**

Those are RAG systems -- they retrieve chunks from your uploaded documents on
each query. There's no persistent synthesis, no cross-referencing, no
accumulated concept pages. Every conversation starts from scratch against raw
sources. This wiki is a compiled artifact that gets richer over time.

**Won't the wiki get stale?**

Yes, if you stop feeding it. That's what the lint workflow catches. Run it
weekly. It tells you which companies haven't been updated, which concept pages
are behind the latest notes, and which links are broken.

**How big can the wiki get?**

Practically, the limit is context window size. The LLM reads index files to
navigate, so it doesn't need to load the entire wiki at once. Individual
company notes of 200-500 lines and concept pages of 100-300 lines are typical.
With good index files, a wiki covering 50+ companies works fine.

**Should I edit wiki files directly?**

You can, but the point is to let the LLM do it. If you manually edit a file,
tell the LLM so it can update cross-references and the log. Manual edits that
don't get propagated create drift.

---

## Credits

- **Andrej Karpathy** for the
  [LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
  that inspired this project
- **[Claude Code](https://docs.anthropic.com/en/docs/claude-code)** by
  Anthropic, the LLM agent this was built with and for

---

## License

MIT License. See [LICENSE](LICENSE).

Fork it, customize it, make it yours. If you build something interesting on
top of this, open an issue -- always curious to see how others use it.
