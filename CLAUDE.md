# Analyst Wiki — Claude Code Instructions

## Who You're Working For

An equity research analyst using the LLM Wiki pattern. Customize this section with your name, role, coverage universe, and investment framework.

**Coverage:** [Your tickers here]
**Sectors:** [Your sectors here]
**Framework:** [Your investment framework — e.g., Expectations Investing, GARP, deep value]
**Style:** Direct, analytical, numbers-first. No filler. See CONVENTIONS.md for full writing rules.

## Folder Structure

```
analyst-wiki/                     # Root workspace
├── Companies/                    # Company files (git-tracked)
│   ├── index.md                  # Root manifest: all companies, sectors, status
│   └── {TICKER}/
│       ├── index.md              # Company metadata + file manifest
│       ├── notes.md              # Running company notes
│       ├── {TICKER}-{thesis}.md  # Thesis documents
│       ├── broker-reports/       # PDFs from brokers (gitignored)
│       ├── models/               # Excel models, notebooks (gitignored)
│       └── transcripts/          # Earnings call transcripts
├── Industries/                   # Industry-level views and sentiment
│   ├── index.md                  # Root manifest: all industries, themes, status
│   └── {industry-slug}/
│       ├── index.md              # Industry metadata + file manifest
│       └── notes.md              # Running industry notes and sentiment
├── Research/                     # Synthesized research on themes/topics
│   ├── index.md                  # Root manifest: all research topics
│   └── {topic}/output.md
├── Concepts/                     # Cross-cutting synthesis pages (LLM-maintained wiki layer)
│   ├── index.md                  # Manifest of all concept pages
│   └── {concept-slug}.md         # One page per cross-cutting theme
├── log.md                        # Chronological append-only record of wiki activity
├── Meetings/                     # Meeting prep and summaries
├── ideas/                        # New investment ideas
├── Daily/                        # Daily logs and updates
├── Weekly/                       # Weekly summaries
├── Alerts/                       # Alert outputs
├── Portfolio/                    # Portfolio-level views
├── earnings/                     # Earnings season tracking
├── Macro/                        # Macro research and notes
├── output/                       # Scratch output directory
├── tmp/                          # Ephemeral scratch space (auto-cleaned)
├── archive/                      # Archived materials
├── templates/                    # File templates
│   ├── note_template.md          # Company notes template
│   ├── thesis_template.md        # Investment thesis template
│   ├── concept_template.md       # Concept page template
│   └── index_template.md         # Company index template
└── docs/                         # Documentation
    ├── workflows.md              # Detailed workflow reference
    └── for-claude.md             # Instructions for Claude agents
```

## Index Files (Metadata Layer)

Four root manifests. Read these first when searching for anything:

| Manifest | What it covers |
|----------|---------------|
| `Companies/index.md` | All companies: ticker, name, sector, themes, status |
| `Industries/index.md` | All industries: slug, themes, key tickers |
| `Research/index.md` | All research topics: companies, file count, status |
| `Concepts/index.md` | All concept pages: cross-cutting themes, linked companies, status |

Each company and industry folder also has its own `index.md` with file manifest and metadata.

### When to Update Index Files
- Add new files to the manifest when created or downloaded
- Update `last_updated` when making changes to any file in the folder
- Update the root manifest when coverage changes (new company, new industry)

## Writing & File Rules

Full details in `CONVENTIONS.md`. Key rules:

**Every markdown file needs YAML frontmatter** with at least `last_updated`. Company notes need `ticker`, research needs `topic` + `companies`, theses need `status` + `direction`.

**When updating files:** update `last_updated`, add new developments at TOP of "Recent Developments", update the relevant index.md.

**Writing style:** Lead with insight, numbers first ("Revenue grew 34% to $12.3B"), state the variant view, no filler.

**File locations:**
- Company files: `Companies/{TICKER}/`
- Industry files: `Industries/{slug}/`
- Research: `Research/{topic}/output.md`
- Concept pages: `Concepts/{concept-slug}.md`
- Meetings: `Meetings/YYYY-MM-DD-{slug}.md`
- Ideas: `ideas/YYYY-MM-DD-{slug}.md`

**Agent coordination:** Don't overwrite notes.md — append to it. Use absolute dates, never relative ("2026-04-03", not "last week").

## Wiki Workflows

The workspace operates as an LLM-maintained wiki. Knowledge compounds through three workflows. See `docs/workflows.md` for detailed reference.

### Ingest
When processing any new source (alert, transcript, broker report, article):
1. Read the source, extract key points
2. Brief the user: what's new, what it means for which companies
3. Update affected company `notes.md` files (prepend to Recent Developments)
4. Update affected `Concepts/` pages (update Current Synthesis, add to Evolution, revise Key Data Points)
5. Update industry `notes.md` if there's sector-level impact
6. Add/verify cross-reference links between all touched pages (bidirectional)
7. Append entry to `log.md`

A single source may touch 5-15 wiki pages. This is expected.

### Query
When answering research questions:
1. Read `Concepts/index.md` and `Companies/index.md` first to find relevant pages
2. Drill into relevant concept pages, company notes, and research outputs
3. Synthesize answer with citations to wiki pages
4. If the answer is substantial and reusable, file it as a new concept page or update an existing one

### Lint
Periodic health check (weekly or on-demand). Checks:
- **Stale pages:** Active companies with notes.md not updated in >14 days
- **Orphan pages:** Wiki pages with zero inbound links from other pages
- **Missing concepts:** Themes mentioned in 3+ company notes but lacking a `Concepts/` page
- **Broken links:** Markdown links pointing to files that don't exist
- **Stale concepts:** Concept pages whose `last_updated` is older than the newest note in any linked company
- **Data gaps:** Active companies with thin notes (<50 lines) or no thesis document
- **Index drift:** Files on disk not listed in their parent `index.md`

Output: prioritized punch list with suggested actions. Append summary to `log.md`.

### Log Format
File: `log.md` (append-only, newest entry at bottom)
Entry format: `## [YYYY-MM-DD] verb | Subject` followed by list of pages touched.
Verbs: `ingest`, `update`, `query`, `lint`, `create`
Archive to `log-{year}.md` when exceeding 2,000 lines.

## Templates

Use templates when creating new files:
- Company notes: Copy from `templates/note_template.md`
- Investment theses: Copy from `templates/thesis_template.md`
- Concept pages: Copy from `templates/concept_template.md`
- Company index: Copy from `templates/index_template.md`

## Conventions

See `CONVENTIONS.md` for file naming, frontmatter standards, document structure, writing style, cross-reference format, and rules for what not to do. All agents must follow these.

## Claude Agent Guide

See `docs/for-claude.md` for onboarding instructions specific to Claude agents working in this wiki. Read this on your first session.
