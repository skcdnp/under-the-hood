Below is a tight, recruiter-friendly README. It keeps the interesting parts, removes the excess detail, and is optimized for scanability (which is how most engineers and recruiters read repos).
It should take ~60–90 seconds to read and clearly communicates:
what the project does
why it’s interesting
what technologies you used
what technical problems you solved

# 🤖 AI Crypto Trading POC

![Python](https://img.shields.io/badge/python-3.10+-blue.svg)
![Streamlit](https://img.shields.io/badge/ui-streamlit-red)
![Ethereum](https://img.shields.io/badge/network-sepolia-purple)
![Status](https://img.shields.io/badge/status-POC-orange)
![License](https://img.shields.io/badge/license-MIT-green)


An experimental trading system where an AI proposes crypto trades — but must explain its reasoning before anyone executes them.

This project explores a simple idea:

Let an AI generate structured trading hypotheses with explicit reasoning, then allow a human to approve or reject them before execution. The system connects to Ethereum data, generates hypotheses using an LLM, displays them in a dashboard, and executes approved trades on Sepolia testnet.

**Core Idea**

The AI doesn't place trades directly.

It produces a structured hypothesis like:
```json
{
  "asset": "ETH",
  "direction": "BUY",
  "entry_price": 3050,
  "take_profit": 3120,
  "stop_loss": 3000,
  "confidence": 0.68,
  "reasoning": "ETH momentum is positive and gas costs allow profitable execution."
}
```
The system then asks a human operator:
“Does this trade idea make sense?”

Because letting an AI freely control your wallet is a great way to learn about risk management the hard way.

**Architecture**
```json
The system was intentionally designed as a small, hackable POC rather than a large distributed trading platform.
crypto-trader-poc/
├── config.py      # API keys and risk limits
├── data.py        # Market data from Alchemy
├── ai.py          # Claude hypothesis generation
├── trader.py      # Trading logic + Sepolia execution
├── app.py         # Streamlit dashboard
└── trades.db      # SQLite storage
```
Everything runs locally with minimal infrastructure.

**System Flow**

Market Data → AI Hypothesis → Human Approval → Testnet Trade → Outcome Feedback
```json
Fetch ETH price and gas data
Generate a trading hypothesis using Claude
Display the hypothesis in a Streamlit dashboard
User approves or rejects the trade
Trade executes on Sepolia testnet
Outcome is recorded and used in future prompts
```
**Dashboard**

The project includes a lightweight Streamlit dashboard for monitoring and approving trades.
```json
The UI displays:
Live ETH price
Current gas price
AI-generated hypotheses
Approve / Reject controls
Trade history and P&L
```
**Risk Controls**

Even for a proof-of-concept, a few guardrails exist.
```json
Gas gate
Trades are blocked if gas exceeds the threshold.
gas_price > 30 gwei

Position limit
max_position = 0.05 ETH

Profit requirement
Trades must exceed estimated gas costs by a safe margin.
expected_profit ≥ 3 × gas_cost
```
All trading occurs on Sepolia testnet.

**Tech Stack**
```json
Python
Streamlit
web3.py
SQLite
Claude API
Alchemy Ethereum RPC
```
**Why This Exists**

Most AI trading experiments try to build fully autonomous trading bots.

This project takes a different approach:

The AI acts more like a research assistant for trade ideas than a system controlling capital. It proposes hypotheses, explains its reasoning, and leaves the final decision to the operator.

⭐ If you find the project interesting, feel free to star the repo.

