# Annotated Reading List

*The primary and reputable sources underpinning Open-SDE's mid-2026 research — grouped by layer of the [SDE reference loop](./reference-architecture.md), annotated, and marked by verification status.*

*Last updated: July 2026 · Part of the [Open-SDE](../README.md) research initiative.*

---

## How to read this list

This is the source of truth behind every dated claim in the Open-SDE docs. Each entry records the **actors**, the **approximate date**, a one-line **why it matters**, and two tags:

- **Source type** — `[primary]` for vendor, standards-body, regulator, or first-party research; `[secondary]` for analyst notes, trade press, aggregators, or promotional write-ups. We prefer primary sources and flag secondary ones explicitly, because much of the machine-economy narrative is carried by promotional material.
- **Verification** — `confirmed` where the claim is corroborated by a primary source; `partly confirmed` where the core claim holds but specific figures, dates, or sub-claims could not be independently verified and were softened. This mirrors the fact-checking verdict attached to each development in the research.

Two entries carry more evidential weight than the rest and are worth reading first: METR's time-horizons benchmark (the best available proxy for how much of a workflow an agent can own) and the Keyrock/CoinDesk measurement of on-chain agent settlement (the first hard number on realized agent payment activity). Both are called out below.

A note on discipline: this list documents **deployed infrastructure**, which is not the same as **realized economic activity**. Where a source describes a rail, a protocol, or a demo rather than clearing demand, the annotation says so. See [`landscape-2026.md`](./landscape-2026.md) for the synthesis and [`concepts.md`](./concepts.md#glossary) for defined terms.

---

## 1. Agent runtimes & capability

The [agent harness](./concepts.md#glossary) layer and the [Agent-Decision](./reference-architecture.md) node — production SDKs, capability measurement, and the pilot-to-production gap. Covered in depth in [`agent-native-economy.md`](./agent-native-economy.md).

- **[Microsoft Agent Framework Overview](https://learn.microsoft.com/en-us/agent-framework/overview/)** · Microsoft · Apr 2026 · `[primary]` `confirmed`
  Primary docs for Agent Framework 1.0 GA, unifying AutoGen and Semantic Kernel with native MCP/A2A support and middleware for tool-approval and telemetry. *Why it matters:* a standardized, enterprise-grade substrate for the Agent-Decision stage.

- **[Microsoft Agent Framework at Build 2026: Agent Harness, Hosted Agents, CodeAct](https://devblogs.microsoft.com/agent-framework/microsoft-agent-framework-at-build-2026-announce/)** · Microsoft (Azure AI Foundry) · May 2026 · `[primary]` `confirmed`
  Introduces the agent harness, session-isolated Foundry Hosted Agents, and [CodeAct](./concepts.md#glossary) — a multi-step plan collapsed into one sandboxed program. *Why it matters:* pushes "decisions as executable code," the core SDE premise, into the action layer.

- **[The next evolution of the Agents SDK](https://openai.com/index/the-next-evolution-of-the-agents-sdk/)** · OpenAI · Apr 15, 2026 · `[primary]` `confirmed`
  A model-native sandboxed harness for long-horizon coding agents with declarative guardrails. *Why it matters:* a direct Governance-Gate → Execution implementation. (A rumored June 2026 "Lockdown Mode" could not be verified and is excluded.)

- **[New in Claude Managed Agents: dreaming, outcomes, and multiagent orchestration](https://claude.com/blog/new-in-claude-managed-agents)** · Anthropic · May 6, 2026 · `[primary]` `confirmed`
  The Claude Agent SDK underpinning hosted Managed Agents with scheduling, a memory-curating "dreaming" pass, and outcomes-based grading. *Why it matters:* operationalizes continuous, self-improving agents with measurable outcome checks.

- **[ADK Go 1.0 Arrives!](https://developers.googleblog.com/adk-go-10-arrives/)** · Google Cloud · 2026 · `[primary]` `partly confirmed`
  Google's Agent Development Kit maturing across languages with Agent Engine deployment and native A2A. *Why it matters:* a multi-language, production-stable framework lowers the cost of composing heterogeneous agent networks. (A single-event "all four languages at Cloud Next" framing was corrected; Python/Java 1.0 predate 2026.)

- **[A2A Protocol Surpasses 150 Organizations, Lands in Major Cloud Platforms](https://www.linuxfoundation.org/press/a2a-protocol-surpasses-150-organizations-lands-in-major-cloud-platforms-and-sees-enterprise-production-use-in-first-year)** · Google, Linux Foundation, 150+ orgs · Apr 9, 2026 · `[primary]` `confirmed`
  [A2A](./concepts.md#glossary) reaches stable v1.0 with cryptographically signed Agent Cards. *Why it matters:* the cross-organization wire protocol for agent delegation and trust. (Draft "v1.2" / "Agentic AI Foundation" claims contradicted the primary source and were corrected to v1.0 / Linux Foundation.)

- **[Task-Completion Time Horizons of Frontier AI Models (METR)](https://metr.org/time-horizons/)** · METR · May 8, 2026 · `[primary]` `partly confirmed` · **empirical anchor**
  Multi-hour 50%/80%-reliability [time horizons](./concepts.md#glossary); the historical ~7-month doubling is widely reported as compressing, though METR flags measurements above 16 hours as unreliable. *Why it matters:* the single best proxy for how much of a workflow an agent can autonomously own. (Specific figures circulating in secondary reporting — task counts, an exact ~3h 80% horizon, a 4–4.3-month doubling — are not stated on METR's page and are excluded.)

- **[Microsoft Entra Agent ID: What's New](https://learn.microsoft.com/en-us/entra/agent-id/whats-new-agent-id)** · Microsoft (Entra) · May 2026 · `[primary]` `partly confirmed`
  Managing AI agents as first-class, credentialed, revocable identities. *Why it matters:* [agent identity](./concepts.md#glossary) is a prerequisite for enforcing constraints, attribution, and audit at the Governance Gate.

- **[Gartner Predicts 40% of Enterprise Apps Will Feature Task-Specific AI Agents by 2026](https://www.gartner.com/en/newsroom/press-releases/2025-08-26-gartner-predicts-40-percent-of-enterprise-apps-will-feature-task-specific-ai-agents-by-2026-up-from-less-than-5-percent-in-2025)** · Gartner · Aug 26, 2025 · `[primary]` `partly confirmed`
  Adoption forecast (40% by 2026, up from <5%) plus the caution that >40% of agentic projects will be canceled by 2027. *Why it matters:* the binding constraint is governance, cost, and reliable execution — not raw capability. (The widely cited "~88–89% of pilots fail to scale" figure comes from secondary trackers, not this release.)

- **[AI Agent Frameworks Compared: LangGraph vs CrewAI vs AutoGen (2026)](https://pecollective.com/blog/ai-agent-frameworks-compared/)** · LangChain (LangGraph), CrewAI · Apr 2026 · `[secondary]` `partly confirmed`
  Secondary comparison: LangGraph for controlled production (state persistence, human-in-the-loop checkpoints, rollback) vs. CrewAI for role-based prototyping. *Why it matters:* the control primitives — checkpoints, durable state, rollback — that governance gates hook into.

---

## 2. Agentic payments & settlement

The [Execution](./reference-architecture.md) rail for value, converging on scoped, cryptographic authority. Covered in [`agent-native-economy.md`](./agent-native-economy.md) and the worked instantiations in [`reference-architecture.md`](./reference-architecture.md).

- **[Stripe powers Instant Checkout in ChatGPT and releases the Agentic Commerce Protocol](https://stripe.com/newsroom/news/stripe-openai-instant-checkout)** · OpenAI, Stripe, Etsy, Shopify · Sep 29, 2025 · `[primary]` `confirmed`
  [ACP](./concepts.md#glossary) (Apache 2.0) and the [Shared Payment Token](./concepts.md#glossary) scoping an agent's authority to one merchant and cart total. *Why it matters:* the full reality → agent → execution loop at consumer scale inside an LLM interface.

- **[Google donates Agent Payments Protocol to FIDO Alliance](https://blog.google/products-and-platforms/platforms/google-pay/agent-payments-protocol-fido-alliance/)** · Google, FIDO Alliance, Mastercard · Apr 2026 · `[primary]` `confirmed`
  AP2 v0.2 with "Human Not Present" payments and cryptographically signed [Mandates](./concepts.md#glossary) as executable authorization; donated to a neutral body. *Why it matters:* the governance gate becoming shared public infrastructure.

- **[Announcing Agent Payments Protocol (AP2)](https://cloud.google.com/blog/products/ai-machine-learning/announcing-agents-to-payments-ap2-protocol)** · Google Cloud, Coinbase, Mastercard, PayPal, Amex · Sep 16, 2025 · `[primary]` `confirmed`
  Original AP2 announcement: Intent/Cart/Payment Mandates as W3C Verifiable Credentials across cards and stablecoins, with an A2A x402 extension. *Why it matters:* establishes mandates as the executable authorization primitive.

- **[Linux Foundation Launches the x402 Foundation](https://www.linuxfoundation.org/press/linux-foundation-is-launching-the-x402-foundation-and-welcoming-the-contribution-of-the-x402-protocol)** · Coinbase, Cloudflare, Linux Foundation, Visa, Mastercard, Google, AWS, Circle, Stripe · Apr 2, 2026 · `[primary]` `confirmed`
  [x402](./concepts.md#glossary) (HTTP-402 stablecoin micropayments) institutionalized as a neutral standard with 20+ founding members. *Why it matters:* the machine-to-machine settlement rail, embedded natively in the network protocol.

- **[Introducing the Machine Payments Protocol (Stripe)](https://stripe.com/blog/machine-payments-protocol)** · Stripe, Tempo, Paradigm · Mar 18, 2026 · `[primary]` `confirmed`
  Open HTTP-402 protocol with a "sessions" spend-cap primitive across cards/BNPL/stablecoins on the Tempo L1. *Why it matters:* vertical integration of execution and settlement for machine commerce.

- **[Visa launches Intelligent Commerce Connect for AI payments (The Paypers)](https://thepaypers.com/payments/news/visa-launches-intelligent-commerce-connect-for-agentic-payments)** · Visa (with AWS, Highnote, Mesh, Payabli) · Apr 2026 · `[secondary]` `confirmed`
  A network-, protocol- and token-vault-agnostic on-ramp giving agents one integration with spend controls across four agent protocols; GA targeted ~June 2026. *Why it matters:* consolidates the card-funded credential/spend-control layer into a network-neutral API.

- **[Mastercard Launches Agent Pay for Machines](https://www.mastercard.com/us/en/news-and-trends/press/2026/june/mastercard-launches-agent-pay-for-machines.html)** · Mastercard, Coinbase, Stripe, Adyen, Ripple, Polygon, Solana, Tempo, Skyfire · Jun 10, 2026 · `[primary]` `confirmed`
  Multi-rail machine-payment platform (cards/bank/stablecoins) with Agentic Tokens binding agent identity + consent + step-up rules; permissions recorded on-chain. *Why it matters:* inverts card tokenization to assume a pre-authorized agent rather than a present human.

- **[Google Shopping introduces Universal Cart, agentic shopping](https://blog.google/products-and-platforms/products/shopping/google-shopping-cart/)** · Google, Visa, Mastercard, Amex, Stripe, Adyen, PayPal · May 19, 2026 · `[primary]` `confirmed`
  Universal Cart plus the Universal Commerce Protocol, a merchant-facing schema layer over retail inventory. *Why it matters:* the "digital twin" of catalogs completing Google's decision → gate → execution stack.

- **[PayPal Launches Agentic Commerce Services to Power AI-Driven Shopping](https://newsroom.paypal-corp.com/2025-10-28-PayPal-Launches-Agentic-Commerce-Services-to-Power-AI-Driven-Shopping)** · PayPal, Google · Oct 28, 2025 · `[primary]` `confirmed`
  Agent Ready, Store Sync, and an open Agent Toolkit exposing a full commerce back office to agent frameworks; UCP support and PYUSD settlement. *Why it matters:* combines executable business logic with programmable value.

- **[Skyfire Demonstrates Secure Agentic Commerce Using KYAPay and Visa Intelligent Commerce](https://www.businesswire.com/news/home/20251218520399/en/Skyfire-Demonstrates-Secure-Agentic-Commerce-Purchase-Using-the-KYAPay-Protocol-and-Visa-Intelligent-Commerce)** · Skyfire, Visa, Consumer Reports · Dec 18, 2025 · `[primary]` `confirmed`
  KYAPay (USDC settlement) plus a ["Know Your Agent" (KYA)](./concepts.md#glossary) framework binding requests to a registered operator/user. *Why it matters:* the attestation layer for agent-to-agent settlement.

- **[Introducing Agentic Wallets (Coinbase Developer Platform)](https://www.coinbase.com/developer-platform/discover/launches/agentic-wallets)** · Coinbase Developer Platform · Feb 11, 2026 · `[primary]` `confirmed`
  MPC-secured, policy-controlled [agentic wallets](./concepts.md#glossary) with session caps, spend limits, guardrails, and native x402. *Why it matters:* governance constraints encoded directly into the wallet.

- **[Circle Launches AI Infrastructure to Power the Agentic Economy](https://www.circle.com/pressroom/circle-launches-ai-infrastructure-to-power-the-agentic-economy)** · Circle · May 11, 2026 · `[primary]` `confirmed`
  Circle Agent Stack: policy-controlled Agent Wallets, an Agent Marketplace, CLI, and gas-free USDC [nanopayments](./concepts.md#glossary) (~$0.000001). *Why it matters:* makes high-frequency M2M settlement economically viable.

- **[Agents that transact: Amazon Bedrock AgentCore Payments](https://aws.amazon.com/blogs/machine-learning/agents-that-transact-introducing-amazon-bedrock-agentcore-payments-built-with-coinbase-and-stripe/)** · AWS, Coinbase, Stripe · May 7, 2026 · `[primary]` `confirmed`
  Agentic stablecoin payment (x402 flow, USDC on Base, per-session limits and audit trails) embedded in a hyperscaler agent platform. *Why it matters:* mainstreams the pay-per-API/resource primitive into enterprise stacks.

- **[Everything we announced at Sessions 2026 (Stripe)](https://stripe.com/blog/everything-we-announced-at-sessions-2026)** · Stripe; partners OpenAI, Google, Meta, Microsoft · May 2026 · `[primary]` `confirmed`
  Agentic Commerce Suite: a per-task one-time-use Link wallet, MPP, Radar for agents, and streaming stablecoin micropayments. *Why it matters:* a major payments incumbent rebuilding around agents acting under scoped, delegated authority rather than as unbounded actors.

- **[Coinbase-backed AI payments protocol wants to fix micropayments but demand is just not there yet (CoinDesk)](https://www.coindesk.com/markets/2026/03/11/coinbase-backed-ai-payments-protocol-wants-to-fix-micropayment-but-demand-is-just-not-there-yet)** · Coinbase (x402 ecosystem), CoinDesk / Artemis · Mar 11, 2026 · `[secondary]` `partly confirmed` · **reality check**
  On-chain data showed x402 processing ~$28k/day despite a ~$7B ecosystem valuation, with roughly half of transactions artificial. *Why it matters:* the clearest distinction between deployed infrastructure and realized activity — do not overstate how "software-defined" the economy already is.

---

## 3. Governance & policy

The [Governance-Gate](./reference-architecture.md) node — runtime policy engines, identity/authorization standards, human-in-the-loop, and the regulatory layer. Covered in depth in [`governance-as-code.md`](./governance-as-code.md).

- **[Introducing the Agent Governance Toolkit (Microsoft)](https://opensource.microsoft.com/blog/2026/04/02/introducing-the-agent-governance-toolkit-open-source-runtime-security-for-ai-agents/)** · Microsoft · Apr 2, 2026 · `[primary]` `confirmed`
  MIT-licensed toolkit whose Agent OS intercepts every action before execution at sub-ms latency, with [execution rings and kill switches](./concepts.md#glossary) and EU AI Act/HIPAA/SOC2 mapping. *Why it matters:* a concrete governance gate — control moves from prompts to runtime infrastructure. (Some sub-packages and framework adapters vary across reporting and are unverified.)

- **[OWASP Top 10 for Agentic Applications](https://genai.owasp.org/2025/12/09/owasp-top-10-for-agentic-applications-the-benchmark-for-agentic-security-in-the-age-of-autonomous-ai/)** · OWASP GenAI Security Project · Dec 9, 2025 · `[primary]` `confirmed`
  The first agentic Top 10, led by [Agent Goal Hijack (ASI01)](./concepts.md#glossary). *Why it matters:* defines the adversarial threat model that motivates enforcement at the gate rather than trusting agent reasoning.

- **[The 2026-07-28 MCP Specification Release Candidate](https://blog.modelcontextprotocol.io/posts/2026-07-28-release-candidate/)** · Model Context Protocol project (Anthropic-originated) and community · Jul 2026 · `[primary]` `partly confirmed`
  Stateless core, Extensions/Tasks, OAuth 2.1/OIDC hardening, and Multi Round-Trip Requests (SEP-2322) standardizing human-in-the-loop for high-risk actions. *Why it matters:* the [protocol](./concepts.md#glossary) itself can halt and require human confirmation before an agent acts.

- **[Announcing the AI Agent Standards Initiative (NIST)](https://www.nist.gov/news-events/news/2026/02/announcing-ai-agent-standards-initiative-interoperable-and-secure)** · NIST, CAISI, NCCoE · Feb 17, 2026 · `[primary]` `confirmed`
  An agent identity/authorization/security initiative; the NCCoE concept paper proposes OAuth 2.0 + SPIFFE/SPIRE + MCP for authenticating autonomous agents. *Why it matters:* the identity substrate needed for agents to transact and be held accountable.

- **[AI Agents in Action: A Playbook for Trusted Adoption, Authorization and Scaling (WEF)](https://www.weforum.org/publications/ai-agents-in-action-a-playbook-for-trusted-adoption-authorization-and-scaling/)** · World Economic Forum, Capgemini · May 26, 2026 · `[primary]` `confirmed`
  Introduces the [Agent Capability and Authorization Profile (ACAP)](./concepts.md#glossary) — a single auditable record of an agent's delegated power. *Why it matters:* a canonical governance-gate artifact bounding what an agent may do before it acts.

- **[New Model AI Governance Framework for Agentic AI (IMDA)](https://www.imda.gov.sg/resources/press-releases-factsheets-and-speeches/press-releases/2026/new-model-ai-governance-framework-for-agentic-ai)** · Singapore IMDA / AI Verify Foundation · Jan 22, 2026 · `[primary]` `partly confirmed`
  A risk-factor framework (reversibility, autonomy level, data sensitivity, read vs. write) for encoding autonomy limits as auditable constraints. *Why it matters:* a blueprint for mapping risk tiers to gate strictness. (The "ACT-1..ACT-4" autonomy tiers are a separate vendor construct, not part of this framework, and are excluded.)

- **[Enforcement of Chapter V under the EU AI Act](https://artificialintelligenceact.eu/enforcement-of-chapter-v-under-the-eu-ai-act/)** · European Commission, EU AI Office · Aug 2026 · `[secondary]` `confirmed`
  [GPAI](./concepts.md#glossary) obligations have applied since Aug 2, 2025; Commission enforcement powers (including fines) become exercisable Aug 2, 2026. *Why it matters:* a binding legal governance gate upstream of any agent execution loop. (Explainer of the primary legal text.)

- **[Council gives final green light to simplify and streamline rules (Digital Omnibus)](https://www.consilium.europa.eu/en/press/press-releases/2026/06/29/artificial-intelligence-council-gives-final-green-light-to-simplify-and-streamline-rules/)** · Council of the EU, European Parliament, European Commission · Jun 29, 2026 · `[primary]` `confirmed`
  Defers stand-alone high-risk (Annex III) obligations to Dec 2, 2027 and product-embedded (Annex I) to Aug 2, 2028; GPAI enforcement timing unchanged. *Why it matters:* signals the machine-checkable technical standards a policy engine would consume are not yet ready.

- **[The General-Purpose AI Code of Practice](https://digital-strategy.ec.europa.eu/en/policies/contents-code-gpai)** · EU AI Office; Anthropic, OpenAI, Google, Microsoft, IBM, Amazon, xAI · Jul 10, 2025 · `[primary]` `confirmed`
  Operationalizes GPAI obligations (transparency, copyright, safety) into a concrete compliance instrument. *Why it matters:* a de facto specification that downstream governance-as-code tooling maps against.

- **[Understanding ISO 42001: The World's First AI Management System Standard (A-LIGN)](https://www.a-lign.com/articles/understanding-iso-42001)** · ISO/IEC; BSI, Schellman, A-LIGN · 2026 · `[secondary]` `confirmed`
  ISO/IEC 42001 AIMS certification entering its first growth wave; referenced by EU AI Act conformity assessment. *Why it matters:* auditable management-system scaffolding linking organizational governance to executable controls.

- **[Why Open Policy Agent is the Missing Guardrail for Your AI Agents (Codilime)](https://codilime.com/blog/why-use-open-policy-agent-for-your-ai-agents/)** · Open Policy Agent community, Strata Identity · 2026 · `[secondary]` `partly confirmed`
  The pattern of enforcing [OPA / Policy-as-Code](./concepts.md#glossary) at the API/MCP-gateway layer so a hijacked agent is blocked before reaching upstream systems. *Why it matters:* the SDE gate rendered as deterministic policy-as-code.

---

## 4. Authority, identity & assurance standards

The standards and institutional sources behind Open-SDE's **assured bounded autonomy** frame — who grants an agent authority, how that authority is bounded and revoked, and how probabilistic reasoning is separated from deterministic authorization, settlement, and accountability. These anchor the [Governance-Gate](./reference-architecture.md) as a **PDP + PEP** split and the runtime-assurance loop. Covered in depth in [`authority-and-safety-model.md`](./authority-and-safety-model.md) and the [`sde-0-conformance-profile.md`](./sde-0-conformance-profile.md).

- **[OpenID AuthZEN Authorization API 1.0](https://openid.net/specs/authorization-api-1_0.html)** · OpenID Foundation, AuthZEN Working Group · Jan 11, 2026 (approved as a Final Specification Jan 12, 2026) · `[primary]` `confirmed` · **Status: Final** · **PDP/PEP anchor**
  A transport-agnostic authorization API with a normative HTTPS/JSON binding that lets a [Policy Enforcement Point (PEP)](./concepts.md#glossary) ask a [Policy Decision Point (PDP)](./concepts.md#glossary) for an access decision (Access Evaluation, Access Evaluations, and Search endpoints). *Why it matters:* the published industry standard for splitting the governance gate into a component that **decides** (PDP) and one that **enforces** (PEP). The PDP/PEP pattern originates in the ABAC/XACML tradition; AuthZEN standardizes the API between them, so mandate evaluation can sit outside the AI and be enforced at distributed points.

- **[NIST AI Agent Standards Initiative](https://www.nist.gov/artificial-intelligence/ai-agent-standards-initiative)** · NIST Center for AI Standards and Innovation (CAISI) with the Information Technology Laboratory (ITL) · Feb 17, 2026 · `[primary]` `confirmed` · **Status: initiative (not a published standard)**
  A three-pillar effort — (1) industry-led agent standards and U.S. leadership in international bodies, (2) community-led open-source protocol development, (3) research in agent security and identity — accompanied by CAISI's RFI on AI Agent Security and ITL's *AI Agent Identity and Authorization Concept Paper*. *Why it matters:* a federal anchor for the claim that agent identity and mandate-scoped authorization are an active, official standardization thread, mapping directly onto the PDP/PEP and delegated-authority model. (A separate news-release URL is [also live](https://www.nist.gov/news-events/news/2026/02/announcing-ai-agent-standards-initiative-interoperable-and-secure).)

- **[NIST AI 800-4: Challenges to the Monitoring of Deployed AI Systems](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.800-4.pdf)** · NIST CAISI · Mar 6, 2026 · `[primary]` `confirmed` · **Status: Final report**
  A final report synthesizing three practitioner workshops (200+ experts) and an 87-paper review, proposing **six** post-deployment monitoring categories: Functionality, Operational, Human Factors, Security, Compliance, and Large-Scale Impacts. *Why it matters:* federal backing for continuous monitoring as a complement to pre-deployment evaluation — the ongoing-oversight leg of SDE-0's reconciliation and monitoring requirements, not a one-time gate.

- **[ITU-T Focus Group on Embodied AI for Multimedia Technologies (FG-EAI)](https://www.itu.int/en/ITU-T/focusgroups/eai/Pages/default.aspx)** · ITU-T, established by Study Group 21 · Feb 19, 2026 · `[primary]` `confirmed` · **Status: Focus Group (pre-standardization)**
  An ITU-T Focus Group studying embodied AI, whose foundational technologies of study explicitly include **closed-loop control** and world-model training, with dedicated working groups on systems integration/interfaces and on evaluation/benchmarking. *Why it matters:* an international-standards anchor for closed-loop control and benchmarking of physical-domain agents — the [Authoritative State Model](./concepts.md#glossary)'s physical (Digital-Twin) specialization and its runtime-assurance loop.

- **[ITU-T Focus Group on Trust and Identity for Humans and Agentic AI (FG-TIDA)](https://www.itu.int/en/mediacentre/Pages/PR-2026-07-09-focus-group-agentic-AI.aspx)** · ITU-T, under Study Group 17 (security) · Announced Jul 9, 2026; first meeting Paris, Nov 2026 · `[primary]` `confirmed` · **Status: Focus Group (pre-standardization, exploratory)**
  A newly established Focus Group on trust management and interoperable identity for both humans and agentic AI; its scope covers identity/discovery reference architectures, trust-lifecycle (assurance) models, "security criteria and benchmarks for the continuous assessment of AI agents," and preserving "meaningful human control" for high-stakes tasks such as financial transactions and critical infrastructure. *Why it matters:* institutional convergence on identity-plus-trust-lifecycle governance for delegated agents — but a Focus Group is a pre-standardization body with published intent, not a binding Recommendation.

- **[IMF Note 2026/004: How Agentic AI Will Reshape Payments](https://www.imf.org/en/publications/imf-notes/issues/2026/04/22/how-agentic-ai-will-reshape-payments-575560)** · International Monetary Fund; Sonja Davidovic and Hervé Tourpe · Apr 22, 2026 · `[primary]` `confirmed` · **Status: published IMF Note** · **architecture anchor**
  Proposes a three-layer framework that concentrates probabilistic reasoning in **intent formation and orchestration** while keeping **authorization/control** and **settlement** strictly deterministic, arguing core rails should stay "dumb" and rules-bound while intelligence sits upstream. *Why it matters:* the authoritative institutional analogue to Open-SDE's central commitment — separate probabilistic judgment from deterministic authorization, settlement, and accountability at the point of irreversible action.

- **[ASTM F3269-21: Safely Bounding Behavior of Aircraft Systems Containing Complex Functions Using Run-Time Assurance](https://store.astm.org/f3269-21.html)** · ASTM International (F38 Committee); Lui Sha (Simplex Architecture); DARPA (Assured Autonomy program, 2018) · 2021 (F3269-17 in 2017; Simplex, IEEE Software 2001) · `[primary]` `confirmed` · **Status: active standard** · **runtime-assurance precedent**
  Codifies an established safety-engineering pattern: a verified safety monitor bounds an untrusted/complex (e.g. AI/ML) function and reverts to a verified-safe mode when a violation is imminent — the Simplex Architecture, "using simplicity to control complexity." *Why it matters:* Open-SDE inherits runtime assurance from mature aviation-certification and DARPA practice rather than inventing it; the verified monitor bounding an untrusted function maps onto a PEP enforcing PDP-issued mandates and reverting to a safe fallback.

---

## 5. Physical execution & DePIN

The Reality-Signals → [Digital-Twin](./concepts.md#glossary) → physical Execution legs — twins, embodied agents, and DePIN markets, with the widest proven-vs-promotional gap. Covered in [`reality-anchored-execution.md`](./reality-anchored-execution.md).

- **[NVIDIA Releases Vera Rubin DSX Reference Design and Omniverse DSX Digital Twin Blueprint](https://nvidianews.nvidia.com/news/nvidia-releases-vera-rubin-dsx-ai-factory-reference-design-and-omniverse-dsx-digital-twin-blueprint-with-broad-industry-support)** · NVIDIA + Siemens, Schneider Electric, Dassault, Cadence, PTC, Eaton, Vertiv, others · Mar 16, 2026 · `[primary]` `confirmed`
  GA of physically accurate AI-data-center digital twins for simulating power/cooling/operations before construction. *Why it matters:* a canonical Digital Twin layer where policies are validated against physics before physical execution. (Specific DSX submodule names in some reporting are unverified.)

- **[Gemini Robotics 1.5 brings AI agents into the physical world (Google DeepMind)](https://deepmind.google/blog/gemini-robotics-15-brings-ai-agents-into-the-physical-world/)** · Google DeepMind · Sep 25, 2025 · `[primary]` `confirmed`
  An embodied-reasoning "brain" paired with a [VLA](./concepts.md#glossary) action model showing cross-embodiment transfer. *Why it matters:* the Agent-Decision layer for physical execution — one policy governing heterogeneous robot bodies.

- **[Into the Omniverse: NVIDIA GTC Showcases Virtual Worlds Powering the Physical AI Era](https://blogs.nvidia.com/blog/gtc-2026-virtual-worlds-physical-ai/)** · NVIDIA; Physical Intelligence · Mar 2026 · `[primary]` `confirmed`
  The physical-AI stack: Cosmos world models, Isaac GR00T, Alpamayo, and the Mega Omniverse Blueprint for validating multi-robot policies in twins before deployment. *Why it matters:* world models as the rehearsal environment reducing the risk of autonomous physical action.

- **[F.02 Contributed to the Production of 30,000 Cars at BMW (Figure AI)](https://www.figure.ai/news/production-at-bmw)** · Figure AI, BMW · H1 2026 · `[primary]` `partly confirmed`
  Figure 02 (~1,250 hours, 90,000+ components, 30,000+ vehicles) moving to a paid Figure 03 commercial contract. *Why it matters:* AI-directed physical labor becoming a schedulable, software-managed resource. (Broader industry unit/fleet counts vary widely by source and are uncertain.)

- **[Amazon deploys over 1 million robots and launches new AI foundation model](https://www.aboutamazon.com/news/operations/amazon-million-robots-ai-foundation-model)** · Amazon (DeepFleet) · Jul 2025 · `[primary]` `partly confirmed`
  1M+ robots across 300+ facilities; the DeepFleet generative-AI orchestrator (~10% travel-time gain). *Why it matters:* logistics as a software-orchestrated cyber-physical system. (Wider secondary figures — global robot totals, driverless-truck counts — are unverified.)

- **[DePIN Compute Revenue Pivot: Akash, io.net, Aethir (BlockEden)](https://blockeden.xyz/blog/2026/03/12/depin-compute-revenue-pivot-akash-ionet-aethir/)** · Render, Hivemapper, Aethir; sector aggregators · Mar 12, 2026 · `[secondary]` `partly confirmed`
  The [DePIN](./concepts.md#glossary) "mining to serving" pivot with real external revenue (Render ~$38M/mo, Hivemapper ~36×, Aethir ~$166M ARR). *Why it matters:* protocol-governed markets clearing against real demand — but **sector market-cap and device figures ($40B–$900B+) are inconsistent across promotional secondary sources and should be treated as uncertain.**

- **[Peaq showcases delivery robot making on-chain payments using USDT (Cryptonews)](https://cryptonews.net/news/altcoins/32906537/)** · peaq, Serve Robotics, Tether; LG (CLOi/CLOiSim) · May 12, 2026 · `[secondary]` `partly confirmed`
  A robot holding a wallet, navigating, and settling a delivery in USDT on-chain. *Why it matters:* the full loop among machines — but **in simulation, not live street deployment.** (A related "Initial Machine Offerings" / robot-RWA tokenization claim could not be verified and is excluded.)

---

## 6. On-chain machine economy & RWA

Agent identity, on-chain settlement measurement, and the tokenized-asset substrate. Covered in [`reality-anchored-execution.md`](./reality-anchored-execution.md) and [`agent-native-economy.md`](./agent-native-economy.md).

- **[AI agents are starting to pay with crypto (Keyrock report, CoinDesk)](https://www.coindesk.com/business/2026/05/21/crypto-rails-are-becoming-the-default-payment-layer-for-ai-agents-report-says)** · Keyrock, CoinDesk · May 21, 2026 · `[secondary]` `confirmed` · **empirical anchor**
  Agents settled ~176M on-chain transactions worth >$73M between May 2025 and April 2026, 98.6% in USDC, with ~76% of payments below the ~30-cent card-fee floor. *Why it matters:* the first hard measurement of realized agent settlement — real, but small, and concentrated in a single stablecoin.

- **[ERC-8004: Trustless Agents (Ethereum ERC)](https://eips.ethereum.org/EIPS/eip-8004)** · Marco De Rossi, Davide Crapis, Jordan Ellis, Erik Reppel; Ethereum ERC process · Created Aug 13, 2025; still Draft as of mid-2026 · `[primary]` `confirmed` · **Status: Draft**
  On-chain Identity/Reputation/Validation registries giving agents ERC-721 identities (with stake-secured re-execution, zkML, and TEE-oracle validation); reference registries were deployed to mainnet with tens of thousands of agents registered, but the specification itself remains **Draft** (Standards Track, Category ERC) and unfinalized. *Why it matters:* a candidate decentralized identity/validation substrate for agent-to-agent commerce — cite it as an emerging, unfinished proposal, not a settled standard, and do not build enforcement guarantees on its wire format as if stable.

- **[Tokenized RWAs Grew From $6B To $31B (Yellow research)](https://yellow.com/research/tokenized-rwas-31b-market-growth-real-race-starting)** · RWA.xyz-tracked issuers · May 2026 · `[secondary]` `confirmed`
  Ex-stablecoin [tokenized RWAs](./concepts.md#glossary) ~$32B (up >200% YoY), led by Treasuries (~$15B), private credit (~$6.2B), and gold (~$4.7B). *Why it matters:* the yield-bearing, compliance-gated asset substrate agents transact over.

- **[BlackRock deepens tokenization push with new on-chain fund offerings (CoinDesk)](https://www.coindesk.com/business/2026/05/09/blackrock-deepens-tokenization-push-with-new-onchain-fund-offerings)** · BlackRock, Securitize, SEC · May 9, 2026 · `[secondary]` `confirmed`
  BUIDL (~$2.4–2.5B AUM) plus two new tokenized structures filed. *Why it matters:* compliance-gated, yield-bearing collateral bridging regulated finance into agent-executable rails.

- **[SEC and CFTC release joint interpretation on crypto asset regulation (Norton Rose Fulbright)](https://www.nortonrosefulbright.com/en/knowledge/publications/a88b661b/sec-and-cftc-release-joint-interpretation-on-crypto-asset-regulation)** · U.S. SEC, U.S. CFTC · Mar 17, 2026 · `[secondary]` `confirmed`
  The first coordinated federal five-category crypto classification. *Why it matters:* a prerequisite governance gate for which tokenized assets agents can legally hold and settle. (Law-firm analysis of the primary interpretation.)

- **[Agent-to-Agent Finance: Blockchain Payments and Trust Infrastructure for Autonomous AI Agents (arXiv)](https://arxiv.org/abs/2607.00245)** · Academic researchers · Jul 2026 · `[primary]` `partly confirmed`
  Formalizes the identity/authorization/payment/reputation stack for autonomous agents. *Why it matters:* academic context for agent-to-agent labor markets and DeFi agent vaults. (Associated third-party TVL/vault figures are secondary and uncertain.)

---

## 7. Concept & thought leadership

Positioning: the literal term "Software Defined Economy" is niche, and mainstream discourse uses adjacent labels. Covered in [`concepts.md`](./concepts.md).

- **[Big Ideas 2026 (Andreessen Horowitz)](https://a16z.com/newsletter/big-ideas-2026-part-1/)** · a16z and a16z crypto · Dec 2025 · `[primary]` `confirmed`
  Frames the "agentic economy" and "building for agents, not humans"; introduces "Know Your Agent" (KYA) as the bottleneck shifts "from intelligence to identity." *Why it matters:* the leading venture articulation of the governance-gate-as-identity thesis.

- **[Gartner Unveils Top Emerging Technologies to Support Autonomous Business](https://www.gartner.com/en/newsroom/press-releases/2025-09-10-gartner-unveils-top-emerging-technologies-to-support-autonomous-business)** · Gartner · Sep 10, 2025 · `[primary]` `confirmed`
  The "programmable economy" and ["machine customers"](./concepts.md#glossary) (influencing ~$30T of purchases by 2030), plus a caution that autonomous-business ROI is unproven. *Why it matters:* the analyst-side synonym for SDE, with a built-in counter-signal.

- **[Agentic commerce: How AI shopping agents can change retail (McKinsey)](https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-automation-curve-in-agentic-commerce)** · McKinsey (QuantumBlack) · Oct 2025 · `[primary]` `confirmed`
  Sizes agentic commerce at $3–5T globally by 2030 and stresses the "agent-ready" / machine-readable-catalog precondition. *Why it matters:* mainstream market-sizing plus the machine-legibility prerequisite for agents to act.

- **[OpenStack: We're Living in a Software Defined Economy (OpenStack blog)](https://www.openstack.org/blog/openstack-taking-its-place-in-the-software-defined-economy/)** · OpenStack / Jonathan Bryce · 2014 · `[primary]` `partly confirmed`
  The most-cited older articulation of the literal "Software Defined Economy" term (a cloud framing). *Why it matters:* positioning context — the label is niche and available, leaving naming space for Open-SDE while it maps onto dominant vocabulary.

---

## Excluded and unverifiable claims

Recorded so future contributors do not reintroduce them. Each was present in earlier drafts or circulating secondary reporting and was dropped or corrected during fact-checking:

- **Autonomous AI grid operator** — claims of an AI acting as an autonomous power-grid operator with "self-healing grids" (~80% shorter outages) were sourced only to a promotional content-farm article with no primary corroboration. Virtual power plants exist as a concept; the autonomous-operator framing does not. **Excluded.**
- **OpenAI "Lockdown Mode" (June 2026)** and an Agent Builder/Evals wind-down — not independently verifiable. **Excluded.**
- **A2A "v1.2" / "Agentic AI Foundation"** — contradicted by the Linux Foundation press release, which states v1.0 under the Linux Foundation. **Corrected.**
- **Specific METR statistics** — precise task counts (~228 tasks, ~31 over 8h), an exact ~3h 80% horizon, and a 4–4.3-month doubling figure are not stated on METR's page (which cites "over a hundred" tasks and gives no explicit doubling figure). **Excluded.**
- **"ACT-1..ACT-4" autonomy tiers** attributed to Singapore's framework — actually a separate vendor construct, not part of the IMDA framework. **Excluded.**
- **DePIN sector market-cap and device totals** ($40B–$900B+, global device counts) — inconsistent across promotional secondary sources. **Treated as uncertain, not asserted.**
- **peaq "Initial Machine Offerings" / CoinList robot-RWA tokenization** — could not be verified. **Excluded.**
- **"MSCI Software Index down ~21% YTD"** market-move claim, and unverified WEF "$236B by 2034" / Gartner "$206.5B 2026" sub-figures — no primary source. **Excluded.**
- **[arXiv 2411.05087](https://arxiv.org/abs/2411.05087) as conceptual support for a "software-defined economy"** — Brown et al., "Measuring Software Innovation with Open Source Software Development Data," is an empirical software-metrics study (GitHub releases, semantic versioning) that uses the phrase "software-defined economy" exactly once, as incidental framing ("an increasingly software-defined economy"). It does not define, theorize, or engage the concept and must **not** be cited as a foundation for Open-SDE. "Software-Defined Economy" is Open-SDE's own working term, not an established standard term; this paper is at most an incidental prior usage of the phrase. **Excluded as a foundation.**
- **ISO "EN ISO/IEC 42001:2026" European-adoption details, a "KPMG" benchmark, and a BigID "47% of orgs" statistic** — not verifiable from primary sources. **Excluded.**

---

## Related

- [`landscape-2026.md`](./landscape-2026.md) — the flagship synthesis of what is actually shipping.
- [`concepts.md`](./concepts.md#glossary) — definitions and the glossary these entries link to.
- [`agent-native-economy.md`](./agent-native-economy.md), [`reality-anchored-execution.md`](./reality-anchored-execution.md), [`governance-as-code.md`](./governance-as-code.md) — the three research areas.
- [`reference-architecture.md`](./reference-architecture.md) — the SDE reference loop expanded.
- [`../ROADMAP.md`](../ROADMAP.md) — open questions this evidence base feeds.

This document *is* the full, annotated source list for Open-SDE. Every dated claim elsewhere in the repository should trace back to an entry here; if it does not, treat it as unverified.
