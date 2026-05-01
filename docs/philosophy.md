# Philosophy

## Why This Exists

If you've been an equity analyst for more than one earnings season, you know the
feeling. A company reports. You need context on their pricing strategy, their
competitive position, what management said last quarter about margins. You know
you've read this before -- in a broker note, in a transcript, in a Slack thread
three months ago. But where?

So you re-derive it. You search your email for the Morgan Stanley note. You
pull up the transcript on the broker portal. You skim 40 pages to find the two
paragraphs that matter. You piece together the context you need, answer the
question, and move on.

Next quarter, you do it again.

This is the fundamental problem with how most analysts manage knowledge. It
doesn't compound. Every session starts roughly from scratch, with you as the
retrieval engine, pulling fragments from scattered sources and reassembling
them in your head. The analysis you did three months ago -- the connections you
drew, the contradictions you spotted, the variant view you articulated -- lives
in a chat history that scrolled off screen, or in a note you can't find, or
nowhere at all.

Analyst Wiki is a system designed to fix this. It's a markdown workspace where
an LLM agent maintains a persistent, interlinked knowledge base on your behalf.
You feed it sources. It does the filing, cross-referencing, and bookkeeping.
Knowledge accumulates. Each source makes every future query better.

---

## The Problem with Analyst Workflows

Let's be specific about where knowledge lives for a typical equity analyst:

**Broker portals.** The primary source of sell-side research. PDFs organized by
date, maybe by ticker if the portal is good. No cross-referencing. No synthesis
across brokers. To find what Goldman and Morgan Stanley both said about TSMC
capex, you open two PDFs and Ctrl+F.

**Bloomberg / FactSet / Capital IQ.** Good for structured data. Terrible for
qualitative context. The terminal doesn't know that management guided capex
above consensus because they're seeing "unprecedented demand" for CoWoS
packaging. That context lives in a transcript you read once.

**Email and Slack.** Where real-time analysis happens -- a colleague flags a
data point, you riff on it, you form a view. Then it scrolls away. Six weeks
later, neither of you can find the thread.

**Personal notes.** Obsidian, Notion, Apple Notes, a text file on the desktop.
Every analyst has a system, and every system has the same problem: you have to
do the writing, the linking, and the maintenance yourself. The moment you get
busy -- which is always -- the notes fall behind.

**Your head.** The most important and least durable storage medium. You carry
an enormous amount of context about your coverage universe. Connections between
companies, where consensus is wrong, what data points to watch. None of it is
written down in a way that survives you having a bad week, or going on
vacation, or covering a new sector.

The common thread: knowledge is scattered, unsynthesized, and decaying. The
analyst is the integration layer, re-deriving context on demand from fragmented
sources. This works, but it doesn't scale, and it doesn't compound.

---

## Why Not RAG?

The obvious objection: "Can't I just upload my documents to ChatGPT / NotebookLM
/ a vector database and search them?"

You can. It's better than nothing. But RAG (Retrieval-Augmented Generation) has
a fundamental limitation for research workflows: it retrieves but doesn't
synthesize.

Here's what RAG does: you ask a question, the system finds the most relevant
chunks from your documents, and the LLM generates an answer from those chunks.
Each query is independent. There's no persistent state. The system doesn't
accumulate understanding over time.

What this means in practice:

**No cross-referencing.** RAG doesn't know that the TSMC capex revision you
asked about on Monday is connected to the NVDA supply constraint you asked
about on Thursday. Each query retrieves chunks in isolation.

**No contradiction detection.** Two broker notes may have conflicting estimates
for the same metric. RAG will happily retrieve both and present them without
flagging the inconsistency. A maintained wiki has a single Current Synthesis
that must reconcile conflicting inputs.

**No accumulated context.** Every query pays the full cost of retrieval and
synthesis. The tenth time you ask about HBM demand, the system does the same
work as the first time. A wiki has a concept page that was built incrementally
over ten ingest sessions and is ready to read immediately.

**No variant view tracking.** RAG doesn't know what the market prices versus
what the data says. It doesn't know that your view diverges from consensus on
a specific dimension. A maintained concept page articulates this explicitly.

**No link structure.** The connections between topics -- which companies are
exposed to which themes, which risks are correlated, which catalysts affect
multiple positions -- exist only in the analyst's head in a RAG system. In a
wiki, they're explicit markdown links that the LLM can follow.

RAG is a retrieval tool. The wiki is a knowledge artifact. They solve different
problems. (You can use RAG tools alongside the wiki -- searching your raw
document corpus to find sources for ingest. The wiki is what you build from
those sources.)

---

## The Wiki as a Persistent Artifact

The core idea is simple: instead of deriving analysis on the fly from raw
sources, compile it once and keep it current.

When a new broker note arrives on AAPL services revenue, you don't just read it
and form an opinion. The LLM reads it and does five things:

1. Updates `Companies/AAPL/notes.md` with the new data point
2. Updates `Concepts/app-store-regulation.md` because the note touches
   regulatory risk
3. Checks if GOOGL or META notes should be cross-referenced
4. Updates `Industries/consumer-electronics/notes.md` if there's sector impact
5. Logs the activity

Each of these updates is incremental. The LLM reads the existing page, decides
what changed, and revises it. The concept page doesn't get rewritten from
scratch -- it gets a new data point added to Key Data Points, a sentence
revised in Current Synthesis, and a dated entry in the Evolution log.

Over time, the concept page accumulates a rich, cross-referenced view of the
theme. It knows which companies are exposed, what the key data points are, how
the narrative has evolved, and where the open questions remain.

When you need to answer "What's our exposure to EU tech regulation?", the wiki
already has the answer compiled. The LLM reads the concept page and follows the
links. The answer draws on every source you've ingested over months, not just
whatever a search engine retrieves.

This is the compounding: each source doesn't just inform one answer. It makes
the entire wiki incrementally better, which makes every future query
incrementally cheaper and richer.

---

## The Analyst's Job vs the LLM's Job

This system works because there's a clean division of labor.

**The analyst's job:**

- Curate sources. You decide what's worth ingesting -- which broker notes
  matter, which news is signal vs noise, which data points to track.
- Direct analysis. You ask the questions. "What's the variant view on TSMC
  capex?" "Where does consensus underestimate HBM demand?" "If EU regulation
  hits App Store take rates, what's the AAPL earnings impact?"
- Spot patterns. The LLM is good at filing and cross-referencing, but you're
  the one who notices that three seemingly unrelated data points imply
  something the market hasn't priced.
- Make investment decisions. The wiki informs. You decide.

**The LLM's job:**

- Summarize sources. Extract key points from broker notes, transcripts, and
  articles.
- Cross-reference. When a new data point arrives, find every page it connects
  to and update the links.
- File consistently. Every ingest follows the same pattern -- update notes,
  update concepts, update index, update log.
- Flag inconsistencies. When a new data point contradicts existing synthesis,
  note the contradiction in the concept page.
- Maintain structure. Keep index files current, keep frontmatter accurate,
  keep links bidirectional.

The analyst is the portfolio manager. The LLM is the research associate who
never sleeps, never forgets to file something, and never lets a cross-reference
go stale.

---

## Concept Pages Are the Alpha

If you take one thing from this document, take this: company notes are table
stakes. Concept pages are where the compounding happens.

A company note on NVDA tells you what's happening with one company. A concept
page on "HBM Supercycle" tells you how NVDA, MU, SK Hynix, TSMC, and Samsung
are all connected through a single demand driver -- what the current supply/
demand picture looks like, where consensus estimates sit, how the narrative has
evolved over the past year, and where the market might be wrong.

That synthesis across companies is the hard work of equity research. It's where
variant views live. It's what differentiates an analyst who covers 30 names
from an analyst who understands 30 names.

And it's exactly the kind of work that falls through the cracks in traditional
workflows. You do the synthesis in your head, on a call with your PM, in a
Slack thread that disappears. Rarely does it get written down in a durable,
cross-referenced form that you can revisit and build on.

The wiki solves this by making concept pages first-class citizens. Every
ingest considers which concepts are affected. Every concept page has explicit
links to the companies it touches. Every concept page has a Current Synthesis
section that articulates the view, and an Evolution section that tracks how the
view has changed over time.

When you disagree with consensus, that disagreement should be written down in a
concept page. When new data arrives, the concept page gets updated, and you can
see whether the data confirms or challenges your view. This is
Mauboussin's Expectations Investing framework made operational: the wiki
tracks what the market prices vs what the data says, company by company, theme
by theme.

---

## Why Markdown and Git

Proprietary note-taking tools come and go. Your research shouldn't depend on a
company's pricing decisions or product roadmap.

Markdown is plain text. It works with every editor, every LLM, every operating
system. There's no import/export friction. No API to authenticate against. No
subscription to maintain.

Git gives you version history for free. Every commit is a snapshot of the entire
wiki. Diffs show exactly what changed -- which is enormously useful for
tracking how your thinking evolved. "What did I think about TSMC capex six
months ago?" is answerable by looking at the git log for
`Concepts/tsmc-capex.md`.

Git also gives you branching for free. Exploring a speculative thesis? Branch
the wiki, let the LLM explore it, and merge it back if the thesis holds up.

And git gives you collaboration for free. If your team shares the wiki repo,
pull requests become a mechanism for reviewing research updates. "Claude thinks
this broker note changes our HBM view -- review the diff and approve."

There's no magic in the tooling. The magic is in the pattern: a persistent,
interlinked, incrementally-maintained knowledge base. The format is plain text
because plain text is the most durable and flexible format there is.

---

## Outsource Thinking, Not Understanding

There is a line that runs through every design decision in this system,
and it comes from a simple observation:

> *"You can outsource your thinking but you cannot outsource your understanding."*

Thinking is mechanical. Summarizing a 40-page broker note. Cross-referencing
a data point across five company pages. Checking which concept pages need
updating. Scanning for stale notes. Formatting a table of consensus
estimates. These are tasks that require intelligence but not judgment. The
LLM does them better, faster, and more consistently than you.

Understanding is not mechanical. Understanding is knowing that the TSMC
capex number matters because it implies a supply bottleneck that the Street
hasn't modeled. Understanding is recognizing that three unrelated data
points — a management comment, a hiring pattern, and a channel check — all
point to the same thesis break. Understanding is having a view that differs
from consensus and knowing exactly why.

The wiki is designed to make your understanding deeper, not to replace it.
Every automation, every ingest, every concept page exists to surface
information and organize it so that *you* can build understanding from it.
The system never makes the investment decision. It never tells you what to
think. It tells you what the data says, what has changed, where the
contradictions are, and what you haven't looked at recently. Then you decide.

This is why the automations are designed the way they are:
- The **morning briefing** surfaces what happened overnight. It does not tell
  you what to trade.
- The **news monitor** scores materiality and creates alerts. It does not
  interpret the alert for your portfolio.
- The **weekly review** synthesizes themes and flags confidence changes. It
  does not recommend position changes.
- **Concept pages** articulate the variant view. But the variant view is
  yours — the LLM compiles the evidence; you form the conviction.

If you find yourself reading wiki outputs without engaging critically — just
accepting the synthesis, forwarding the briefing, acting on the alerts without
your own judgment — the system is failing you. It's doing its job (thinking)
but you're abdicating yours (understanding).

The best use of this system: you spend *less* time on filing and
cross-referencing and *more* time on the hard questions. What does the market
price for this company? Where is the consensus wrong? What would change my
view? The wiki gives you the foundation to think about those questions deeply.
It cannot answer them for you.

---

## How to Think About This System

The wiki is not a product. It's a pattern. Fork it, change the folder
structure, add sections to the templates, rip out the parts you don't need.
What matters is the three operations (ingest, query, lint) and the principle
that knowledge should be compiled and maintained, not re-derived from raw
sources on every query.

If you're an equity analyst, this gives you a system that grows more useful
with every source you feed it. If you're a different kind of researcher --
macro, credit, VC due diligence -- the same pattern applies. Change the
templates, change the folder names, keep the workflow.

The goal is simple: stop re-deriving. Start compounding.
