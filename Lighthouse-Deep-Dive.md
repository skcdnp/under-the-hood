# Monarch-Lighthouse 🏛️

> A personal financial intelligence platform that connects macro conditions, breaking news, and your actual stock portfolio — powered by a local AI, running entirely on your machine.

---

## What It Is

Monarch-Lighthouse is a locally-run dashboard that does one thing well: it tells you what the market is doing, why it might be doing it, and what it means for the stocks you own — without requiring a Bloomberg terminal, a cloud subscription, or hours of manual research.

Open it in the morning and you see a live macro snapshot alongside an AI-generated interpretation of what those signals mean. Scroll down and you see portfolio cards — one per stock you track — each with a concise, AI-written insight that connects recent company news to the broader market environment. Refresh mid-day and only genuinely new information triggers a new insight. If nothing material has changed, the platform tells you that too.

[![Monarch-Lighthouse Dashboard](docs/screenshot.png)](https://github.com/skcdnp/under-the-hood/blob/main/docs/Screenshot.png)
*Macro Dashboard and Portfolio Intelligence cards — running locally on a single machine*

---

## Why I Built It

Financial news is abundant. What's missing is interpretation that's tied to *your* portfolio.

Most retail investors either drown in noise — scanning five different news tabs, manually checking macro data, trying to mentally connect the dots — or they rely on generic market commentary that has nothing to do with the stocks they actually hold. The tools that do offer personalised, intelligent analysis (Bloomberg, Refinitiv) cost thousands of dollars a month and are built for institutional desks.

I wanted something in between: lightweight, local, free to run, and genuinely intelligent about my specific holdings. Something that feels less like a data terminal and more like a knowledgeable analyst you can check in with at the start of the day.

There was nothing like that available. So I built it.

---

## How It Works

### Three Views, One Coherent Picture

**Macro Dashboard**
Pulls live data from trusted public sources — CPI, Fed Funds Rate, GDP, yield curve spread, unemployment. Below the numbers, an AI-generated paragraph interprets what these signals collectively suggest. Not raw data — a readable hypothesis. *"Elevated CPI against a 3.64% Fed Funds rate suggests markets are pricing in a prolonged higher-for-longer environment..."*

**Market Headlines** *(coming in Phase 2)*
A deduplicated, thematically grouped digest of the day's most significant financial stories — not a raw chronological feed. Each theme gets a one-line summary. Duplicate stories from different publishers are collapsed into one.

**Portfolio Intelligence**
One card per stock you track (up to 10 tickers). Each card connects recent company-specific news with the current macro context to produce a directional framing — not a buy/sell signal, but a clear *"here is how to think about this right now."* Cards show when they were last generated, and only update when something material changes.

---

## Design Choices

**Local-first, no cloud required.**
Everything runs on your machine. The AI model, the database, the data collectors — all local. No data leaves your laptop. No monthly fees.

**Model-agnostic AI layer.**
The insight engine is fully decoupled from the model running behind it. Out of the box it runs against a local LLM via Ollama. Swapping to Claude or GPT-4 is a single line in a config file. The prompts are plain text files — tuning the tone or analytical posture means editing a template, nothing else.

**Materiality-gated AI calls.**
The system doesn't call the AI every time you refresh. It compares the current data against what was used to generate the last insight. If nothing material has changed — no new Tier 1 news, no new filings, no macro releases — it serves the prior insight and tells you so. This keeps things fast, avoids redundancy, and prevents the AI from rephrasing the same story three times across a day.

**Graceful degradation.**
No data source failure crashes the app. If Reuters' RSS is down, the platform continues with available sources and labels the gap clearly. If the AI model is unreachable, the prior insight renders with a warning banner. The user always sees something useful.

**Transparent data provenance.**
Every insight shows what data it was based on and when that data was last refreshed. Nothing is presented without a visible timestamp and source attribution.

---

## Architecture Overview

```
┌──────────────────────────────────────────┐
│              User Interface              │
│   Macro Dashboard | Portfolio Cards      │
└────────────────────┬─────────────────────┘
                     │
┌────────────────────▼─────────────────────┐
│            Insight API Layer             │
│   Serves insights · Orchestrates refresh │
└──────┬─────────────────────┬─────────────┘
       │                     │
┌──────▼──────────┐   ┌──────▼──────────────┐
│ Materiality     │   │  Insight Generator  │
│ Engine          │──▶│                     │
│                 │   │  Prompt templates   │
│ "Has anything   │   │  Swappable LLM      │
│  material       │   │  adapter            │
│  changed?"      │   │                     │
└──────┬──────────┘   └─────────────────────┘
       │
┌──────▼──────────────────────────────────┐
│           Data Store (SQLite)           │
│  Articles · Macro snapshots · Insights  │
│  News ledger · Portfolio · Source state │
└──────────────────┬──────────────────────┘
                   │
┌──────────────────▼──────────────────────┐
│         Data Collector Layer            │
│                                         │
│  RSS · Finnhub · FRED · BLS · EDGAR    │
│  Alpha Vantage · yfinance · BEA        │
│                                         │
│  Each collector is independent.         │
│  Owns its own rate limits and retries.  │
└──────────────────┬──────────────────────┘
                   │
         Free public data sources
   (Reuters, FRED, Finnhub, EDGAR, BLS...)
```

The key insight in the architecture is the **Materiality Engine** sitting between the data layer and the AI layer. Rather than calling the LLM on every refresh, it hashes the input data and compares it against what was used to generate the last insight. The AI is only invoked when something genuinely new has arrived — a new Tier 1 news article, a fresh SEC filing, a scheduled macro release. This keeps the system fast and non-repetitive.

---

## Data Sources

All data sources used in Phase 1 are free or freemium — no paid subscriptions required.

| Source | What It Provides |
|---|---|
| FRED API | CPI, GDP, Fed Funds Rate, Unemployment, Yield Curve |
| Federal Reserve RSS | FOMC decisions and press releases |
| BLS API | Non-Farm Payrolls, CPI releases |
| BEA RSS | GDP and PCE releases |
| Reuters RSS | Global financial news |
| AP News RSS | Breaking business headlines |
| CNBC RSS | US equity market headlines |
| Finnhub API | Per-ticker news with entity tagging |
| SEC EDGAR RSS | 8-K filings and insider transactions (Form 4) |
| yfinance | Earnings calendar and historical price data |
| GDELT DOC 2.0 | Geopolitical signals, global news coverage |

Source credibility is tiered: government agencies and central banks are Tier 0 (ground truth); major financial publishers are Tier 1; structured data vendors are Tier 2. The materiality engine only triggers AI regeneration on Tier 0 and Tier 1 signals.

---

## Build Phases

The platform is being built incrementally. Each phase delivers a complete, working slice.

| Phase | Focus | Status |
|---|---|---|
| **Phase 1** | Macro dashboard · FRED/BLS/Fed collectors · Portfolio cards · Local LLM via Ollama | Complete |
| **Phase 2** | Full news pipeline · RSS collectors · Deduplication · Thematic headline grouping | Complete |
| **Phase 3** | EDGAR insider signals · Cloud LLM support (Claude, GPT-4) · Enhanced UI | Complete |
| **Phase 4** | Additional central banks · EIA energy data · Social sentiment · Multi-user support | Future |

---

## What It's Not

To be clear about scope:

- **Not a trading system.** No buy/sell signals, no order routing, no automated execution.
- **Not a cloud product.** Runs locally. No remote access, no hosted version in Phase 1.
- **Not a news aggregator.** The goal is interpretation, not volume.
- **Not a paid tool.** Built entirely on free public data sources.

---

## Philosophy

This platform is designed to be built incrementally and improved through use. Phase 1 establishes the full end-to-end loop. Subsequent phases add data richness, UI polish, and intelligence depth — but the core architecture doesn't change. Every decision in Phase 1 is made to make Phase 2 easier, not harder.

---

*Built in March 2026. Work in progress.*
