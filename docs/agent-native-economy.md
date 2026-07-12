# Agent-Native Economy: Software Agents Under Delegated Authority

*Research Area 1 — the Agent-Decision node of the SDE reference loop: production agent runtimes, machine identity, cross-organization coordination, and the pilot-to-production gap.*

*Last updated: July 2026 · Part of the [Open-SDE](../README.md) research initiative.*

---

## Scope

In the [SDE reference loop](./concepts.md#glossary) (Reality Signals → Digital Twin → Agent Decision → Governance Gate → Execution), the **Agent-Decision** node is where an AI agent turns context into a *proposed* action. This document surveys what is actually shipping at that node in mid-2026: the production agent runtimes and their harness layers, the identity substrate that makes agents accountable, the protocols and rails that let agents transact with one another, and the empirical evidence on how far this has moved from demo to deployment.

A framing correction runs through the whole survey. These are **software agents acting under delegated authority**, not first-class economic actors. An agent participates in the economy only once a human or organization has granted it an identity and a **bounded mandate** — scope, budget, duration, target, and revocation conditions. Identity establishes *who* is acting; the mandate establishes *what it may do*. Everything below is infrastructure for that precondition, and for keeping the probabilistic act of *deciding* separate from the deterministic acts of *authorizing*, *executing*, and *settling* — the three-layer split the IMF describes for agentic payments, where intent and orchestration may be probabilistic but authorization/control and settlement must stay deterministic and auditable ([IMF Note 2026/004, "How Agentic AI Will Reshape Payments"](https://www.imf.org/en/publications/imf-notes/issues/2026/04/22/how-agentic-ai-will-reshape-payments-575560), April 2026).

The disciplined framing for the whole repository applies most sharply here: **the rails and primitives now largely exist, while realized economic activity through them is still early.** Where a claim is only partly corroborated, it is hedged; where the fact-check excluded a claim, it is not made.

---

## 1. Production agent runtimes and the harness layer

The most concrete 2026 development is that agent SDKs stopped being thin wrappers over a model API and started shipping a standardized **[agent harness](./concepts.md#glossary)** — context compaction, memory, file/tool access, sandboxed execution, and middleware — so an agent can work a long task without exhausting its context window.

- **Microsoft Agent Framework** reached 1.0 general availability in early April 2026, converging AutoGen's agent abstractions and Semantic Kernel's enterprise features (session state, type safety, middleware, telemetry) into a single supported platform for .NET and Python, with native MCP and A2A support ([Microsoft Learn](https://learn.microsoft.com/en-us/agent-framework/overview/)). At Build 2026 Microsoft added the harness layer, session-isolated **Foundry Hosted Agents**, and **[CodeAct](./concepts.md#glossary)** — where an agent collapses a multi-step plan into a single sandboxed program run in an isolated micro-VM, reported to cut latency ~50% and tokens >60% ([Microsoft, Build 2026](https://devblogs.microsoft.com/agent-framework/microsoft-agent-framework-at-build-2026-announce/)). CodeAct is the clearest expression of the SDE premise — *decisions as executable code* — pushed into the agent's own action layer.
- **OpenAI** shipped a model-native, sandboxed harness for long-horizon coding agents on April 15, 2026, adding bring-your-own sandbox execution, configurable memory, and declarative guardrails ([OpenAI](https://openai.com/index/the-next-evolution-of-the-agents-sdk/)). (A separately circulated June 2026 "Lockdown Mode" claim could not be verified and is not relied on here.)
- **Anthropic's Claude Agent SDK** now underpins hosted **Managed Agents**, which add a scheduler, a memory-curating "dreaming" pass that reviews prior sessions during idle time to update memory without changing weights, and outcomes-based grading (announced May 6, 2026) ([Anthropic](https://claude.com/blog/new-in-claude-managed-agents)). Persistent memory plus scheduled re-execution plus outcome checks is the substrate for continuous, self-correcting agents.
- **Google's ADK** matured across languages; ADK Go 1.0 shipped in 2026 with Agent Engine deployment and native A2A support ([Google Developers](https://developers.googleblog.com/adk-go-10-arrives/)). The earlier "all four languages GA at once" framing overstated a messier reality (Python/Java 1.0 predate 2026; Go and TypeScript matured later), so it is stated here as a gradual, multi-language maturation.

**Why it matters for the SDE:** hosted, session-isolated, framework-agnostic runtimes with built-in tool-approval middleware and telemetry are exactly the infrastructure the Agent-Decision node needs — and their approval hooks map directly onto the downstream [Governance Gate](./governance-as-code.md).

---

## 2. Long-horizon capability: how much of a workflow an agent can own

The single best proxy for how much of an economic process an agent can autonomously own is its **[time horizon](./concepts.md#glossary)** — the length of task (measured as the time a human expert would take) it can complete at a given reliability.

METR's time-horizons benchmark, last updated May 8, 2026, places frontier 50%- and 80%-reliability horizons in the **multi-hour range**, and reporting widely describes the historical ~7-month doubling as compressing to a few months ([METR](https://metr.org/time-horizons/)). Two caveats are load-bearing and are stated here deliberately: METR **flags measurements above 16 hours as unreliable** on its current suite, and the precise task counts and doubling figures that circulated in secondary coverage are *not* stated on METR's page — so they are not repeated. Read conservatively, the trend still points one way: the automatable share of multi-step workflows is expanding at least as fast as prior trend, which is what makes the Agent-Decision node worth building infrastructure around.

---

## 3. Agent identity and mandate: the precondition for participation

You cannot enforce executable constraints, attribution, or audit trails on a software agent without a durable, revocable machine identity. **[Agent identity](./concepts.md#glossary)** — credentialing agents like human or service accounts so their actions can be authenticated, attributed, and revoked — emerged in 2026 as the recognized precondition for cross-organization agent commerce.

Identity is necessary but not sufficient: authenticating *who* an agent is does not authorize *what* it may do. That requires a separate mandate plus an authorization decision taken **outside the model**. The distinction was standardized in January 2026 when the OpenID Foundation approved the [AuthZEN Authorization API 1.0](https://openid.net/specs/authorization-api-1_0.html) as a Final specification, splitting the authorization gate into a **Policy Decision Point** (which decides) and a **Policy Enforcement Point** (which enforces the verdict on each action) — the ABAC/XACML separation rendered as an interoperable JSON API. NIST's [AI Agent Standards Initiative](https://www.nist.gov/artificial-intelligence/ai-agent-standards-initiative) (announced February 2026, with a companion concept paper on AI agent identity *and* authorization) pairs the two for the same reason.

| Instrument | What it provides | Status |
|---|---|---|
| **Microsoft Entra Agent ID** | Manages agents as first-class enterprise identities with lifecycle, credential, and access governance | Reportedly reached GA around April–May 2026 ([Microsoft Learn](https://learn.microsoft.com/en-us/entra/agent-id/whats-new-agent-id)) — *partly confirmed* |
| **ERC-8004 (Trustless Agents)** | Three on-chain registries (Identity, Reputation, Validation) giving agents ERC-721 identities, reputation signals, and optional validation for cross-org discovery without prior trust | Specification still **Draft** (Standards Track/ERC, created Aug 2025); reference registries deployed to Ethereum mainnet in early 2026 ([EIP-8004](https://eips.ethereum.org/EIPS/eip-8004)) — *emerging proposal, not a settled standard* |
| **Skyfire "Know Your Agent" (KYA)** | Cryptographically binds an agent's requests to a registered operator and authorized user; paired with KYAPay stablecoin settlement | Demonstrated Dec 2025 with Visa Intelligent Commerce ([Skyfire/Visa](https://www.businesswire.com/news/home/20251218520399/en/Skyfire-Demonstrates-Secure-Agentic-Commerce-Purchase-Using-the-KYAPay-Protocol-and-Visa-Intelligent-Commerce)) — *confirmed* |
| **a16z "Know Your Agent" primitive** | Proposed cryptographically signed credentials linking an agent to its principal, constraints, and liability — an agent analog to KYC | Proposal, Dec 2025 ([a16z Big Ideas 2026](https://a16z.com/newsletter/big-ideas-2026-part-1/)) — *confirmed* |

a16z frames the agent-economy bottleneck as shifting "from intelligence to identity," noting that non-human identities already outnumber human employees roughly 96-to-1 in financial services ([a16z](https://a16z.com/newsletter/big-ideas-2026-part-1/)). ERC-8004 saw thousands of agents registered across chains within weeks of mainnet reference deployment — an early but real signal of demand for a decentralized trust substrate, though the specification itself remains a **Draft** ERC (created August 2025) and is best read as an emerging proposal rather than a settled standard.

---

## 4. Agent-to-agent coordination and settlement

Once agents have identities, they can discover, authenticate to, and delegate to one another across organizational boundaries — and, increasingly, pay one another.

- **A2A (Agent2Agent).** Donated to the Linux Foundation in 2025, the interoperability protocol reached its first stable **v1.0** and surpassed **150 organizations** in production (AWS, Cisco, Google, IBM, Microsoft, Salesforce, SAP, ServiceNow), adding cryptographically **signed Agent Cards** for domain-level trust ([Linux Foundation, April 9, 2026](https://www.linuxfoundation.org/press/a2a-protocol-surpasses-150-organizations-lands-in-major-cloud-platforms-and-sees-enterprise-production-use-in-first-year)). This is the emerging wire protocol for the "execution via protocols" layer of the SDE. (Draft claims of a "v1.2" spec or an "Agentic AI Foundation" are contradicted by the primary source and are not made here.)
- **Agentic wallets.** Coinbase launched **Agentic Wallets** on Feb 11, 2026 — MPC-secured, policy-controlled wallets with session caps, spend limits, and guardrails plus native x402 support, so an agent transacts within bounded authority ([Coinbase Developer Platform](https://www.coinbase.com/developer-platform/discover/launches/agentic-wallets)). Circle followed on May 11, 2026 with the **Circle Agent Stack**: self-custodial policy-controlled Agent Wallets, an Agent Marketplace for service discovery, a CLI, and **gas-free USDC nanopayments** as small as ~$0.000001 for high-frequency machine-to-machine flows ([Circle](https://www.circle.com/pressroom/circle-launches-ai-infrastructure-to-power-the-agentic-economy)).
- **Agent-to-agent labor markets.** With ERC-8004 identity and stablecoin settlement in place, agents can begin hiring other agents — a yield agent paying a risk-analysis agent, autonomous DeFi vaults allocating capital within encoded constraints. A July 2026 preprint formalizes the identity/authorization/payment/reputation stack for this pattern ([arXiv:2607.00245](https://arxiv.org/abs/2607.00245)) — *partly confirmed*, and the specific vault TVL figures reported in secondary coverage are treated as uncertain.

There is now a **hard measurement** of this activity rather than only narrative. A Keyrock analysis (via CoinDesk, May 21, 2026) found agents settled roughly **176 million on-chain transactions worth over $73 million** between May 2025 and April 2026, with **98.6% in USDC** and about 76% of payments falling below card networks' ~30-cent fee floor ([CoinDesk](https://www.coindesk.com/business/2026/05/21/crypto-rails-are-becoming-the-default-payment-layer-for-ai-agents-report-says)). Real, but small — and structurally concentrated in a single stablecoin, a systemic-risk point taken up in the [roadmap](../ROADMAP.md). The mechanics of the payment and settlement rails themselves are covered in the [reference architecture](./reference-architecture.md).

---

## 5. The binding constraint: the pilot-to-production gap

Raw model capability is not the bottleneck. Adoption evidence shows the opposite.

Gartner predicted ~40% of enterprise apps would feature task-specific AI agents by 2026, up from under 5% in 2025, and separately forecast that **more than 40% of agentic AI projects will be canceled by 2027** over cost, unclear ROI, and weak risk controls ([Gartner](https://www.gartner.com/en/newsroom/press-releases/2025-08-26-gartner-predicts-40-percent-of-enterprise-apps-will-feature-task-specific-ai-agents-by-2026-up-from-less-than-5-percent-in-2025)). Secondary 2026 trackers report that roughly **88–89% of agent pilots never reach production** — a figure that comes from industry roundups rather than the Gartner release, so it is cited as a secondary signal, not a primary statistic.

The implication is the core of Open-SDE's thesis: the minority of pilots that scale are distinguished by **governance, cost control, and reliable execution**, not by a smarter model. That is precisely what the [Governance Gate](./governance-as-code.md) is meant to encode by default — and one of the central open questions in the [roadmap](../ROADMAP.md) is which primitives separate the pilots that scale from those that don't.

What the failing pilots lack is rarely intelligence; it is the **authority and assurance scaffolding** that keeps an agent's actions bounded and reversible. Concretely, four controls tend to be missing. First, an identity and a mandate the agent cannot exceed. Second, an authorization decision taken *outside* the model — at a Policy Decision Point, enforced at a Policy Enforcement Point on every action ([AuthZEN Authorization API 1.0](https://openid.net/specs/authorization-api-1_0.html), Final, January 2026) — rather than trusting the agent's own reasoning to self-limit. Third, a deterministic execution and settlement layer kept separate from the probabilistic reasoning that proposed the action ([IMF Note 2026/004](https://www.imf.org/en/publications/imf-notes/issues/2026/04/22/how-agentic-ai-will-reshape-payments-575560), April 2026). Fourth, a **runtime-assurance** monitor that bounds the agent and reverts to a verified-safe fallback when a violation is imminent — not a novel idea but inherited safety-engineering practice, codified for aviation in [ASTM F3269-21](https://store.astm.org/f3269-21.html) (2021) and traceable to Lui Sha's Simplex Architecture. And because a one-time pre-deployment test cannot anticipate real-world behavior, continuous post-deployment monitoring — across functionality, operational, human-factors, security, compliance, and large-scale-impact dimensions — is itself the subject of a final NIST report ([NIST AI 800-4, "Challenges to the Monitoring of Deployed AI Systems"](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.800-4.pdf), March 2026). These controls are what the [SDE-0 conformance profile](./sde-0-conformance-profile.md) makes mandatory and what the [authority-and-safety model](./authority-and-safety-model.md) specifies in full; the pilot-to-production gap is, in large part, the gap between an agent that *can* act and one whose authority to act is granted, bounded, monitored, and revocable.

---

## 6. Open-source frameworks specialize into control primitives

Beneath the hyperscaler SDKs, the open-source layer specialized around *control*. By 2026, LangGraph (with improved state persistence and first-class human-in-the-loop checkpoints) is positioned for production-grade control, audit trails, and rollback, while CrewAI targets fast role-based prototyping; a common pattern is prototyping on CrewAI and migrating production-critical paths to LangGraph ([framework comparison](https://pecollective.com/blog/ai-agent-frameworks-compared/)) — *partly confirmed, secondary source*. Human-in-the-loop checkpoints, durable state, and rollback points are the control primitives the SDE needs to insert governance gates and safe reversibility into otherwise-autonomous execution graphs.

---

## 7. Concentration and lock-in risk

An economy whose executable rules run on a handful of proprietary agent runtimes carries a systemic-risk profile worth naming explicitly. The Agent-Decision node is consolidating around a few vendor SDKs (Microsoft, OpenAI, Anthropic, Google); the settlement layer measured above is 98.6% dependent on a single stablecoin issuer. Whether the identity, coordination, and settlement stacks **converge into one interoperable trust fabric or fragment along hyperscaler lines** is, from the SDE's perspective, the decisive open question — not whether agents *can* transact, but who controls the rules when they do. This is carried into the [roadmap](../ROADMAP.md) as a standing research theme.

---

## Real vs. early: a summary

| Capability | Status | Evidence |
|---|---|---|
| Production agent SDKs with a standardized harness layer | **Real / shipping** | Microsoft Agent Framework 1.0 GA; OpenAI harness; Claude Managed Agents; ADK Go 1.0 |
| CodeAct — decisions as sandboxed executable code | **Real / shipping** | Microsoft Build 2026 |
| Rising long-horizon capability | **Real, but bounded** | METR multi-hour horizons; >16h flagged unreliable; doubling *reportedly* compressing |
| Agent identity + authorization standards | **Shipping (mixed maturity)** | AuthZEN API 1.0 Final (PDP/PEP); ERC-8004 registries on mainnet but spec still Draft; Entra Agent ID (partly confirmed); Skyfire/a16z KYA |
| A2A cross-org coordination | **Real / shipping** | A2A v1.0, 150+ orgs, signed Agent Cards |
| Agentic wallets with encoded spend caps | **Real / shipping** | Coinbase Agentic Wallets; Circle Agent Stack |
| Measurable on-chain agent settlement | **Real, but small** | Keyrock: ~176M tx, >$73M, 98.6% USDC |
| Agent-to-agent labor markets / DeFi agent vaults | **Early / partly confirmed** | arXiv:2607.00245; secondary TVL figures uncertain |
| Autonomous agents scaling in the enterprise | **Early** | ~88–89% of pilots fail to scale; >40% of projects expected canceled by 2027 |

---

## Related

- [Concepts, primitives, and glossary](./concepts.md) — definitions for the terms used here.
- [Authority and Safety Model](./authority-and-safety-model.md) — the delegated-authority and runtime-assurance controls referenced above, and the eight non-claims.
- [SDE-0 Conformance Profile](./sde-0-conformance-profile.md) — the minimum identity, mandate, PDP/PEP, and monitoring requirements a participating agent must meet.
- [Governance as Code](./governance-as-code.md) — the gate that bounds the decisions surveyed here.
- [Reality-Anchored Execution](./reality-anchored-execution.md) — the embodied Agent-Decision leg (VLA models, robots, DePIN).
- [The SDE Reference Loop, Expanded](./reference-architecture.md) — how these pieces compose end-to-end.
- [The Software-Defined Economy in Mid-2026](./landscape-2026.md) — the full dated landscape.
- [Roadmap](../ROADMAP.md) — the open questions raised above.

See [references.md](./references.md) for the full, annotated source list.
