# Conventions

Rules for agents and humans working in this workspace. All agents must follow these.

## File Naming

| Type | Pattern | Example |
|------|---------|---------|
| Company notes | `Companies/{TICKER}/notes.md` | `Companies/AAPL/notes.md` |
| Company index | `Companies/{TICKER}/index.md` | `Companies/AAPL/index.md` |
| Company thesis | `Companies/{TICKER}/{TICKER}-{slug}.md` | `Companies/AAPL/AAPL-services-rerating.md` |
| Concept page | `Concepts/{concept-slug}.md` | `Concepts/ai-capex-cycle.md` |
| Industry notes | `Industries/{slug}/notes.md` | `Industries/semiconductors/notes.md` |
| Industry index | `Industries/{slug}/index.md` | `Industries/semiconductors/index.md` |
| Research topic | `Research/{slug}/output.md` | `Research/datacenter-power/output.md` |
| Daily log | `Daily/YYYY-MM-DD.md` | `Daily/2026-04-10.md` |
| Meeting notes | `Meetings/YYYY-MM-DD-{slug}.md` | `Meetings/2026-04-10-earnings-prep.md` |
| Ideas | `ideas/YYYY-MM-DD-{slug}.md` | `ideas/2026-04-10-short-xyz.md` |

Use kebab-case for slugs. No spaces in filenames.

## Frontmatter

Every markdown file must have YAML frontmatter. Minimum fields by type:

**Company notes:**
```yaml
---
ticker: AAPL
last_updated: 2026-04-10
---
```

**Research output:**
```yaml
---
topic: Descriptive title
companies: [AAPL, MSFT, GOOGL]
last_updated: 2026-04-10
---
```

**Industry notes:**
```yaml
---
industry: Semiconductors
last_updated: 2026-04-10
---
```

**Thesis:**
```yaml
---
ticker: AAPL
thesis: Services re-rating
status: active | draft | closed
direction: long | short | neutral
last_updated: 2026-04-10
---
```

**Concept page:**
```yaml
---
concept: AI Capex Cycle
companies: [MSFT, GOOGL, AMZN]
industries: [cloud-computing, ai-infrastructure]
status: active
last_updated: 2026-04-10
---
```

## Document Structure

### Company notes.md
```markdown
# {TICKER} — {Company Name}

## Investment Thesis
One paragraph. What's the view, why, and what changes it.

## Key Metrics to Track
Table of KPIs the market watches.

## Recent Developments
Reverse chronological. Date-prefixed entries.

## Key Risks

## Open Questions

## See Also
- **Concepts:** [links to relevant concept pages]
- **Related Companies:** [links to related company notes]
- **Industry:** [link to industry notes]
```

### Research output.md
```markdown
# {Topic Title}

## Summary
3-5 bullet executive summary.

## Analysis
Main body. Use ## headers for sections.

## Investment Implications
Who wins, who loses, what to do.

## Sources
Numbered references.
```

### Concept page
```markdown
# {Concept Name}

## Current Synthesis
3-5 sentences. Where the theme stands today, the variant view,
what the market prices vs what we see.

## Key Data Points
Table of numbers that matter for this theme.

## Company Implications
### {TICKER}
How this concept affects the company. Link to notes.

## Evolution
Reverse-chronological log of how this thesis has changed.
- YYYY-MM-DD — What changed

## Open Questions
What we don't know. What would change the view.

## Sources
Numbered references.

## See Also
- **Companies:** [links]
- **Industries:** [links]
- **Related Concepts:** [links]
```

## Writing Style

- Lead with the insight, not the setup
- Numbers first: "Revenue grew 34% to $12.3B" not "The company reported strong revenue"
- Be specific: "Trades at 35x forward PE vs 5yr avg 45x" not "valuation is reasonable"
- State the variant view: what do we see that the market doesn't?
- No filler, no hedging, no "it remains to be seen"
- Use absolute dates: "2026-04-03", not "last week"

## When Updating Files

1. Update `last_updated` in frontmatter to today's date
2. Add new developments at the TOP of "Recent Developments" (reverse-chron)
3. Update the relevant index.md (Companies, Industries, or Research)
4. If a thesis changed, update the thesis file's `status` field

## Cross-Reference Format

Use standard markdown links (not Obsidian wikilinks). Use relative paths from the linking file.

**`## See Also` in notes.md files** (at bottom of company notes):
```markdown
## See Also
- **Concepts:** [AI Capex Cycle](../../Concepts/ai-capex-cycle.md) · [Cloud Margin Expansion](../../Concepts/cloud-margin-expansion.md)
- **Related Companies:** [MSFT](../MSFT/notes.md) · [GOOGL](../GOOGL/notes.md)
- **Industry:** [Cloud Computing](../../Industries/cloud-computing/notes.md)
```

**`## Related` in index.md files:**
```markdown
## Related
- **Companies:** [MSFT](../MSFT/notes.md) · [GOOGL](../GOOGL/notes.md)
- **Concepts:** [AI Capex Cycle](../../Concepts/ai-capex-cycle.md)
- **Industry:** [Cloud Computing](../../Industries/cloud-computing/notes.md)
```

**Bidirectional rule:** When company A links to concept X, concept X must link back to company A. When adding a link in one direction, always add the reverse.

## Log Conventions

Single file at repo root: `log.md`. Append-only, newest at bottom.

**Entry format:**
```markdown
## [YYYY-MM-DD] verb | Subject
Description of what happened. List of pages touched.
```

**Verbs:** `ingest`, `update`, `query`, `lint`, `create`

**Archive:** When `log.md` exceeds 2,000 lines, rename to `log-{year}.md` and start a fresh `log.md`.

## Concept Page Conventions

- **Naming:** kebab-case (`ai-capex-cycle.md`, not `AI Capex Cycle.md`)
- **Required frontmatter:** `concept`, `companies`, `industries`, `status`, `last_updated`
- **Section order:** Current Synthesis -> Key Data Points -> Company Implications -> Evolution -> Open Questions -> Sources -> See Also
- **Evolution section:** Reverse-chronological, date-prefixed entries recording how the thesis has changed over time
- **When to create:** When a theme appears in 3+ company notes, or when a query answer deserves to be preserved as reusable knowledge
- **When to update:** Whenever new information affects the theme — do not let concept pages go stale while company notes advance

## Output Formats by Task

| Task | Output location | Format |
|------|----------------|--------|
| Earnings analysis | `Companies/{TICKER}/notes.md` | Update Recent Developments |
| Industry scan | `Industries/{slug}/notes.md` | Append to Recent Developments |
| Research deep-dive | `Research/{topic}/output.md` | Full output.md with frontmatter |
| Daily briefing | `Daily/YYYY-MM-DD.md` | Structured daily log |
| Meeting prep | `Meetings/YYYY-MM-DD-{slug}.md` | Attendees, agenda, key points |
| Thesis update | `Companies/{TICKER}/{TICKER}-{slug}.md` | Update status, add evidence |
| Idea generation | `ideas/YYYY-MM-DD-{slug}.md` | Thesis, catalysts, risks, sizing |

## What Not To Do

- Don't create README.md files in subfolders
- Don't add comments explaining obvious things
- Don't duplicate data already in another file — cross-reference instead
- Don't commit binary files (PDFs, XLSX) to git — they belong in gitignored folders
- Don't overwrite notes.md — always append (prepend to Recent Developments)
- Don't use relative dates in files ("last week") — use absolute dates ("2026-04-03")
- Don't create a concept page for a theme that only appears in 1-2 company notes
- Don't leave one-directional links — always make them bidirectional
- Don't forget to update `last_updated` when touching a file
- Don't forget to append to `log.md` after any wiki operation
