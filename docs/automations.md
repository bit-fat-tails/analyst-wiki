# Automations

The wiki becomes dramatically more useful when you automate the routine
ingestion and monitoring work. This document describes the automations a
well-run analyst wiki should have, organized by cadence.

**The core principle:** You can outsource your thinking but you cannot
outsource your understanding. Every automation here outsources *thinking* —
scanning, summarizing, cross-referencing, flagging staleness. None of them
outsource *understanding* — forming conviction, identifying variant views,
making investment decisions. The automations surface information and organize
it. You build understanding from it.

If an automation starts telling you what to trade or how to feel about a
data point, it's crossed the line. Redesign it.

These automations can run on any agent platform that supports scheduled
execution: Claude Code background agents (`/schedule`), Codex automations,
cron jobs calling an LLM API, or even calendar-triggered scripts. The exact
implementation depends on your setup — what matters is the coverage and rhythm.

## Tiers

Not all automations are equally important. Build them in order:

The automations that deliver daily value from week one:
- Morning Briefing
- News Monitors (AM + PM)
- Email/Research Extractor
- Weekly Review
- Earnings Tracker
- Memory/Context Update

Six automations. That's all you need. Resist the urge to build more on
day one — an unreliable automation is worse than no automation because you
stop trusting it and start doing manual checks.

---

## The Daily Cycle

A production analyst wiki runs 3 automated passes per day. Each one has a
specific job and writes to a specific place in the wiki.

```
  06:00-07:00  ┌─────────────────────┐
  (pre-market) │ Morning Briefing    │──► Daily/YYYY-MM-DD.md
               └─────────────────────┘

  09:00-10:00  ┌─────────────────────┐
  (open)       │ News Monitor AM     │──► Alerts/YYYY-MM-DD-*.md
               └─────────────────────┘

  10:00-11:00  ┌─────────────────────┐
               │ Email/Research      │──► Daily/YYYY-MM-DD-email-research/
               │ Extractor           │    + Companies/*/notes.md updates
               └─────────────────────┘

  14:00-15:00  ┌─────────────────────┐
  (afternoon)  │ News Monitor PM     │──► Alerts/YYYY-MM-DD-*.md
               └─────────────────────┘    (delta from morning only)
```

### Morning Briefing

**When:** Pre-market, every day (adjust for your timezone and market hours)

**What it does:** Produces a concise daily briefing covering overnight news,
calendar commitments, market moves, and near-term events for your coverage
universe.

**Output:** `Daily/YYYY-MM-DD.md`

**Structure:**
- Executive Read (3-5 bullets — what changed and why it matters)
- Priority Actions (what to do today)
- Calendar (meetings and events)
- Market & News Signals (material moves by ticker)
- Watchlist / Questions (open items to monitor)

**Data sources:** News APIs, financial data providers, calendar, internal
messaging (Slack, email), market data.

**Key principle:** This is a decision-oriented memo, not a news dump. Each
item should answer "so what?" for your portfolio.

### News Monitors (AM + PM)

**When:** Mid-morning and mid-afternoon, every day

**What they do:** Scan news feeds for material developments affecting your
coverage universe. Score each item for materiality. Create alert artifacts
only for items that could affect a thesis or require action.

**Output:** `Alerts/YYYY-MM-DD-{slug}.md` — one file per material event

**Key principles:**
- Merge multiple articles about the same event into one artifact
- The PM monitor is a delta from the morning, not a full-day replay
- Each artifact should include: what happened, why it matters, tickers
  affected, action/monitoring cue, sources
- Skip low-materiality coverage unless it changes a thesis

### Email/Research Extractor

**When:** Late morning, every day

**What it does:** Scans incoming email for broker research, newsletters,
and other material insights. Extracts key points and — when durable enough —
updates the relevant company notes directly.

**Output:**
- Report in `Daily/YYYY-MM-DD-email-research-extractor/`
- Direct updates to `Companies/*/notes.md` when warranted

**Key principles:**
- Prioritize rating/PT changes, estimate revisions, thesis-changing data,
  and differentiated channel checks
- Merge multiple broker notes making the same call into one bullet
- Distinguish between "notes-worthy" updates (go into company notes) and
  "research queue" items (worth following up but not yet durable)

---

## The Weekly Cycle

Weekly automations handle synthesis and earnings prep.

```
  Monday AM    ┌─────────────────────┐
               │ Weekly Review       │──► Weekly/YYYY-WNN.md
               └─────────────────────┘

  Monday AM    ┌─────────────────────┐
               │ Earnings Tracker    │──► earnings/ or reports/
               └─────────────────────┘
```

### Weekly Review

**When:** Monday morning

**What it does:** Creates a strategic weekly summary that synthesizes the
week's activity into portfolio-level themes. This is the automation that
keeps your Concepts/ pages honest.

**Output:** `Weekly/YYYY-WNN.md`

**Structure:**
- Strategic Read (5 bullets max — what shifted in your thinking)
- Theme Scorecard (table: theme, evidence this week, confidence change)
- Company Thesis Changes (only where conviction changed)
- Research Health (stale notes, missing prep)
- Next Week Priorities (ranked action list)

**Key principle:** This should read like a portfolio agenda, not a digest
archive. Merge daily alerts into weekly themes. State what changed in your
conviction, not what happened in the news.

### Earnings Tracker

**When:** Monday morning (or more frequently during earnings season)

**What it does:** Tracks upcoming earnings dates for your coverage universe
and flags prep gaps — companies reporting soon where your notes are stale or
missing key context.

**Output:** Report with upcoming calendar, prep gaps, and schedule changes

**Key principles:**
- Focus on prep gaps, not just dates
- Flag companies where notes haven't been updated since last earnings
- Include adjacent peers that might move your companies on sympathy
- Increase frequency to daily during peak earnings season

---

## Maintenance Automations

Less frequent automations that keep the infrastructure healthy.

### Memory/Context Update

**When:** Weekly (Sunday evening works well)

**What it does:** Updates long-term context artifacts — glossary terms, people
profiles, and cached context — from recent work. Keeps your LLM's persistent
memory current without manual curation.

---

## Setting Up Automations

### With Claude Code

Use `/schedule` to create recurring background agents:

```
/schedule "Run the morning briefing" every weekday at 7am
/schedule "Monitor news for my coverage" every weekday at 10am and 3pm
/schedule "Weekly review and wiki lint" every Monday at 9am
```

### With Codex Automations

Create `automation.toml` files in your Codex automations directory with
cron-style scheduling (RRULE format), a prompt describing the task, and
the wiki directory as the working directory.

### With Custom Scripts

Any scheduling system works. The key elements:
1. A prompt that describes the task and output contract
2. A schedule (cron, systemd timer, cloud scheduler)
3. The wiki directory as context
4. An LLM with file read/write access

---

## Design Principles for Automations

**1. Each automation writes to a specific place.**
Morning briefing → `Daily/`. News monitor → `Alerts/`. Weekly review →
`Weekly/`. No automation should scatter output across random locations.

**2. Deltas, not replays.**
The PM news monitor only covers what changed since the AM monitor. Don't
restate the same fact in every automation.

**3. Decision-oriented, not comprehensive.**
Every item should answer "so what?" for your portfolio. A company's stock
moving 2% on no news is not an alert. A supplier announcing a capacity cut
that affects your thesis is.

**4. Merge overlapping signals.**
If three broker notes say the same thing about TSMC capex, that's one bullet
with three source citations, not three separate alerts.

**5. Fail gracefully and visibly.**
Every automation should log failures to a known location. Silent failures
are the worst kind — you stop trusting the system and start doing manual
checks, which defeats the purpose.

**6. Start with 2-3, then expand.**
Start with:
1. Morning briefing (highest daily value)
2. News monitor (keeps alerts flowing)
3. Weekly review (forces synthesis)

Add the email extractor and earnings tracker once those are solid.

---

## Automation Dependency on the Wiki

The automations and the wiki reinforce each other:

```
  Automations generate ──► Alerts, Daily, Weekly files
                              │
                              ▼
  Ingest workflow absorbs ──► Company notes, Concept pages updated
                              │
                              ▼
  Wiki gets richer ─────────► Better context for next automation run
                              │
                              ▼
  Automations get smarter ──► More relevant alerts, sharper briefings
```

This is the compounding loop. The morning briefing is better when the wiki
has rich company notes. The weekly review is more useful when the log records
every ingest. Each piece makes the others better.
