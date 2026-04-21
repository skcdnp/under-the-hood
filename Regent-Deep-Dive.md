

# 🤖 AI Crypto Trading POC

![Python](https://img.shields.io/badge/python-3.10+-blue.svg)
![Streamlit](https://img.shields.io/badge/ui-streamlit-red)
![Ethereum](https://img.shields.io/badge/network-sepolia-purple)
![Status](https://img.shields.io/badge/status-POC-orange)
![License](https://img.shields.io/badge/license-MIT-green)


# Regent Product Refrence Document 

**Product**: Monarch Regent  
**Version**: Testnet POC Baseline  
**Environment Assumption**: Deployed and operating on Ethereum Sepolia testnet only

## 1. Product Purpose
Regent is an AI-assisted crypto trading application that generates trade hypotheses, supports human approval, executes trades in a controlled workflow, and tracks outcomes for iterative improvement.

This High Level document is requirements-first and intentionally avoids deep architecture detail.

## 2. Product Goals
1. Prove end-to-end hypothesis-to-execution workflow on testnet without real capital risk.
2. Enforce strict risk controls before any trade execution.
3. Keep a complete decision and trade history for evaluation and auditability.
4. Provide an operator-friendly interface for monitoring and approvals.

## 3. Non-Goals (Current PRD Scope)
1. Mainnet trading with real capital.
2. Fully autonomous live trading without human oversight.
3. Advanced multi-asset portfolio optimization.
4. Cross-chain routing and CEX integration.

## 4. Target Users
1. Product operator/trader validating strategy hypotheses.
2. Technical reviewer monitoring system safety and quality.

## 5. Core User Flows
1. Operator opens dashboard and reviews market + system status.
2. Operator requests or waits for AI hypothesis generation.
3. Operator reviews hypothesis details and reasoning.
4. Operator approves or rejects the hypothesis.
5. Approved hypothesis is eligible for execution workflow.
6. Trade outcomes are captured and visible in history/performance views.

## 6. Functional Requirements
### FR-1 Market Snapshot Availability
1. System must provide current ETH price context, 24h directional context, gas price, wallet balance, and network indicator.
2. System must surface whether gas conditions are feasible for trading.

### FR-2 AI Hypothesis Generation
1. System must generate one structured hypothesis per cycle/request.
2. Hypothesis must include: direction, entry band, take-profit, stop-loss, confidence, reasoning, invalidation condition.
3. If gas feasibility fails, system must block actionable hypothesis generation.

### FR-3 Hypothesis Lifecycle Management
1. System must persist hypotheses with unique IDs and timestamps.
2. System must support status transitions at minimum: `active`, `approved`, `rejected`, `triggered`.
3. System must display active hypotheses with operator actions.

### FR-4 Human-in-the-Loop Controls
1. Operator must be able to explicitly approve or reject each hypothesis.
2. Approval/rejection actions must be persisted and auditable.

### FR-5 Execution Workflow (Testnet)
1. Approved and triggered hypotheses must result in a recorded execution event.
2. Each execution record must include linkage to source hypothesis.
3. Trade status and lifecycle must be visible in dashboard history.

### FR-6 Outcome and Performance Tracking
1. System must track trade records and P&L fields.
2. System must provide basic performance metrics (trade count, win rate, avg/total P&L).
3. System should feed recent outcomes back into subsequent hypothesis context.

### FR-7 Data Persistence
1. System must store hypotheses, trades, and outcomes in local persistent storage.
2. Persisted records must be queryable for dashboard display and review.

## 7. Risk and Safety Requirements
1. Gas gate must block trade flow when gas exceeds configured threshold.
2. Position size must be capped by configurable maximum.
3. Minimum expected profitability must account for gas-cost multiple.
4. Environment must remain testnet-only for this phase.
5. Secrets must be managed via environment variables, never hardcoded in tracked source files.

## 8. UX and Operational Requirements
1. Dashboard must surface real-time-ish status suitable for manual operation.
2. System must allow on-demand refresh and on-demand hypothesis generation.
3. Trade and hypothesis states must be understandable without reading logs.

## 9. Success Criteria (POC)
1. Hypotheses are generated and persisted with required fields.
2. Operator approval/rejection works reliably and updates state.
3. Triggered hypotheses produce linked trade records in testnet flow.
4. Dashboard shows market, hypotheses, trade history, and performance metrics.
5. Safety gates prevent out-of-policy execution.

## 10. Release Scope Statement
This PRD defines Regent as a **testnet validation product**. Mainnet readiness requires a separate PRD revision with expanded security, execution reliability, and governance requirements.


# Regent Implementation Status Reference
 
**Scope**: What is completed, in-progress, and intentionally deferred/phased-out for the current testnet PRD baseline.

## 1. Completed (Implemented)
1. Configuration layer for API/network/risk settings and validation.
2. Market data provider integration with gas checks and market snapshots.
3. AI hypothesis engine with structured JSON output expectations.
4. Hypothesis persistence, retrieval, and lifecycle status updates.
5. Streamlit dashboard for monitoring, hypothesis review, and operator actions.
6. Local SQLite persistence for hypotheses, trades, and outcomes tables.
7. Basic trade execution workflow and trade-history recording.
8. Performance summary display (trade count, win rate, avg/total P&L).
9. Testnet network targeting (Sepolia) as operating environment.

## 2. In Progress / Partially Implemented
1. Execution realism: current execution flow records trades but still uses simulated transaction values.
2. Live pricing depth: market pricing logic currently includes mocked components where DEX-grade sourcing was intended.
3. Learning loop hardening: outcome feedback exists, but full closed-loop optimization is not production-grade.
4. Comprehensive integration testing: component tests exist in workflow, but full automated end-to-end coverage is not complete.

## 3. Phased Out for This PRD Cycle
1. Mainnet deployment and real-capital execution.
2. Advanced risk gates beyond current gas/size/profit controls (e.g., robust slippage and volatility halts).
3. Backtesting infrastructure with formal historical strategy validation.
4. Multi-asset expansion and portfolio-level orchestration.
5. Hardware-wallet and institutional-grade signing controls.

## 4. Operational Readiness Statement
Regent is currently positioned as a **testnet POC validation system**. It demonstrates core product behavior and operator workflow, while intentionally deferring production-grade execution and governance requirements.





# Simplified Crypto Trading POC Implementation Plan

## Document Purpose & Audit Trail

**Created**: Feb 26, 2026
**Last Updated**: Feb 26, 2026
**Original Document**: POC Requirements.txt (457 lines, comprehensive production system)
**Simplification Goal**: Reduce complexity from 6-week production system to 1-week POC while preserving core innovation

**Decision Log**:
- **Why Simplify**: Original spec has 11 components, full risk management, real-time streaming - overkill for POC
- **Core Innovation to Preserve**: AI-generated trading hypotheses with reasoning
- **Primary Risk**: Oversimplifying may lose key value propositions
- **Mitigation**: Document all deferred features for future implementation
- **Testnet First** *(updated Feb 26, 2026)*: Switched target from Ethereum mainnet to Sepolia testnet. Full loop (hypothesis → execution → reconciliation) should be proven with no real capital before touching mainnet.
- **UI Layer Added** *(updated Feb 26, 2026)*: Added Streamlit dashboard as the interaction layer. Terminal output alone is too limited for monitoring hypotheses and approving trades. Streamlit is Python-native, requires one pip install, and keeps the stack simple.

---

## 1. Original Complexity Assessment

### 1.1 Original System Components (11 total)
| Component | File | Complexity | POC Necessity |
|-----------|------|------------|---------------|
| Web3 Provider | provider.py | High | ✅ Required |
| Data Ingestor | ingestor.py | High | ⚠️ Simplify |
| Feature Engine | features.py | Medium | ❌ Defer |
| Hypothesis Engine | hypothesis.py | High | ✅ Required |
| Risk Gate | risk.py | High | ⚠️ Simplify |
| Strategy Runner | strategy.py | Medium | ⚠️ Simplify |
| OMS/Executor | executor.py | High | ❌ Defer |
| Reconciler | reconciler.py | Medium | ❌ Defer |
| Learning Loop | learner.py | High | ⚠️ Simplify |
| CLI | trader.py | Medium | ✅ Required |
| DB Layer | db.py | Medium | ✅ Required |

### 1.2 Original Risk Gates (5 total)
| Gate | Implementation | POC Necessity |
|------|----------------|---------------|
| Gas Ceiling | Real-time monitoring | ✅ Keep |
| Slippage | Complex calculation | ❌ Defer |
| Balance | Wallet balance checks | ❌ Defer |
| Volatility Halt | 5min price change | ❌ Defer |
| Trade Cap | Position sizing | ⚠️ Simplify |

---

## 2. Simplified POC Architecture

### 2.1 Reduced Component Count (4 files instead of 11)

```
crypto-trader-poc/
├── config.py          # API keys, settings, risk thresholds
├── data.py            # Alchemy connection & basic price fetching
├── ai.py              # Claude API & hypothesis generation
├── trader.py          # Core logic, risk checks, trade execution (Sepolia)
├── app.py             # Streamlit UI — dashboard, hypothesis approval, trade history
└── trades.db          # SQLite database (3 tables only)
```

### 2.2 Simplified Database Schema

**Original**: 6 tables with complex relationships
**Simplified**: 3 tables focused on core functionality

```sql
-- hypotheses (id, asset, direction, entry_price, tp, sl, reasoning, created_at, status)
-- trades (id, hypothesis_id, entry_price, exit_price, pnl, status, created_at)  
-- outcomes (hypothesis_id, win_loss, pnl, accuracy_score, created_at)
```

---

## 3. Phase-Based Implementation Plan

### Phase 1: Foundation (1-2 days)
**Goal**: Basic data connection + AI hypothesis generation

#### 3.1.1 Core Components
- **config.py**: API keys, basic settings
- **data.py**: Alchemy connection, ETH/USDC price fetching
- **ai.py**: Claude API integration, hypothesis prompt engineering
- **trader.py**: Basic CLI, hypothesis display

#### 3.1.2 Simplified Features
- **Data Fetching**: Poll every 5 minutes (not WebSocket streaming)
- **Price Data**: Current price + 24h change only
- **AI Prompts**: Simple structured prompts (no complex feature engineering)
- **Risk Check**: Gas price < 30 gwei + smart gas management
- **Network**: Sepolia testnet (no real capital at risk)
- **Position Sizing**: 0.05 ETH max (Sepolia test ETH only)

#### 3.1.3 Deliverables
- Streamlit dashboard loads at `localhost:8501`
- Live ETH price and gas price visible on screen
- AI hypothesis appears with reasoning and confidence score
- Approve / Reject buttons for each hypothesis
- Hypothesis saved to SQLite on generation

### Phase 2: Testnet Trading (2-3 days)
**Goal**: Real transaction execution on Sepolia testnet with gas-aware risk management

#### 3.2.1 Core Components
- **Trade Executor**: Sepolia transactions via web3.py
- **Gas Monitor**: Real-time gas price tracking and trading windows
- **P&L Tracker**: Simulated profit/loss calculation including gas costs
- **Risk Manager**: Gas gate enforcement

#### 3.2.2 Simplified Features
- **Execution**: Real Sepolia transactions (test ETH, no real capital)
- **Gas Monitoring**: Real-time gas price tracking with trading windows
- **P&L Calculation**: (exit_price - entry_price) * size - gas_cost
- **Position Sizing**: 0.05 ETH max (test ETH only)
- **Risk Management**: 3x gas cost minimum profit requirement
- **UI**: Trade history and status visible in Streamlit dashboard

#### 3.2.3 Deliverables
- Approved hypotheses trigger real Sepolia transactions
- Trade status (pending / filled / failed) shown in dashboard
- P&L tracked per trade in SQLite
- Gas gate blocks trades when gas > 30 gwei

### Phase 3: Learning Loop (2-3 days)
**Goal**: Basic outcome feedback to improve AI hypotheses

#### 3.3.1 Core Components
- **Outcome Tracker**: Win/loss per hypothesis
- **Feedback Loop**: Send last 5 outcomes to Claude
- **Basic Dashboard**: Show hypothesis performance

#### 3.3.2 Simplified Features
- **Learning**: Basic pattern recognition (no machine learning)
- **Feedback**: Include recent outcomes in next AI prompt
- **Metrics**: Win rate, average P&L, confidence accuracy

#### 3.3.3 Deliverables
```
$ python trader.py learn

Recent Hypothesis Performance:
BUY hypotheses: 70% win rate, avg P&L: +$20
SELL hypotheses: 30% win rate, avg P&L: -$15

AI Feedback Integration:
✅ Last 5 outcomes included in next hypothesis generation
✅ Confidence scoring calibration based on actual results
```

---

## 4. Key Features

### 4.1 Gas Management Strategy
- **Max Gas**: 30 gwei threshold
- **Position Size**: 0.05 ETH max (test ETH on Sepolia)
- **Profit Requirement**: 3x gas cost minimum
- **Trading Windows**: Monitor for low gas periods

### 4.2 AI Hypothesis Engine
- **Claude API**: Haiku model for cost efficiency
- **Input Signals**: Current price, 24h change %, gas price
- **Structured Output**: JSON format with direction, entry, TP, SL, reasoning, confidence
- **Learning Loop**: Outcomes fed back into next hypotheses (Phase 3)

### 4.3 Risk Management
- **Gas Gates**: Hard stop when gas > 30 gwei
- **Position Limits**: 0.05 ETH maximum
- **Profit Thresholds**: Must clear gas costs + buffer
- **Network Safety**: Sepolia testnet only — mainnet requires explicit upgrade decision

### 4.4 UI Layer (Streamlit)
- **Dashboard**: Live ETH price, gas price, system status
- **Hypothesis Panel**: AI-generated hypotheses with reasoning, Approve/Reject buttons
- **Trade History**: All executed trades with P&L
- **Runs locally** at `localhost:8501` — no deployment needed

---

## 5. Implementation Timeline

| Phase | Duration | Key Deliverables | Dependencies |
|-------|----------|----------------|--------------|
| Phase 1 | 1-2 days | Price data + AI hypotheses + Streamlit dashboard | API keys, Sepolia setup |
| Phase 2 | 2-3 days | Testnet trade execution + gas management | Phase 1 complete |
| Phase 3 | 2-3 days | Learning loop + outcome feedback | Phase 2 complete |
| Mainnet | Post-POC | Real capital trading | Phase 3 stable, explicit decision |
| **Total** | **5-8 days** | **Working Testnet POC** | **None** |

---

## 6. Success Criteria

- [ ] AI generates structured hypotheses with reasoning and confidence score
- [ ] Streamlit dashboard displays live price, gas, and hypotheses
- [ ] Approve/Reject buttons work and persist decisions to SQLite
- [ ] Gas management prevents trades when gas > 30 gwei
- [ ] Testnet trades execute on Sepolia with real transactions
- [ ] Learning loop shows outcome feedback in dashboard
- [ ] System runs continuously for 24 hours on testnet

---

*Document Version: 1.2*
*Last Updated: Feb 26, 2026*
