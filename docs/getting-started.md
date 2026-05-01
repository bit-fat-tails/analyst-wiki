# Getting Started

A step-by-step guide to setting up your Analyst Wiki and building the habit of
compounding research knowledge.

---

## Prerequisites

You need three things:

1. **An LLM agent with file access.** This guide assumes
   [Claude Code](https://docs.anthropic.com/en/docs/claude-code), but any LLM
   agent that can read and write local files works -- Cursor, Windsurf, Copilot
   Workspace, or a custom agent.

2. **Git.** You already have it if you're on a Mac (it ships with Xcode
   command line tools). On Linux, `sudo apt install git`. On Windows,
   [git-scm.com](https://git-scm.com/).

3. **A text editor.** VS Code is recommended because its markdown preview lets
   you browse the wiki like a website. But any editor works -- this is just
   plain text.

---

## Initial Setup

### Fork and Clone

Go to the project repository on GitHub and fork it to your account. Then clone
your fork:

```bash
git clone https://github.com/YOUR_USERNAME/analyst-wiki.git
cd analyst-wiki
```

### Verify the Structure

You should see:

```
Companies/
Concepts/
Industries/
Research/
Daily/
Weekly/
Meetings/
earnings/
ideas/
Portfolio/
Macro/
templates/
log.md
```

### Make Your First Commit

The scaffold is ready to use. Commit it as your baseline:

```bash
git add -A
git commit -m "Initial wiki scaffold"
```

Everything from here is customization.

---

## Configuring Your Coverage

Start with 5-10 companies. You can always add more later. Trying to set up 50
companies on day one leads to thin notes everywhere and a wiki that feels
overwhelming instead of useful.

### Step 1: Edit the Master Index

Open `Companies/index.md`. You'll see a starter table:

```markdown
| Ticker | Name | Sector | Themes | Status |
|--------|------|--------|--------|--------|
| AAPL | Apple Inc | Consumer Electronics | Hardware, services, AI | Active |
```

Replace or extend this with your coverage. Status options:

- **Active** -- companies you follow closely and update regularly
- **Watch** -- companies you track but don't cover deeply
- **Monitor** -- on the radar, checked occasionally
- **Pass** -- evaluated and decided against active coverage

Example for a semiconductor-focused analyst:

```markdown
| Ticker | Name | Sector | Themes | Status |
|--------|------|--------|--------|--------|
| NVDA | NVIDIA Corp | Semiconductors | AI accelerators, data center, gaming | Active |
| AMD | Advanced Micro Devices | Semiconductors | AI accelerators, data center, CPU | Active |
| TSM | TSMC | Semiconductors | Foundry, advanced packaging, CoWoS | Active |
| MU | Micron Technology | Memory | HBM, DRAM, data center | Active |
| INTC | Intel Corp | Semiconductors | Foundry, CPU, restructuring | Watch |
| ASML | ASML Holding | Semicap Equipment | EUV, lithography | Active |
```

### Step 2: Create Company Folders

For each company in your coverage, create a folder and copy the templates:

```bash
# Create the folder
mkdir -p Companies/NVDA

# Copy templates
cp templates/note_template.md Companies/NVDA/notes.md
cp templates/index_template.md Companies/NVDA/index.md
```

### Step 3: Fill In the Frontmatter

Open `Companies/NVDA/notes.md` and fill in the YAML frontmatter at the top:

```yaml
---
ticker: NVDA
name: NVIDIA Corp
sector: Semiconductors
last_updated: 2026-05-01
coverage_status: active
---
```

Do the same for `Companies/NVDA/index.md`:

```yaml
---
ticker: NVDA
name: NVIDIA Corp
sector: Semiconductors
themes: [AI accelerators, data center, gaming, HBM]
status: active
last_updated: 2026-05-01
---
```

You don't need to write the notes content yet. That's Claude's job.

### Step 4: Repeat for Your Other Companies

This is the one tedious step. For 5-10 companies, it takes about 10 minutes.
You can also ask Claude to do it:

> "I've added NVDA, AMD, TSM, MU, INTC, and ASML to Companies/index.md.
> Create the folder structure for each -- copy the templates, fill in the
> frontmatter, and update the index files."

Claude will create all the folders, copy the templates, and fill in the
metadata.

---

## Adding Your First Industry

Industries give you a sector-level view across companies.

### Step 1: Create the Industry Folder

```bash
mkdir -p Industries/semiconductors
```

### Step 2: Create the Index and Notes Files

**`Industries/semiconductors/index.md`:**

```yaml
---
industry: Semiconductors
key_tickers: [NVDA, AMD, TSM, MU, INTC, ASML]
themes: [AI accelerators, HBM, advanced packaging, foundry]
status: active
last_updated: 2026-05-01
---
```

**`Industries/semiconductors/notes.md`:**

```yaml
---
industry: Semiconductors
last_updated: 2026-05-01
---

# Semiconductors

## Current View
[Start empty -- Claude will fill this in as you ingest sources]

## Key Themes
- AI accelerator demand
- HBM supply/demand
- Advanced packaging capacity (CoWoS)
- Foundry competition

## Recent Developments

## See Also
- **Companies:** [NVDA](../../Companies/NVDA/notes.md), [AMD](../../Companies/AMD/notes.md)
- **Concepts:** (none yet)
```

### Step 3: Update the Industries Index

Open `Industries/index.md` and add your industry:

```markdown
| Industry | Key Tickers | Themes | Status |
|----------|-------------|--------|--------|
| semiconductors | NVDA, AMD, TSM, MU, INTC, ASML | AI, HBM, packaging | Active |
```

---

## Your First Ingest

This is where the wiki starts working. Open Claude Code in the wiki directory:

```bash
cd analyst-wiki
claude
```

Now paste a source. This can be anything:

- A paragraph from a broker note
- A quote from an earnings transcript
- A news article
- Your own observation after reading something

**Example:**

> Ingest this: "TSMC reported Q1 2026 revenue of $25.8B, up 35% y/y, beating
> consensus of $24.9B. Management raised full-year capex guidance to $40-44B
> (from $38-42B), citing 'insatiable demand' for CoWoS advanced packaging.
> CoWoS capacity is sold out through 2027. HBM-related revenue grew 3x y/y."
>
> Update the relevant company notes, concept pages, and log.

**What Claude should do:**

1. **Read `Companies/index.md`** to find which companies this affects (TSM,
   and potentially NVDA, MU, AMD through cross-references)

2. **Update `Companies/TSM/notes.md`** -- add a dated entry under Recent
   Developments:
   ```
   ### 2026-05-01
   - Q1 2026 revenue $25.8B (+35% y/y), beat consensus $24.9B
   - FY capex guidance raised to $40-44B (from $38-42B)
   - CoWoS advanced packaging sold out through 2027
   - HBM-related revenue grew 3x y/y
   - Source: TSMC Q1 2026 earnings
   ```

3. **Check for relevant concept pages** in `Concepts/index.md`. If none exist
   yet for CoWoS or HBM, Claude might suggest creating them (or create them if
   the theme appears in multiple company notes).

4. **Update cross-references** -- add links from TSM notes to related company
   notes (NVDA as a customer, MU for HBM supply).

5. **Append to `log.md`:**
   ```
   ## [2026-05-01] ingest | TSMC Q1 2026 earnings
   - Companies/TSM/notes.md
   - log.md
   ```

After the ingest, check the files Claude modified. Read through them. This is
how you calibrate -- make sure the summaries are accurate, the cross-references
make sense, and the level of detail is right for your workflow.

---

## Your First Concept Page

Concept pages are the compounding layer. They synthesize a theme across
multiple companies.

Wait until you have at least 2-3 company notes with content, then ask Claude:

> "I've been ingesting notes about HBM demand across NVDA, MU, and TSM. Create
> a concept page that synthesizes the HBM supercycle theme across these
> companies. Use the concept template."

Claude will:

1. Read the existing notes for NVDA, MU, and TSM
2. Create `Concepts/hbm-supercycle.md` from the template
3. Write a Current Synthesis that articulates the theme and variant view
4. List key data points from across the company notes
5. Write per-company implications
6. Add bidirectional links (concept page links to company notes, company notes
   link to concept page)
7. Register the page in `Concepts/index.md`
8. Log the creation

Review the concept page. The Current Synthesis section is the most important
part -- it should articulate not just what's happening, but what the market
prices vs what you think the data says. If it doesn't capture your view, tell
Claude to revise it.

---

## Your First Query

Now test the retrieval. Ask a question that requires synthesis:

> "If TSMC is sold out on CoWoS through 2027, what does that mean for NVDA's
> supply situation and MU's HBM revenue trajectory?"

Claude should:

1. Read `Concepts/index.md` to find the relevant concept page
2. Read `Concepts/hbm-supercycle.md` for the synthesized view
3. Follow links to `Companies/NVDA/notes.md` and `Companies/MU/notes.md`
4. Synthesize an answer that draws on all the accumulated context

The answer should be richer than what you'd get from a cold query against raw
sources, because the concept page already contains cross-referenced data points
and an articulated view.

If the query produces a substantial, reusable insight, tell Claude to file it:

> "That analysis on CoWoS bottleneck impact is good. Add it to the HBM
> supercycle concept page under a new section, or create a research output
> if it's substantial enough."

Don't let good analysis disappear into chat history. That's the whole point.

---

## Your First Lint

After a week or two of use, run a health check:

> "Lint the wiki. Check for stale pages, orphan pages, missing concepts,
> broken links, data gaps, and index drift."

Claude will scan the wiki and report something like:

```
## Lint Results - 2026-05-15

### Stale Pages (not updated in 14+ days)
- Companies/INTC/notes.md (last_updated: 2026-05-01)

### Missing Concepts
- "CoWoS advanced packaging" mentioned in TSM, NVDA, AMD notes
  but no dedicated concept page exists

### Data Gaps
- Companies/INTC/notes.md has <10 lines of content
- Companies/ASML/notes.md has no thesis document

### Orphan Pages
- (none)

### Broken Links
- Companies/AMD/notes.md links to Concepts/ai-accelerator-demand.md
  which doesn't exist

### Index Drift
- Companies/TSM/transcripts/q1-2026.md exists on disk
  but not listed in Companies/TSM/index.md
```

Work through the punch list. Some items are quick fixes (update the index,
fix broken links). Some require research (create the missing concept page,
update stale notes). The lint is a forcing function that prevents decay.

---

## Tips for Building the Habit

### Start each day with an ingest

Spend 10 minutes at the start of your day feeding overnight news into the wiki.
Broker notes that arrived, news articles, data releases. This is the
equivalent of reading the morning note, except the output persists.

### End each research session by filing

After a deep dive into a company or theme, ask Claude:

> "Is there anything from this session that should be filed into the wiki?
> Any company notes to update, concept pages to revise, or new concept pages
> to create?"

### Weekly lint pass

Pick a day -- Friday afternoon works well. Ask Claude to lint the wiki. Spend
20 minutes working through the punch list. This prevents drift from
accumulating.

### Use the log as your activity record

`log.md` is an append-only chronological record of everything that happened in
the wiki. It's useful for answering "When did I last update my NVDA notes?"
and "What did I ingest last week?" Glance at it occasionally.

### Let concept pages develop organically

Don't try to create all your concept pages on day one. Let them emerge from
ingest sessions. When Claude notices a theme appearing across 3+ company notes,
that's the signal to create a concept page. Forced concept pages tend to be
thin and abandoned.

---

## Common Mistakes

### Writing the wiki yourself

The whole point is to let the LLM do the filing. If you're typing bullets into
`notes.md` by hand, you're using this as a note-taking app, not a wiki. Paste
sources into the conversation and tell Claude to ingest them.

### Skipping the log

The log feels like bureaucracy until you need it. "When did I last update my
view on TSMC capex?" is answerable in seconds with a log. Without it, you're
grepping git history.

### Forgetting bidirectional links

If `Companies/NVDA/notes.md` mentions an HBM concept, it should link to
`Concepts/hbm-supercycle.md`. And that concept page should link back to NVDA.
Always tell Claude to check cross-references. One-way links create orphans.

### Letting good analysis disappear into chat history

You ask Claude a great question. It synthesizes a compelling answer from three
concept pages and five company notes. Then you close the conversation and the
answer is gone. Tell Claude to file it -- update a concept page, create a
research output, or at minimum add a note to the relevant company file.

### Starting too big

50 companies on day one means 50 empty notes.md files. Start with 5-10
companies you actively cover. Get the ingest habit working. Expand once the
workflow feels natural.

### Editing files without telling Claude

If you manually edit a wiki file -- which is fine to do -- mention it in the
next conversation so Claude can update cross-references and the log. Manual
edits that aren't propagated create silent drift.

---

## What Comes Next

Once the basic workflow is running:

- **Add thesis documents** for your highest-conviction positions using
  `templates/thesis_template.md`
- **Create a CLAUDE.md** file in the project root with wiki-specific
  instructions for your LLM (writing style, ingest behavior, concept page
  thresholds)
- **Wire in data sources** via MCP servers (earnings transcripts, financial
  data, web search) so ingests can pull live data
- **Set up a daily routine** where Claude auto-ingests news for your coverage
  companies
- **Share the repo** with your team if collaborative research is useful

The wiki is a pattern, not a finished product. Adapt it to your workflow, your
coverage, and your style of research. The only thing that matters is that
knowledge compounds instead of being re-derived.
