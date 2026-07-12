# The Software-Defined Economy in Mid-2026: What Is Actually Shipping

*A dated, cited survey of what has moved from research demo to production infrastructure across the SDE reference loop — and what has not.*

*Last updated: July 2026 · Part of the [Open-SDE](../README.md) research initiative.*

---

## How to read this document

This is the flagship "latest developments" survey for Open-SDE. Its discipline is
simple and deliberate: **every non-obvious factual claim carries an inline link to a
primary source, and states an approximate date in prose.** Where the underlying
research could only partly corroborate a claim, the language hedges accordingly
("reportedly", "as of this writing"). Claims the fact-check could not verify are
excluded rather than softened.

The organizing frame is the [SDE reference loop](./reference-architecture.md):

```
Reality Signals → Digital Twin → Agent Decision → Governance Gate → Execution
```

The single most important reading of mid-2026 is a distinction, not a headline:
**the rails, primitives, and standards for the loop now largely exist, while realized,
non-synthetic economic activity through them is still early.** The gap between
*deployed infrastructure* and *clearing demand* is the thing this document works
hardest not to overstate.

A positioning note also matters. The literal phrase "Software-Defined Economy" remains
niche; mainstream 2025–2026 discourse consolidated instead around the "agentic economy"
([a16z](https://a16z.com/newsletter/big-ideas-2026-part-1/), Stripe, Circle, WEF), the
"programmable economy" and "machine customers"
([Gartner](https://www.gartner.com/en/newsroom/press-releases/2025-09-10-gartner-unveils-top-emerging-technologies-to-support-autonomous-business)),
and the "agent economy" (McKinsey, WEF). Open-SDE keeps its distinctive emphasis on
reality-anchored execution and governance-as-code while mapping onto that vocabulary.
See [concepts.md](./concepts.md) for the full positioning.

---

## Executive summary: real vs. early

The table separates what is **real and shipping** from what is **early or speculative**,
keyed to the three core research areas and the loop stages they touch. Every row is
substantiated in the sections below.

| SDE layer | Real and shipping (mid-2026) | Early or speculative |
| --- | --- | --- |
| **Agent-Native Economy** (Agent Decision) | Production agent SDKs with a standardized *harness* layer (Microsoft Agent Framework 1.0 GA, OpenAI's sandboxed harness, Claude Agent SDK / Managed Agents, Google ADK); A2A v1.0 under the Linux Foundation; rising long-horizon capability | Autonomous agents owning end-to-end multi-hour workflows *reliably*; ~88–89% of pilots still fail to reach production |
| **Governance as Code** (Governance Gate) | Runtime policy engines (Microsoft Agent Governance Toolkit, OPA at the MCP gateway); protocol-level human-in-the-loop (MCP 2026-07-28 RC); codified threat model (OWASP Top 10 for Agentic Applications); a *Final* PDP/PEP authorization API (OpenID AuthZEN 1.0); federal agent-identity work (NIST AI Agent Standards Initiative); a *final* post-deployment monitoring report (NIST AI 800-4) | Regulatory obligations as *machine-checkable* standards; liability attribution when a passing gate still yields harm; sub-ms engines without single points of failure; identity registries built on still-*Draft* specs (ERC-8004) |
| **Reality-Anchored Execution** (Twin → Execution) | Industrial digital twins GA (NVIDIA Omniverse DSX); VLA robot foundation models; first *paid* embodied deployments (Figure at BMW); fleet orchestration (Amazon DeepFleet); DePIN revenue pivot | Fully autonomous physical execution with *certified* governance; machine-to-machine on-chain settlement on live streets (peaq/Serve is simulation-only) |
| **Payments & Settlement** (Execution) | Card-network agent products (Visa, Mastercard); open protocols (ACP, AP2, x402, MPP); stablecoin micropayment rails; hyperscaler integration (AWS Bedrock AgentCore Payments) | Organic (non-gamed) transaction demand at scale; convergence of competing stacks into one interoperable trust fabric |
| **On-chain Machine Economy** (Execution) | Measured agent settlement (~176M tx, >$73M, 98.6% USDC); ERC-8004 reference registries live on mainnet (spec still *Draft*); tokenized RWAs ~$32B ex-stablecoins | End-to-end integration of the five loop stages (still largely siloed pilots); thin organic x402 demand (~$28k/day, much test/gamed); ERC-8004 finalization |

---

## 1. Agent-Native Economy: the Agent-Decision node became productized

The clearest "real and shipping" story of the year is that agent runtimes consolidated
around a small set of production SDKs that now bundle an
[agent harness](./concepts.md#glossary) — context compaction, memory, file/tool access,
sandboxed execution, and middleware — rather than exposing a bare model API.

- In early April 2026, Microsoft shipped **Agent Framework 1.0** to general
  availability, converging AutoGen's agent abstractions and Semantic Kernel's
  enterprise features into one supported platform for .NET and Python, with native MCP
  and A2A support ([Microsoft Learn](https://learn.microsoft.com/en-us/agent-framework/overview/)).
  At Build 2026 (May) Microsoft added the harness itself, session-isolated **Foundry
  Hosted Agents**, and **[CodeAct](./concepts.md#glossary)** — collapsing a multi-step
  plan into a single sandboxed program run in an isolated micro-VM
  ([Microsoft DevBlogs](https://devblogs.microsoft.com/agent-framework/microsoft-agent-framework-at-build-2026-announce/)).
  CodeAct pushes "decisions as executable code," the core SDE premise, into the action
  layer itself.
- On April 15, 2026, OpenAI shipped the next evolution of its **Agents SDK**: a
  model-native, sandboxed harness for long-horizon tasks with configurable memory and
  declarative guardrails
  ([OpenAI](https://openai.com/index/the-next-evolution-of-the-agents-sdk/)).
  (A separately reported June "Lockdown Mode" could not be verified and is excluded.)
- On May 6, 2026, Anthropic detailed how the **Claude Agent SDK** underpins hosted
  **Managed Agents**, adding a scheduler, a memory-curating "dreaming" pass, and
  outcomes-based grading
  ([Anthropic](https://claude.com/blog/new-in-claude-managed-agents)).
- Google's **Agent Development Kit** matured across languages, with **ADK Go 1.0**
  shipping in 2026 and Agent Engine deployment plus native A2A support
  ([Google Developers Blog](https://developers.googleblog.com/adk-go-10-arrives/)).
  (Reporting on a single "all four languages at Cloud Next" moment overstates a messier
  reality, so it is not repeated here.)

Cross-organization interoperability advanced in parallel. The **A2A (Agent2Agent)
protocol** reached a stable v1.0 under the Linux Foundation, adding cryptographically
signed **Agent Cards** for domain trust and surpassing 150 organizations in production,
per an April 9, 2026 Linux Foundation release
([Linux Foundation](https://www.linuxfoundation.org/press/a2a-protocol-surpasses-150-organizations-lands-in-major-cloud-platforms-and-sees-enterprise-production-use-in-first-year)).
(Draft references to an "Agentic AI Foundation" or an A2A "v1.2" contradict the primary
source and are excluded.)

**Capability, measured.** The best public proxy for *how much of a workflow an agent can
own* is METR's task-completion time-horizon benchmark. As of its May 8, 2026 update,
METR places frontier 50%-reliability horizons in the multi-hour range, and broader
reporting describes the historical ~7-month doubling as compressing
([METR](https://metr.org/time-horizons/)). Two caveats are load-bearing and are stated
plainly here: METR flags measurements **above 16 hours as unreliable** on its current
suite, and the specific task counts and doubling figures circulated elsewhere are **not
stated on METR's page** and are not used in this document.

**The binding constraint is not capability.** The disciplined counter-signal is
adoption. Gartner's August 2025 forecast — ~40% of enterprise apps featuring
task-specific agents by 2026, up from <5% in 2025 — sits alongside its projection that
**>40% of agentic-AI projects will be canceled by 2027** over cost, unclear ROI, and
weak risk controls
([Gartner](https://www.gartner.com/en/newsroom/press-releases/2025-08-26-gartner-predicts-40-percent-of-enterprise-apps-will-feature-task-specific-ai-agents-by-2026-up-from-less-than-5-percent-in-2025)).
Secondary 2026 trackers report that roughly **88–89% of agent pilots never reach
production** (a figure that comes from those roundups, not the Gartner release). The
implication for the SDE is direct: the bottleneck is governance, cost control, and
reliable execution — the Governance-Gate node — not raw model horsepower.

> **Concentration risk to watch.** An economy whose executable rules run on a handful
> of proprietary agent runtimes carries lock-in and systemic-risk exposure that this
> repo treats as an open question rather than a settled good. See
> [agent-native-economy.md](./agent-native-economy.md).

---

## 2. Governance as Code: the gate became runtime infrastructure

This is where the year's maturation was fastest and most concrete, and where
"governance as code" stopped being a slogan.

- On April 2, 2026, Microsoft open-sourced the MIT-licensed **Agent Governance
  Toolkit**, whose *Agent OS* policy engine intercepts every agent action **before
  execution at sub-millisecond latency**, with CPU-style **execution rings** and a
  **kill switch**, plus compliance mappings to the EU AI Act, HIPAA, and SOC 2
  ([Microsoft Open Source](https://opensource.microsoft.com/blog/2026/04/02/introducing-the-agent-governance-toolkit-open-source-runtime-security-for-ai-agents/)).
  (Some sub-packages and the exact framework-adapter list vary across reporting and are
  not asserted here.)
- The recommended containment pattern shifted to enforcing
  **[Open Policy Agent](./concepts.md#glossary)** at the tool-calling / MCP-gateway
  layer, so the policy engine — not the agent — decides what is permitted, and even a
  hijacked agent is blocked before reaching upstream systems
  ([Codilime](https://codilime.com/blog/why-use-open-policy-agent-for-your-ai-agents/)).
- Human-in-the-loop moved into the protocol. The **MCP 2026-07-28 release candidate**
  standardizes confirmation via **Multi Round-Trip Requests (SEP-2322)** — a server
  returns an `InputRequiredResult` and the client gathers user input before re-issuing
  the call — alongside OAuth 2.1/OIDC authorization hardening
  ([MCP blog](https://blog.modelcontextprotocol.io/posts/2026-07-28-release-candidate/)).
  As a release candidate, this is described here as in-progress rather than final.

The **threat model** is now codified. On December 9, 2025, the OWASP GenAI Security
Project published its first **Top 10 for Agentic Applications**, led by **Agent Goal
Hijack (ASI01)** — attackers concealing instructions in documents, emails, or RAG
content to redirect an autonomous agent's objective
([OWASP](https://genai.owasp.org/2025/12/09/owasp-top-10-for-agentic-applications-the-benchmark-for-agentic-security-in-the-age-of-autonomous-ai/)).
The lesson threaded through the whole governance layer: enforce at the gate, do not
trust the agent's own reasoning.

**Identity and authorization instruments** proliferated. NIST's Center for AI Standards
and Innovation launched an **AI Agent Standards Initiative** on February 17, 2026, with
an NCCoE concept paper proposing agent identity via OAuth 2.0 + SPIFFE/SPIRE + MCP
([NIST](https://www.nist.gov/news-events/news/2026/02/announcing-ai-agent-standards-initiative-interoperable-and-secure)).
The WEF and Capgemini introduced the **Agent Capability and Authorization Profile
(ACAP)** on May 26, 2026 — a single auditable record of an agent's delegated power
([WEF](https://www.weforum.org/publications/ai-agents-in-action-a-playbook-for-trusted-adoption-authorization-and-scaling/)).
Microsoft **Entra Agent ID** reached general availability (reported around April–May
2026), managing agents as first-class, credentialed, revocable identities
([Microsoft Learn](https://learn.microsoft.com/en-us/entra/agent-id/whats-new-agent-id)).

**The regulatory layer is real but still calibrating.** EU AI Act GPAI obligations have
applied since August 2, 2025, and the Commission's supervision and enforcement powers
(including fines, via the AI Office) become exercisable **August 2, 2026**
([artificialintelligenceact.eu](https://artificialintelligenceact.eu/enforcement-of-chapter-v-under-the-eu-ai-act/)).
Yet the **Digital Omnibus** package — given final Council green light on June 29, 2026 —
defers most stand-alone high-risk (Annex III) obligations to December 2, 2027 and
product-embedded (Annex I) high-risk to August 2, 2028, leaving GPAI enforcement timing
intact
([Council of the EU](https://www.consilium.europa.eu/en/press/press-releases/2026/06/29/artificial-intelligence-council-gives-final-green-light-to-simplify-and-streamline-rules/)).
The tell for the SDE: the machine-checkable technical standards (CEN-CENELEC) that a
policy-as-code engine would consume are **not yet ready**, so conformity remains largely
document-and-audit based. Supporting scaffolding is arriving — **ISO/IEC 42001** AI
management-system certification entered its first growth wave
([A-LIGN](https://www.a-lign.com/articles/understanding-iso-42001)) — but the open
question is whether these obligations gain a machine-checkable expression with explicit
policy-to-code traceability, so that automated enforcement and human audit reinforce
rather than displace one another. See [governance-as-code.md](./governance-as-code.md).

### Authority, identity, and assurance standards: the assured-bounded-autonomy turn (H1 2026)

The through-line of the first half of 2026 is that the **authority, identity, and
assurance layer began to catch up with raw agent capability.** Where prior years
produced runtimes that could *act*, H1 2026 produced the specifications that state *who
grants an agent authority, how that authority is bounded and revoked, and how
probabilistic AI judgment is separated from deterministic execution, settlement, and
accountability.* That is the distinction this repository frames as **assured bounded
autonomy** — software agents acting under delegated authority — rather than "autonomous
agents." Six primary-source developments anchor the turn, each labelled by maturity:

- **A decision/enforcement split, standardized (Final).** The OpenID Foundation approved
  the **AuthZEN Authorization API 1.0** as a *Final* specification on January 12, 2026
  (the document is dated January 11, 2026)
  ([OpenID](https://openid.net/specs/authorization-api-1_0.html)). It defines a
  transport-agnostic API between a **Policy Decision Point (PDP)**, which evaluates
  policy, and a **Policy Enforcement Point (PEP)**, which intercepts a request and
  enforces the verdict over simple JSON. The PDP/PEP split is not new — it is inherited
  from the ABAC/XACML tradition — but AuthZEN standardizes the contract *between* them,
  giving the Governance Gate a way to separate *deciding* from *enforcing*.
- **Federal work on agent identity and authorization (announcement).** On February 17,
  2026, NIST's Center for AI Standards and Innovation (CAISI), with its Information
  Technology Laboratory (ITL), announced the **AI Agent Standards Initiative**, organized
  around three pillars: industry-led agent standards plus international leadership;
  community-led open-source protocol development; and research in agent security and
  identity ([NIST](https://www.nist.gov/artificial-intelligence/ai-agent-standards-initiative)).
  Its ITL "AI Agent Identity and Authorization Concept Paper" maps onto mandate-scoped
  delegation and PDP/PEP authorization.
- **Post-deployment monitoring, made official (final report).** NIST published
  **AI 800-4, "Challenges to the Monitoring of Deployed AI Systems,"** as a *final*
  report on March 6, 2026
  ([NIST](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.800-4.pdf)). Drawing on three
  workshops (200+ experts) and an 87-paper review, it proposes **six post-deployment
  monitoring categories** — Functionality, Operational, Human Factors, Security,
  Compliance, and Large-Scale Impacts — establishing, from a US federal source, that
  one-time pre-deployment evaluation is not sufficient on its own and that continuous
  operational monitoring is required after deployment.
- **Probabilistic reasoning, deterministic settlement (published).** IMF
  **Note 2026/004, "How Agentic AI Will Reshape Payments"** (April 22, 2026), proposes a
  three-layer framework in which intent formation and orchestration may be
  **probabilistic**, but **authorization/control and settlement must remain
  deterministic** and rules-bound
  ([IMF](https://www.imf.org/en/publications/imf-notes/issues/2026/04/22/how-agentic-ai-will-reshape-payments-575560)).
  It is the institutional statement of the SDE's core architectural commitment:
  concentrate adaptive judgment upstream and keep the point of irreversible action
  auditable and predictable.
- **Standards bodies convene on embodied and identity trust (Focus Groups).** ITU-T
  established the **Focus Group on Embodied AI for Multimedia Technologies (FG-EAI)**
  under Study Group 21 on February 19, 2026, naming *closed-loop control* and
  *world-model training* among its foundational study areas
  ([ITU](https://www.itu.int/en/ITU-T/focusgroups/eai/Pages/default.aspx)); and it
  announced the **Focus Group on Trust and Identity for Humans and Agentic AI
  (FG-TIDA)** under Study Group 17 on July 9, 2026, whose scope covers agent identity,
  trust-lifecycle assurance models, and "continuous assessment of AI agents," and which
  explicitly aims to preserve **meaningful human control** for high-stakes actions such
  as executing financial transactions
  ([ITU](https://www.itu.int/en/mediacentre/Pages/PR-2026-07-09-focus-group-agentic-AI.aspx)),
  with its first meeting set for Paris in November 2026. Both are *pre-standardization*
  Focus Groups, not binding ITU-T Recommendations — they signal institutional
  convergence, not settled rules.

Read together, these are the governance and identity layer *catching up* to capability.
None is a shipped, end-to-end assurance system; they are the specifications and
monitoring frameworks on which one could be built, and they map cleanly onto an inherited
safety-engineering pattern — a trusted, verified monitor bounding an untrusted complex
function and reverting to a safe mode (Lui Sha's Simplex Architecture; ASTM F3269-21
run-time assurance; DARPA's Assured Autonomy program). Open-SDE develops how AuthZEN's
PDP/PEP split, NIST AI 800-4's monitoring categories, and the IMF's
probabilistic-vs-deterministic layering combine into a governance gate, runtime
assurance, and reconciliation of the execution receipt against the observed outcome in
[authority-and-safety-model.md](./authority-and-safety-model.md).

---

## 3. Payments and settlement: competing-but-converging under neutral bodies

Payments are the most visibly built-out execution layer, and the recurring
governance-gate primitive across every entrant is the same: **authority scoped to an
exact merchant, cart, and spend cap via signed mandates and tokens.**

**Card networks** anchored the credential and trust layer:

- Visa launched **Intelligent Commerce Connect** (April 2026), a network-, protocol-
  and token-vault-agnostic on-ramp giving agents one integration with spend controls
  across four agent protocols, with GA targeted around June 2026
  ([The Paypers](https://thepaypers.com/payments/news/visa-launches-intelligent-commerce-connect-for-agentic-payments)).
- Mastercard launched **Agent Pay for Machines** on June 10, 2026 — a multi-rail
  platform across cards, bank accounts, and stablecoins whose **Agentic Tokens** bind
  agent identity, consent, and step-up rules, with permissions recorded on Polygon,
  Solana, and Base
  ([Mastercard](https://www.mastercard.com/us/en/news-and-trends/press/2026/june/mastercard-launches-agent-pay-for-machines.html)).

**Open protocols** dominate the wire:

- The **Agentic Commerce Protocol (ACP)** — co-developed by OpenAI and Stripe
  (Apache 2.0, September 2025) — powers **ChatGPT Instant Checkout** and introduces the
  **[Shared Payment Token](./concepts.md#glossary)**, scoping an agent's authority to
  one merchant and exact cart total without exposing card credentials
  ([Stripe](https://stripe.com/newsroom/news/stripe-openai-instant-checkout)).
- Google's **Agent Payments Protocol (AP2)** was released as v0.2 with "Human Not
  Present" payments and donated to the **FIDO Alliance** to keep it neutral; its signed
  **[Mandates](./concepts.md#glossary)** (carried as W3C Verifiable Credentials) form a
  per-transaction audit trail
  ([Google](https://blog.google/products-and-platforms/platforms/google-pay/agent-payments-protocol-fido-alliance/);
  original announcement
  [Google Cloud](https://cloud.google.com/blog/products/ai-machine-learning/announcing-agents-to-payments-ap2-protocol)).
- Coinbase and Cloudflare's **[x402](./concepts.md#glossary)** — HTTP-402 stablecoin
  micropayments — was formalized under the **Linux Foundation** on April 2, 2026 with
  20+ founding members including Visa, Mastercard, Google, AWS, Circle, and Stripe
  ([Linux Foundation](https://www.linuxfoundation.org/press/linux-foundation-is-launching-the-x402-foundation-and-welcoming-the-contribution-of-the-x402-protocol)).
- Stripe and Tempo/Paradigm launched the **[Machine Payments Protocol (MPP)](./concepts.md#glossary)**
  on March 18, 2026, reviving HTTP 402 with a "sessions" spend-cap primitive across
  cards, BNPL, and stablecoins on a purpose-built L1
  ([Stripe](https://stripe.com/blog/machine-payments-protocol)).

**Stablecoin rails and wallets** target machine-to-machine payments that legacy card
economics cannot serve. Coinbase launched **Agentic Wallets** on February 11, 2026 —
MPC-secured, policy-controlled, with session caps and native x402
([Coinbase](https://www.coinbase.com/developer-platform/discover/launches/agentic-wallets)) —
and Circle launched its **Agent Stack** on May 11, 2026, including policy-controlled
Agent Wallets and gas-free USDC **[nanopayments](./concepts.md#glossary)** as small as
~$0.000001
([Circle](https://www.circle.com/pressroom/circle-launches-ai-infrastructure-to-power-the-agentic-economy)).
Skyfire's **KYAPay** pairs USDC settlement with a "Know Your Agent" identity framework
([Business Wire](https://www.businesswire.com/news/home/20251218520399/en/Skyfire-Demonstrates-Secure-Agentic-Commerce-Purchase-Using-the-KYAPay-Protocol-and-Visa-Intelligent-Commerce)).
Hyperscalers embedded these rails directly: AWS launched **Bedrock AgentCore Payments**
(preview) on May 7, 2026, built with Coinbase and Stripe, settling in USDC on Base with
per-session limits and audit trails
([AWS](https://aws.amazon.com/blogs/machine-learning/agents-that-transact-introducing-amazon-bedrock-agentcore-payments-built-with-coinbase-and-stripe/)).

**Activity, measured.** A Keyrock analysis reported by CoinDesk on May 21, 2026 provides
the first hard measurement: agents settled roughly **176 million on-chain transactions
worth over $73 million** between May 2025 and April 2026, **98.6% in USDC**, with ~76% of
payments below the ~30-cent card fee floor
([CoinDesk](https://www.coindesk.com/business/2026/05/21/crypto-rails-are-becoming-the-default-payment-layer-for-ai-agents-report-says)).
Real, but small — and structurally favoring programmable stablecoin settlement for
sub-cent flows. See [agent-native-economy.md](./agent-native-economy.md) and
[references.md](./references.md).

---

## 4. Reality-Anchored Execution: paid deployment, with the widest proven-vs-promotional gap

The physical Decision→Execution leg moved from demonstration toward early *paid*
deployment — but this is also where sourcing is weakest and caveats matter most.

**Digital twins** are a maturing validation substrate. On March 16, 2026, NVIDIA made
its **Omniverse DSX Blueprint** generally available alongside the Vera Rubin DSX
reference design, building physically accurate twins of AI data-center "factories" to
simulate power, cooling, and operations before construction, with broad industrial
partner support
([NVIDIA](https://nvidianews.nvidia.com/news/nvidia-releases-vera-rubin-dsx-ai-factory-reference-design-and-omniverse-dsx-digital-twin-blueprint-with-broad-industry-support)).
At GTC 2026, NVIDIA presented an expanded physical-AI stack — **Cosmos** world models,
**Isaac GR00T**, and the **Mega** Omniverse Blueprint for rehearsing multi-robot policies
in twins before deployment
([NVIDIA blog](https://blogs.nvidia.com/blog/gtc-2026-virtual-worlds-physical-ai/)).
These world models are the rehearsal environment where agent policies are validated
against physics before acting.

**Embodied agents** supply the physical Agent-Decision layer through
**[Vision-Language-Action (VLA)](./concepts.md#glossary)** foundation models. Google
DeepMind's **Gemini Robotics 1.5** (announced September 25, 2025) pairs an
embodied-reasoning "brain" with a VLA action model and demonstrates cross-embodiment
transfer — one policy across heterogeneous robot bodies
([Google DeepMind](https://deepmind.google/blog/gemini-robotics-15-brings-ai-agents-into-the-physical-world/)),
with Physical Intelligence's pi-0.5 and NVIDIA Isaac GR00T on parallel tracks.

**Physical execution reached paid deployment.** Figure moved from research demo to a
paid commercial deployment at BMW's Spartanburg plant: Figure 02 units accumulated
roughly 1,250 operating hours contributing to production of 30,000+ vehicles, followed
by a commercial contract for Figure 03 units
([Figure AI](https://www.figure.ai/news/production-at-bmw)). Reported per-hour pricing
and broader fleet counts vary by source and are treated here as uncertain. Amazon's
**DeepFleet** generative-AI orchestrator coordinates a fleet reported at **1M+ robots**
across 300+ facilities, with a reported ~10% travel-time improvement
([Amazon](https://www.aboutamazon.com/news/operations/amazon-million-robots-ai-foundation-model)).

**DePIN** networks pivoted from token-mining toward paid services with real external
revenue — Render (~$38M monthly), Hivemapper (~36× growth), and Aethir (~$166M ARR) —
though sector-wide market-cap and device figures ($40B–$900B+) are inconsistent across
promotional secondary sources and should be treated with caution
([BlockEden](https://blockeden.xyz/blog/2026/03/12/depin-compute-revenue-pivot-akash-ionet-aethir/)).

> **The central caveat.** Several headline "machine economy" milestones remain
> **simulation-only**. peaq's May 12, 2026 demonstration of a Serve Robotics bot
> settling a delivery in USDT on-chain ran in a simulated urban environment, not on live
> streets ([Cryptonews](https://cryptonews.net/news/altcoins/32906537/)). An
> "autonomous AI grid operator" claim circulating in promotional content could not be
> verified and is **excluded**. The sim-to-real gap and the absence of *certified*
> governance gates for autonomous physical action are the central unresolved problems.
> See [reality-anchored-execution.md](./reality-anchored-execution.md).

---

## 5. On-chain machine economy: the loop runs end-to-end among machines

The on-chain layer shows the SDE loop operating end-to-end among machines, alongside a
rapidly institutionalizing asset layer.

- **Agent identity** is being *explored* via **[ERC-8004 "Trustless Agents"](./concepts.md#glossary)** —
  an Ethereum proposal that **remains a Draft** (Standards Track, Category ERC; created
  August 13, 2025; still Draft as of mid-2026) even though its reference Identity /
  Reputation / Validation registries were deployed to Ethereum mainnet in early 2026
  (around January 29, 2026), with thousands of agents registered within weeks
  ([Ethereum EIPs](https://eips.ethereum.org/EIPS/eip-8004)). It should be read as an
  emerging, unfinished substrate — not a settled standard — and the SDE does not build
  mandate-enforcement guarantees on its wire format or registry semantics as if stable.
- **Tokenized real-world assets** excluding stablecoins crossed **~$32B** in May 2026
  (up >200% YoY), led by tokenized Treasuries
  ([Yellow research](https://yellow.com/research/tokenized-rwas-31b-market-growth-real-race-starting)),
  with BlackRock's **BUIDL** at ~$2.4–2.5B AUM and two further tokenized structures filed
  ([CoinDesk](https://www.coindesk.com/business/2026/05/09/blackrock-deepens-tokenization-push-with-new-onchain-fund-offerings)) —
  giving agents a yield-bearing, compliance-gated collateral asset.
- On **March 17, 2026**, the SEC and CFTC issued a first-of-its-kind joint five-category
  crypto classification framework — a prerequisite governance gate determining which
  assets agents can legally hold
  ([Norton Rose Fulbright](https://www.nortonrosefulbright.com/en/knowledge/publications/a88b661b/sec-and-cftc-release-joint-interpretation-on-crypto-asset-regulation)).
- Emerging agent-to-agent labor markets (DeFi agent vaults hiring other agents) are
  being formalized in the literature, e.g. a July 2026 arXiv paper on agent-to-agent
  finance ([arXiv](https://arxiv.org/abs/2607.00245)) — reported here as early-stage.

---

## 6. The reality check: infrastructure over-build vs. thin organic demand

The disciplined finding of this survey is a single sentence: **the rails exist; clearing
demand is nascent.** The sharpest illustration is x402. Despite an ecosystem valuation
near $7B, CoinDesk reported on March 11, 2026 that on-chain data showed x402 processing
only about **$28,000 in daily volume**, with on-chain analysis suggesting roughly **half
of transactions were artificial**
([CoinDesk](https://www.coindesk.com/markets/2026/03/11/coinbase-backed-ai-payments-protocol-wants-to-fix-micropayment-but-demand-is-just-not-there-yet)).
Read alongside the Keyrock measurement (§3) — real, but small — the picture is an
**infrastructure-build inflection, not a proven-at-scale economy.**

The strategic questions therefore concern *consolidation and accountability*, not
feasibility:

1. **Convergence vs. fragmentation.** Do the payment, identity, and governance stacks
   (AP2/UCP vs. ACP/MPP vs. x402 vs. card-network credentials) converge into one
   interoperable trust fabric, or fragment along hyperscaler lines? Membership overlaps
   heavily, but interoperability is unresolved.
2. **Liability.** Who is responsible when a hijacked or misaligned agent executes a
   harmful action *despite a passing gate or a signed mandate*?
3. **Reversibility boundary.** Which economic actions (money movement, access change,
   irreversible commitments) must remain human-gated, and how do risk tiers map to gate
   strictness?
4. **Systemic risk.** Concentration in a single settlement stablecoin (98.6% USDC), a
   handful of proprietary runtimes, sub-ms policy engines as single points of failure,
   and kill-switch cascades in multi-agent systems.

These are developed as a research agenda in [ROADMAP.md](../ROADMAP.md).

---

## Related

- [concepts.md](./concepts.md) — SDE definitions, primitives, and the full glossary.
- [agent-native-economy.md](./agent-native-economy.md) — research area 1: the
  Agent-Decision node in depth.
- [reality-anchored-execution.md](./reality-anchored-execution.md) — research area 2:
  twins, embodied agents, and DePIN.
- [governance-as-code.md](./governance-as-code.md) — research area 3: the executable
  governance gate.
- [authority-and-safety-model.md](./authority-and-safety-model.md) — the assured-bounded-autonomy
  model: PDP/PEP, runtime assurance, reconciliation, and the eight non-claims.
- [reference-architecture.md](./reference-architecture.md) — the five-stage loop mapped
  to real 2026 technologies, with worked end-to-end instantiations.
- [ROADMAP.md](../ROADMAP.md) — open questions and prioritized research agenda.

See [references.md](./references.md) for the full, annotated source list.
