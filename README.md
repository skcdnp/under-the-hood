
# Shomi K. Chowdhury

**Technical Product & Program Leader  |  AI Builder  |  Fintech & Capital Markets**

[LinkedIn](https://linkedin.com/in/shomichowdhury) · [GitHub](https://github.com/skcdnp/under-the-hood) · Jersey City, NJ

---

## Background

I am a strategic technical product leader with 18 years of experience building mission-critical financial infrastructure across capital markets and fintech. I define product strategy as a product leader and own cross-functional execution as a technical program driver, flexing between both based on what the situation demands.

I am usually there when the problem is complex, the stakes are high, the timeline is tight, and there is no existing playbook. I do not manage systems from a distance. I like build them, make the architecture calls, navigate the stakeholder complexity, and drive the outcomes.

**The through-line: high-visibility, technically complex programs with real dollar impact.**

---

## Experience

### Capital One — Director, Technical Product / Program Management *(2022 – Present)*

Leading two enterprise-critical programs simultaneously, both at the intersection of financial technology, AI-driven decisioning, and large-scale systems transformation.

**ARCTIC — Real-Time Intelligent Collections Platform.** Led the modernization of Capital One's collections decisioning platform from a 24-hour batch cycle to sub-3-second real-time decisioning on a fully event-driven, cloud-native AWS architecture. The platform ingests enterprise data signals, normalizes them through a canonical event model, orchestrates workflows via AWS Step Functions, applies ML-ready decisioning logic through a centralized rules engine built on Decision Model Notation, and records every decision with full traceability for compliance. Runs active-active across multiple AWS regions.

**Capital One–Discover Integration.** Leading the technical integration strategy for the post-acquisition Collections platform, aligning data models, conversion processes, and decisioning capabilities across 14+ engineering teams — with $1B+ in projected charge-off savings.

Also built the TPM organization at Capital One from zero, scaling to support 20+ engineering teams, and drove a 25% reduction in time-to-market through improved developer experience, shared contribution models, and AI-assisted CI/CD tooling.

---

### Morgan Stanley — Vice President & Associate, Product & TPM *(2011 – 2022)*

Over eleven years at Morgan Stanley, three programs define the tenure:

- **Enterprise Sub-Ledger Platform** — Built from scratch, replacing manual Excel-dependent financial workflows with a fully automated accounting platform integrating SAP, Zuora, Workday, and Ariba under SOX, Basel III, and GDPR constraints. Results: 65% reduction in monthly close processing time, 40% error reduction, 95% user satisfaction, zero direct ledger entries by Month 2.
- **CRM & Financial Advisor Workstation** — Designed and delivered the enterprise CRM platform for 35,000 financial advisors and CSAs, migrating 2.5 million accounts and 6 million prospect records. Advisor engagement increased 17%.
- **Contact Center Modernization** — Deployed intelligent routing and IVR modernization that reduced average call wait times from 5 minutes to under 2 minutes.

---

### Earlier Career & Entrepreneurship

Senior business analyst and consulting roles at **JP Morgan Chase** and **Wells Fargo**, focused on portfolio accounting engine modernization and core banking system migrations.

Co-founded **Giving Digitized** — a consumer social-giving platform scaling to 9 cities and 27+ charity partnerships, where I owned product strategy, engineering execution, and operational scale in a zero-to-one environment.

---

## How I Work with AI — Philosophy, Practice, and Systems Built

I want to be precise about how I think about AI, because the framing matters. I do not use AI as a tool I reach for occasionally to accelerate a task. I treat AI as a collaborator — an active participant embedded across the full product delivery lifecycle. This is not a recent adoption driven by industry momentum. It reflects a fundamental shift in how I approach the work itself, developed over years of building with AI in both professional and personal contexts.

---

### AI as Daily Collaborator — Augmentation Before Automation

Across requirements gathering, go-to-market strategy, technical analysis, stakeholder communication, and roadmap prioritization — AI is embedded in each of these functions as a thinking partner, not a shortcut. When I am working through a complex stakeholder alignment problem, I reason through it with AI, pressure-testing my logic and stress-testing my recommendations before I walk into the room. When I am synthesizing technical documentation, regulatory guidance, or competitive research to inform a product decision, AI compresses what would take days into hours — without reducing the rigor of the judgment. When I am preparing for a C-suite conversation, I use AI to sharpen the framing, simulate the pushback, and ensure my recommendation is airtight before I deliver it.

I also use AI to build proof-of-concepts, draft and iterate product requirements documents, and stress-test architectural decisions in real time. The development loop — define, build, test, iterate — is faster and more rigorous when AI is a participant throughout, not a tool consulted at the end. This is augmentation, not automation. The judgment remains human. AI makes it faster and better-informed.

---

### Building Agentic Systems Before the Term Existed

Before agentic AI became the dominant conversation in technology, I was building its architectural patterns in production. **ARCTIC** — the Real-Time Intelligent Collections platform at Capital One — is, at its core, an agentic system. It was designed around a perceive, reason, act, and record loop: customer state changes are ingested as business-salient events, canonicalized through a standardized schema, processed through an AWS Step Functions orchestration layer that gathers context, applies ML-ready decisioning logic via Decision Model Notation, executes a fulfillment action, and records every decision with full traceability for compliance. Every rule in the decisioning layer is independently swappable with a model call — the architecture was designed to evolve toward ML without requiring the underlying orchestration to change.

I drove the technical vision for ARCTIC in partnership with architecture teams and aligned engineering, business analysis, and operations organizations around this pattern — before the industry had a name for what we were building. The design principles applied here — canonical event models for stable ingestion, modular task libraries for reusability, observability as a first-class requirement, human-in-the-loop gates for high-stakes actions — are the same principles I apply to every AI system I build today.

---

### Personal AI Systems — Applied Builder Credibility

Beyond my professional work, I actively build AI-powered systems on my own time — serious attempts to apply agentic AI patterns to real-world financial problems. Full project details at [github.com/skcdnp/under-the-hood](https://github.com/skcdnp/under-the-hood).

These are some of my projects. Others are tucked away on my mac hard-drive and serving other purposes.


**Monarch Regent — Autonomous Crypto Trading System**
`Python · Streamlit · Anthropic Claude API · Ethereum / Sepolia Testnet`

Building an autonomous trading system that combines live blockchain data with AI-generated market hypotheses. The system pulls real-time ETH price and gas data from the Ethereum network, feeds it into a Claude AI model that produces structured trade hypotheses — complete with entry/exit targets, confidence scores, and written reasoning — then stores everything in a local database for outcome tracking.

The architecture is intentionally lean: a five-file Python stack (data ingestion, AI layer, trade execution, config, and a Streamlit dashboard) that proves the core innovation without premature complexity. The dashboard lets a human review and approve or reject each AI hypothesis before any capital moves, keeping a human in the loop at every decision point.

Built with gas-aware risk management — trades are blocked when Ethereum network fees exceed a defined threshold, and position sizing is capped to control downside. All current execution happens on the Sepolia testnet, with mainnet upgrade gated behind explicit validation criteria.

Additional Details in the project folder.


**Monarch Chancellor — AI-Powered Investment Analytics Platform**
`Python · FastAPI · React 18 · TypeScript · Anthropic Claude API · SQLAlchemy`

A full-stack investment research platform with strict separation of concerns: quantitative computation — portfolio metrics, allocation analysis, risk calculations — runs locally with deterministic logic, while Claude generates plain-language portfolio insights and strategic recommendations against that computed foundation. A hard-gated verification system enforces allocation constraints before any recommendation is surfaced. No output reaches the user without passing constraint validation. The design reflects a core principle: AI generates, humans verify, and the verification gate is not optional.


**Monarch Lighthouse — AI-Powered Investment Analytics Platform** 

Monarch-Lighthouse is a personal financial intelligence platform that aggregates real-time macro data and company-specific news to generate AI-driven portfolio insights — running entirely on a local machine with no cloud dependency or paid data subscriptions.

The platform monitors key economic indicators, ingests news from tiered public sources (Reuters, SEC EDGAR, Federal Reserve, FRED, Finnhub), and uses a materiality engine to determine when genuinely new information warrants generating a fresh AI insight. This prevents redundant analysis and ensures every output reflects a meaningful change in market conditions.

The AI layer is model-agnostic, designed to run locally via Ollama and configurable to connect to frontier models such as Claude or GPT-4 through a single setting. All data ingestion, deduplication, and insight generation run as independent, fault-tolerant components — no single source failure disrupts the system.

Technology: Claude Code, Windsurf, Python · FastAPI · SQLite · React · Ollama · FRED API · Finnhub API · SEC EDGAR · BLS API · Federal Reserve RSS · yfinance


---

All systems were built AI-natively throughout the development lifecycle. Product requirements were drafted in collaboration with AI and iterated through real-time feedback loops. Architecture decisions were stress-tested with AI before implementation. Code was developed using **Claude** and **Windsurf**, with AI participating in design review, debugging, and refinement at every stage. The experience of building this way — as a product leader, not an observer — is what gives me a concrete understanding of where AI adds leverage, where it introduces risk, and how to design systems that are both capable and accountable.

