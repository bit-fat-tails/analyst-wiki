# Alerts

Alert outputs — news, filings, broker notes, and other time-sensitive signals.

**File naming:** `YYYY-MM-DD-{slug}.md` (e.g., `2026-05-01-tsmc-cowos-capacity-expansion.md`)

**Typical content:**
- Source and date
- Key points (what happened)
- Companies affected
- Thesis impact (supportive, challenging, or neutral — and why)

**Role in the wiki:** Alerts are a primary ingest source. When a new alert arrives, Claude should process it through the full ingest workflow: read the alert, update affected company notes, update concept pages, add cross-references, and log it.

**Tip:** Set up automated alert pipelines (email filters, news scrapers, broker feeds) that drop markdown files into this folder. Then periodically ask Claude to ingest any unprocessed alerts.
