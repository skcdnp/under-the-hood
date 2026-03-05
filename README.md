# under-the-hood
A look at the blueprints and schematics behind my private repositories.

____________________________________________________________________________________________________________________________________________________

**Monarch Sentinel — Automated Day Trading Insights Generator **

Still tinkering so details to come later.

____________________________________________________________________________________________________________________________________________________

**Monarch Chancellor — Personal Investment Analytics Platform**

Building a full-stack investment analysis application designed to help retail investors make data-driven portfolio decisions without the complexity of institutional tooling.

The platform combines a Python/FastAPI backend with a React + TypeScript frontend to deliver real-time portfolio drift analysis, rebalancing recommendations, and AI-generated investment insights. A core design principle is strict separation of concerns between quantitative computation (all financial math runs locally) and AI narrative generation (Claude LLM produces plain-language explanations, never raw numbers) — ensuring accuracy and auditability at every step.

Key engineering highlights include a hard-gated trade verification system that enforces allocation constraints before any recommendation is surfaced, a PII scrubbing layer that controls exactly what data reaches external APIs, and an append-only audit trail for every data ingestion, policy change, and AI interaction. The system is built with multi-user isolation, JWT authentication, and a caching architecture designed for graceful degradation on stale market data.

The project follows a phased roadmap — Phase 1 covers portfolio management and AI-assisted analysis; later phases will expand into macroeconomic data integration and deeper SEC filing analysis.

**Stack: Python, FastAPI, SQLAlchemy, React 18, TypeScript, Vite, shadcn/ui, Recharts, Anthropic Claude API**

######
_____________________________________________________________________________________________________________________________________________________

**Monarch-Regent — AI-Driven Crypto Trading System**

Building an autonomous trading system that combines live blockchain data with AI-generated market hypotheses. The system pulls real-time ETH price and gas data from the Ethereum network, feeds it into a Claude AI model that produces structured trade hypotheses — complete with entry/exit targets, confidence scores, and written reasoning — then stores everything in a local database for outcome tracking.

The architecture is intentionally lean: a five-file Python stack (data ingestion, AI layer, trade execution, config, and a Streamlit dashboard) that proves the core innovation without premature complexity. The dashboard lets a human review and approve or reject each AI hypothesis before any capital moves, keeping a human in the loop at every decision point.

Built with gas-aware risk management — trades are blocked when Ethereum network fees exceed a defined threshold, and position sizing is capped to control downside. All current execution happens on the Sepolia testnet, with mainnet upgrade gated behind explicit validation criteria.

Phase 1 (data → AI → storage pipeline) is complete. Phase 2 (live testnet execution with gas management) is in progress.
