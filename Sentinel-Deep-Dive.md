# 👑 Sentinel

> **An AI-assisted intraday trading system built from scratch — spec, strategy, architecture, and code.**

![Status](https://img.shields.io/badge/Status-Live%20%26%20Trading-brightgreen?style=for-the-badge)
![Stack](https://img.shields.io/badge/Stack-FastAPI%20%2B%20React%20%2B%20SQLite-blue?style=for-the-badge)
![Data](https://img.shields.io/badge/Data-Alpaca%20Markets-orange?style=for-the-badge)
![Phase](https://img.shields.io/badge/Phase-1%20Complete-purple?style=for-the-badge)

---

## 🧭 What Is This?

Sentinel is a **local, desktop-grade intraday trading assistant** for US equities. It monitors a live data feed, evaluates rule-based strategies on confirmed 1-minute bar closes, surfaces actionable entry and exit signals, and logs every trade decision for review and analysis.

This is not a template, tutorial, or SaaS product. It is a **fully custom system** conceived, architected, and built end-to-end — from blank page to live trading session — as a demonstration of what it looks like when product thinking, systems design, and technical execution are applied together at full depth.

---

## 🎯 Why I Built It

Most retail trading tools are either black boxes (you don't know what triggered the signal) or raw data feeds (you know everything but have to do all the work). Monarch Sentinel occupies the gap: a **transparent, rule-based assistant** that shows its reasoning at every step, enforces risk discipline automatically, and keeps the human in control of every execution decision.

The secondary goal was deliberate: **build something real enough to trade with**, not a prototype. That forced every design decision to have integrity — data accuracy, state machine correctness, edge case handling, and user experience under pressure.

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────┐
│                   MONARCH SENTINEL                  │
│                                                     │
│  ┌──────────┐    ┌──────────────┐    ┌───────────┐  │
│  │  Alpaca  │───▶│  Bar Builder │───▶│ Strategy  │  │
│  │ WebSocket│    │  (1m + 5m)   │    │  Engine   │  │
│  └──────────┘    └──────────────┘    └─────┬─────┘  │
│                                            │        │
│  ┌─────────────────────────────────────────▼──────┐ │
│  │              SQLite Database                   │ │
│  │  bars · bars_5m · signals · trades · watchlist │ │
│  └─────────────────────────────────────────┬──────┘ │
│                                            │        │
│  ┌─────────────────────────────────────────▼──────┐ │
│  │           FastAPI Backend (localhost:8000)     │ │
│  │    REST API + WebSocket broadcast manager      │ │
│  └─────────────────────────────────────────┬──────┘ │
│                                            │        │
│  ┌─────────────────────────────────────────▼──────┐ │
│  │        React Frontend (localhost:3000)         │ │
│  │   Watchlist · Charts · Signals · Trade Log     │ │
│  └────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────┘
```

**The system runs entirely on localhost.** No cloud dependency, no subscription, no data leaving your machine. `python main.py` + open browser.

---

## ⚙️ Tech Stack

| Layer | Technology | Why |
|-------|-----------|-----|
| **Backend** | Python 3.12 + FastAPI + asyncio | Quant finance ecosystem, async-native for WebSocket |
| **Frontend** | React + TypeScript + Vite + Tailwind | Type-safe signal objects, fast dev iteration |
| **Charting** | TradingView Lightweight Charts v4 | Purpose-built for OHLCV, full overlay control |
| **Database** | SQLite (WAL mode) | Single local file, zero config, sufficient for 5–8 tickers |
| **Data Feed** | Alpaca Markets WebSocket (`alpaca-py`) | Real-time 1-minute bars, REST historical backfill |
| **State** | Zustand (frontend) | Lightweight, no boilerplate, selector-friendly |
| **Styling** | Tailwind CSS + Material Symbols | Dark trading UI with custom brand tokens |

---

## 📐 What Was Designed Before a Line Was Written

This project began with a complete written specification before any code was produced. The design phase produced:

### 1. System Specification
A 15-section product spec covering:
- User flow and session lifecycle state machine
- Signal card field definitions (entry + close)
- Chart overlay requirements
- Risk sizing formula
- Database schema design
- API contract (REST + WebSocket)
- Phase 1 deliverable checklist with explicit in/out-of-scope boundaries

### 2. Strategy Addendum
A separate technical document for the VWAP Reclaim + Retest strategy covering:
- Full 11-state machine per ticker with pseudocode
- ATR-anchored parameter design (self-calibrating across volatility regimes)
- Three-state market context classifier (FAVOURABLE / NEUTRAL / ADVERSE)
- Stand-down guardrails with composite recovery conditions
- Mandatory room-to-target filter (0.75R minimum)
- Session time-of-day confidence modifiers
- Deferred items explicitly listed by phase

### 3. Data Provider Analysis
A structured evaluation of Alpaca free tier vs paid tier vs IBKR vs E*TRADE vs Morgan Stanley — covering data quality, WebSocket limitations, historical bar delays, VWAP accuracy implications, and upgrade path decisions.

> **The design artifacts exist as living documents** — not post-hoc documentation, but the actual inputs that guided the build.

---

## 📊 Phase 1 Strategies

Custom build strategies built but foundation relied on thre more common frameworks.

 🔴 Opening Range Breakout (ORB)
 🟣 VWAP Reclaim + Retest (High-Selectivity)
 🟡 Trend Pullback Continuation

## 🧠 Core Design Decisions

### The Confirmed-Close Guarantee
Every strategy trigger — reclaim detection, hold confirmation, ORB breakout — fires only on **confirmed 1-minute bar closes**. The `confirmed` boolean flag in the bar store is the architectural enforcement mechanism. No intra-bar evaluation anywhere in the system.

### Dual-Timeframe Bar Pipeline
The bar builder maintains two streams simultaneously:
- **1-minute bars** — entry/hold confirmation, retest detection, VWAP computation
- **5-minute bars** — aggregated from confirmed 1m bars, used for reclaim detection and market context

Both streams share a single Alpaca WebSocket connection. The 5m bar fires on every 5th confirmed 1m bar.

### Signal Lifecycle State Machine
Each ticker runs an independent state machine:

```
INACTIVE → MONITORING → SIGNAL_ACTIVE → POSITION_OPEN → CLOSE_SIGNAL → CLOSED
                                    ↘ REJECTED (dismiss)
                                    ↘ EXPIRED (timeout)
```

One active strategy per ticker per session. No new entry while position is open. Signal cards are immediately removed if conditions invalidate before the user acts.


### Force-Close Safety Net
An asyncio background task fires at **15:45 ET** on every live session, closing all open positions at last bar close price. No overnight exposure by accident.

---

## 🗄️ Database Schema

```sql
-- Live bar data (1-minute)

-- 5-minute aggregated bars (for strategy bias detection)

-- Daily reference levels (loaded at session start)

-- Signal lifecycle

-- Trade execution log

-- Active watchlist

-- Strategy assignments per ticker

```

---

## 🔌 API Surface

### REST Endpoints
```
GET  /api/v1/watchlist               — List active tickers
POST /api/v1/watchlist               — Add ticker
DELETE /api/v1/watchlist/{ticker}    — Remove ticker

GET  /api/v1/bars/{ticker}           — Historical bars (1m or 5m)
GET  /api/v1/daily_levels/{ticker}   — PDH/PDL/PDC/PMH/PML

GET  /api/v1/signals                 — List signals (filter by status/ticker)
POST /api/v1/signals/{id}/act        — Execute entry (creates trade)
POST /api/v1/signals/{id}/dismiss    — Reject signal

GET  /api/v1/trades                  — Trade history
POST /api/v1/trades/{id}/close       — Close position (P&L computed server-side)

POST /api/v1/session/start           — Start live session
POST /api/v1/session/stop            — Stop session
GET  /api/v1/session/status          — Session state + market hours

GET  /api/v1/settings                — Load configuration
PUT  /api/v1/settings                — Save configuration
```

### WebSocket Events (`/ws`)
```json
{ "type": "BAR_UPDATE",            "data": { "ticker": "AAPL", "bar_time": "...", ... } }
{ "type": "BAR_5M_UPDATE",         "data": { "ticker": "AAPL", "bar_time": "...", ... } }
{ "type": "SIGNAL_NEW",            "data": { "id": 42, "ticker": "AAPL", ... } }
{ "type": "SIGNAL_INVALIDATED",    "data": { "id": 42, "reason": "..." } }
{ "type": "TICKER_STATE_CHANGED",  "data": { "ticker": "AAPL", "state": "POSITION_OPEN" } }
{ "type": "SESSION_STATE_CHANGED", "data": { "state": "LIVE" } }
```

---

## 🚦 Build Phases

| Wave | Feature | Status |
|------|---------|--------|
| Wave 0 | Shared Contracts (TypeScript types + Python Pydantic models + API spec) | ✅ Complete |
| Wave 1 | Bootstrap — FastAPI skeleton + React shell + mock data layer | ✅ Complete |
| Wave 2 | Database layer — all repositories + REST CRUD | ✅ Complete |
| Wave 3 | Data feed — Alpaca WebSocket + Mock provider + Bar Builder | ✅ Complete |
| Wave 4 | Strategy Engine + ORB strategy | ✅ Complete |
| Wave 5 | Signal lifecycle + Signal Panel UI | ✅ Complete |
| Wave 6 | VWAP Reclaim + Trend Pullback strategies + VWAP overlay | ✅ Complete |
| Wave 7 | Trade close flow + Force-close scheduler + Trade Log UI | ✅ Complete |
| Wave 8 | Trade Log screen (filterable history, stats) | ✅ Complete |
| Wave 9 | Session control — full lifecycle, provider wiring, auto-scheduler | ✅ Complete |
| Wave 10 | Chart overlays — retest zone, position marker, OR bands, key levels | ✅ Complete |
| Settings | Settings screen — risk params + provider credentials | ✅ Complete |
| UI Redesign | Full dark theme — brand tokens, Material Symbols, trading aesthetic | ✅ Complete |
| Live Debug | Post-live session bug fixes — bar streaming, signal recovery, page refresh | ✅ Complete |

---

## 💡 What This Project Demonstrates

This project was designed to show something specific: **what end-to-end product ownership looks like when applied to a complex technical domain**.

| Dimension | What's Here |
|-----------|------------|
| **Product Thinking** | A spec written before code — user flows, state machines, signal card fields, phase boundaries, deferred items explicitly called out |
| **Technical Depth** | Working implementation across full stack — WebSocket streaming, strategy evaluation loops, SQLite repositories, React state management |
| **Systems Design** | Dual-timeframe bar pipeline, confirmed-close guarantee, provider abstraction, force-close safety net |
| **Data Literacy** | Structured analysis of IEX vs SIP data quality, VWAP accuracy implications, API tier tradeoffs |
| **Decision Quality** | Every non-obvious decision is documented with rationale — in the spec, the addendum, and the build journal |
| **Shipping Discipline** | 14 waves from blank page to live trading session, with a live debug session that surfaced and fixed 6 layered failures in one sitting |

---


**Built by** a product leader who wanted to understand the full stack — not just the roadmap.

*Monarch Sentinel — because good signals deserve good infrastructure.*

</div>
