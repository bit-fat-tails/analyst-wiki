---
title: Customization Guide
last_updated: 2026-05-01
---

# Customizing the Analyst Wiki

This guide walks you through making the wiki yours. The default scaffold is a starting point — the real value comes when the structure reflects how you actually think about your coverage.

## Setting Up Your Coverage Universe

Start with `Companies/index.md`. This is the master manifest. Every company you cover gets a row:

```markdown
| Ticker | Name | Sector | Themes | Status |
|--------|------|--------|--------|--------|
| AAPL | Apple Inc | Consumer Electronics | Hardware, Services, AI | Active |
| MSFT | Microsoft Corp | Software / Cloud | Cloud, AI, Enterprise | Active |
| CRWD | CrowdStrike | Cybersecurity | Endpoint, Platform | Watch |
```

**Status options:**
- **Active** — You cover this name. Notes should be current, updated after every earnings and material event.
- **Watch** — On your radar but not in the portfolio. Lighter-touch monitoring.
- **Monitor** — Peripheral relevance (e.g., a competitor to a name you cover). Update when it matters.
- **Pass** — Looked at it, decided against it. Keep the note so you remember why.

**Sectors** are yours to define. Don't just copy GICS — use groupings that match how you analyze companies. A tech analyst might use "Semis," "Cloud Infra," "Consumer Internet," and "Ad-Tech" rather than the official "Information Technology" bucket. The sector should tell you which companies to read together.

**Themes** are cross-cutting tags. A company can have multiple themes. These become the connective tissue — when you want to pull up everything related to "AI capex cycle," themes are how you find it.

For each company, create a folder:
```
Companies/
  AAPL/
    index.md    # Metadata + file manifest (use templates/index_template.md)
    notes.md    # Running notes (use templates/note_template.md)
```

Start with 10-15 names. You can always add more. A wiki with 10 deeply maintained companies is worth more than one with 50 stubs.

## Choosing Your Concept Pages

Concept pages are the analytical layer that makes this more than a filing cabinet. They synthesize a theme across multiple companies and evolve over time.

**Start with 5-8 concepts that connect at least 2-3 companies.** If a concept only touches one name, it belongs in that company's notes, not in a standalone page.

### How to identify the right concepts

Look for themes you find yourself writing about in multiple company notes. If you keep mentioning "AI capex cycle" in your NVDA notes, your MSFT notes, and your AMZN notes, that's a concept page.

### Examples by sector

**Technology / AI analyst:**
- AI Capex Cycle (hyperscaler spend, GPU demand, power constraints)
- Cloud Migration & Repatriation (on-prem vs. cloud economics)
- AI Integration in Consumer Hardware (on-device inference, NPUs)
- Ad-Tech Disruption from AI (creative automation, targeting changes)
- Open Source vs. Proprietary AI (model economics, moat implications)

**Healthcare analyst:**
- GLP-1 Market Expansion (Lilly, Novo, competitive pipeline)
- Biosimilar Pricing Pressure (margin compression timelines)
- Medtech AI Adoption (surgical robotics, diagnostic imaging)
- Gene Therapy Commercialization (manufacturing scale, payer dynamics)

**Financials analyst:**
- Net Interest Margin Cycle (rate sensitivity, deposit betas)
- Embedded Finance & BaaS (fintech-bank partnerships)
- Credit Quality Normalization (delinquency trends by product)
- Insurance Pricing Cycle (catastrophe loss trends, reserve adequacy)

**Industrials / Energy analyst:**
- Datacenter Power Demand (utility capex, grid constraints)
- Reshoring & Supply Chain Localization (CHIPS Act, tariff effects)
- EV Transition Economics (battery cost curves, charging infra)

Each concept page follows `templates/concept_template.md`. The "Current Synthesis" section is the most important — it should be the 3-5 sentence answer to "where does this theme stand today?"

## Adapting CLAUDE.md

The `CLAUDE.md` file in the project root is the instruction set for your AI assistant. Customize it to reflect your workflow:

### Must-change sections

1. **Who You're Working For** — Your name, role, fund, coverage universe. This is context the AI uses for every interaction.

2. **Coverage list** — Replace the example tickers with yours. The AI uses this to know which companies matter to you.

3. **Investment Framework** — How do you think about stocks? Expectations Investing, bottoms-up fundamental, top-down macro, event-driven? The framework shapes how concept pages are structured and how the AI weighs evidence. Write 3-5 sentences about your approach.

4. **Folder Structure** — Adjust if you add or remove top-level directories.

### Nice-to-change sections

5. **Writing style** — If you have a distinct voice (terse, formal, casual-analytical), describe it. The AI will match it when writing on your behalf.

6. **Tool-specific instructions** — Remove references to tools you don't use. Add instructions for tools you do.

7. **Earnings workflow** — If you use a specific transcript provider or have a particular earnings analysis process, document it.

## Connecting Data Sources

The wiki is most powerful when it's connected to live data. The architecture uses MCP (Model Context Protocol) servers as the bridge between the AI and your data sources.

### Transcript providers
If you have access to Quartr, FactSet, or S&P Capital IQ transcripts, add the relevant MCP server to your Claude Code configuration. The wiki's ingest workflow will automatically extract key points from transcripts and update company notes.

### Financial data
Connect a market data provider (FMP, Bloomberg BQL, FactSet) so the AI can pull live prices, estimates, and fundamentals when updating notes or answering questions. Describe in CLAUDE.md which provider to prefer and any usage limits.

### Internal tools
If your fund has proprietary scoring systems, alternative data sources, or internal research databases, create MCP servers for them. The pattern is always the same: expose the tool via MCP, add it to the config, document usage in CLAUDE.md.

### Document storage
Point the wiki at wherever your broker research and internal documents live — Google Drive, SharePoint, a local folder. The AI can read PDFs and extract relevant points into company notes.

You don't need all of these on day one. Start with transcripts and financial data; add the rest as you see what workflow gaps emerge.

## Industry Structure

Industries in this wiki are analytical groupings, not GICS codes. Create industries that reflect how you actually analyze companies together.

**Good industry groupings:**
- "AI Infrastructure" (NVDA, AMD, AVGO, MRVL) — companies that move together on AI capex signals
- "Consumer Internet" (GOOGL, META, SNAP, PINS) — companies competing for ad dollars and user attention
- "Cloud Platforms" (MSFT, AMZN, GOOGL) — analyzed together on workload mix and pricing trends

**Less useful:**
- "Information Technology" — too broad, groups companies with nothing in common
- "Communication Services" — GICS says GOOGL and T are peers; your analysis says otherwise

Each industry gets a folder with `index.md` (metadata, key tickers) and `notes.md` (running industry-level observations). Update industry notes when you see sector-wide shifts that affect multiple names.

A company can appear in multiple industries if that reflects how you analyze it. GOOGL might sit in both "Cloud Platforms" and "Consumer Internet."

## Investment Framework

Your framework should be encoded in the wiki, not just in your head.

### In CLAUDE.md
Write a concise statement of how you invest. The AI uses this as a lens for every piece of analysis. Example:

> We use Expectations Investing: compare the market's implied expectations (via reverse DCF) against our fundamental view. We look for inflection points where consensus expectations are about to shift. Holding periods vary — we're patient, but sell when price meets our target.

### In Concept Pages
Each concept's "Current Synthesis" should reflect your framework. If you're an expectations investor, the synthesis should state what the market prices in and where you disagree. If you're a bottoms-up stock picker, focus on company-specific catalysts. If you're a macro overlay analyst, lead with the macro setup.

### In Company Notes
The "Investment Thesis" in each company's notes.md is the single most important paragraph in the wiki. It should answer: what is the view, why do we hold it, and what changes it. Revisit it quarterly.

## Team Usage

Multiple analysts can share a wiki using Git.

### Single-analyst setup
Fork the repo, customize, push to your private remote. You're the only writer; the AI is your co-author.

### Multi-analyst team

**Branching model:**
- `main` is the shared truth. Always deployable, always readable.
- Each analyst works on a feature branch for substantial updates (e.g., `dipanshu/nvda-earnings-q2`).
- Small updates (adding a development to notes.md, updating a date) can go directly to `main`.

**Ownership:**
- Each company should have a primary analyst. Note it in the company's `index.md` frontmatter (add a `primary_analyst` field).
- Concept pages can be shared, but one analyst should own the "Current Synthesis" to avoid conflicting views.
- Industry pages are typically owned by whoever covers the most names in that industry.

**Merge conflicts:**
- `notes.md` conflicts are common when two analysts update the same company. Since new developments are prepended, conflicts usually resolve cleanly (both additions go in, newest first).
- Concept page conflicts are harder — if two analysts update the synthesis, someone needs to reconcile.

**Code review for research:**
- Use pull requests for thesis documents and major concept page rewrites. This creates a record of analytical evolution and invites debate.

## Scaling Tips

### At 10-20 companies
The default structure works fine. You can navigate by memory. `Companies/index.md` is your homepage.

### At 30-50 companies
- **Group by sector** in `Companies/index.md` using the "By Sector" section. This becomes your primary navigation.
- **Lint regularly.** Run the wiki lint workflow monthly to catch stale pages, orphan notes, and missing concept pages. At this scale, some pages will inevitably drift.
- **Prune aggressively.** Move companies to "Pass" status when you stop covering them. Archive their folders rather than deleting.

### At 50-100 companies
- **Add search tooling.** Full-text search across all markdown files becomes essential. Tools like `ripgrep` work for simple queries. For semantic search (e.g., "which companies benefit from rising AI capex"), consider embedding your wiki into a vector store.
- **Split concept pages** when they get too long. A concept page over 200 lines probably covers multiple distinct themes that deserve their own pages.
- **Automate staleness detection.** Set up a scheduled job to flag company notes not updated in 14+ days.

### At 100+ companies (team wiki)
- **Hierarchical index files.** The root `Companies/index.md` links to sector-level indexes (`Companies/semis/index.md`) which link to individual companies.
- **Dedicated concept page owners.** Assign each concept page to an analyst to prevent drift.
- **Embedding-based search.** At this scale, you need semantic search. Use a vector store (Chroma, Pinecone, or a local FAISS index) to embed all wiki pages. Query with natural language: "What's our view on datacenter power constraints and how does it affect NVDA vs. AMD?"
- **Automated ingest pipelines.** Set up workflows that automatically ingest earnings transcripts, broker reports, and news into the wiki. The AI processes each source through the ingest workflow (extract, update notes, update concepts, cross-reference).
- **Version history matters.** Use Git blame to understand how a thesis evolved. Tag major analytical shifts (e.g., `v2026-q1-thesis-update`) so you can diff your views over time.

### Performance considerations
- Keep individual markdown files under 500 lines. Split when they grow beyond that.
- Use the `log.md` archive mechanism (archive to `log-{year}.md` at 2,000 lines) to keep the activity log manageable.
- Gitignore large binary files (PDFs, Excel models) so the repo stays fast. Store them locally but reference them in index files.

---

The wiki is a tool for compounding knowledge. The more you put in — especially into concept pages and the "Recent Developments" sections of company notes — the more useful it becomes over time. Start small, stay consistent, and let the structure grow with your coverage.
