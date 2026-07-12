# Working Definition and Scope

*What the Software-Defined Economy is, what it deliberately is not, and how it relates to the adjacent vocabulary of 2026.*

*Last updated: July 2026 · Part of the [Open-SDE](../README.md) research initiative.*

> **Maturity: Reviewed.** This document has been reviewed against primary sources but
> is not a standard or a specification. Protocol and specification statuses below use
> the labels **Final / Stable / Draft / Experiment**; repository documents use
> **Idea / Draft / Reviewed / Prototype / Evaluated**.

---

## Working definition

> **A Software-Defined Economy is a socio-technical system in which software agents,
> operating under delegated authority, allocate scarce resources or initiate
> state-changing actions through explicit policy, authorization, execution,
> settlement, and accountability controls.**

Three commitments are load-bearing in that sentence and distinguish Open-SDE from a
general "AI can transact" thesis:

1. **Delegated authority, not first-class agency.** An agent is *software acting under
   delegated authority*. Some principal — a person or an organization — grants that
   authority, bounds it, and can revoke it. The interesting questions are *who grants
   it, how it is bounded, and how it is revoked*, not whether software can act.

2. **Probabilistic judgment separated from deterministic control.** Reasoning, intent
   formation, and orchestration may be probabilistic; **authorization, execution, and
   settlement must be deterministic and auditable.** This mirrors the three-layer
   framework in the IMF's April 2026 note
   [*How Agentic AI Will Reshape Payments*](https://www.imf.org/en/publications/imf-notes/issues/2026/04/22/how-agentic-ai-will-reshape-payments-575560)
   (Note No. 2026/004), which argues that adaptive reasoning should sit upstream while
   the authorization/control and settlement layers stay strictly rules-bound.

3. **Assured bounded autonomy.** Autonomy is permitted only inside verified bounds. A
   trusted, verified monitor bounds an untrusted or complex function and reverts to a
   safe mode when a violation is imminent — the run-time assurance pattern codified for
   aviation in [ASTM F3269-21](https://store.astm.org/f3269-21.html) (2021) and rooted
   in Lui Sha's Simplex Architecture and DARPA's Assured Autonomy program. Open-SDE
   inherits this practice; it does not invent it.

Humans are not removed from the loop. Their role moves from per-action approval to
setting authority, budgets, and monitoring; handling exceptions; and revoking
authority — "AI recommends, humans decide."

The minimum set of controls that makes a system conformant to this definition is
specified separately as the **SDE-0 minimum conformance profile**
([sde-0-conformance-profile.md](sde-0-conformance-profile.md)): an identifiable
principal/operator/agent; a mandate with scope, budget, duration, target, and
revocation conditions; state inputs carrying provenance, timestamp, freshness, and
uncertainty; a Policy Decision Point / Policy Enforcement Point independent of the AI;
execution with idempotency, rate limits, budget caps, and a safe fallback;
reconciliation of the execution receipt against the observed outcome; and continuous
monitoring, incident logging, recovery, and human override.

---

## A working term, not a settled label

**"Software-Defined Economy" is Open-SDE's working term.** It is not an established
standard, an industry category, or an academic term of art. We use it because it names
the property we care about — economic behavior *defined by executable policy and
control* — but we do not claim it is widely adopted.

We are deliberate about one negative claim: the phrase appears incidentally in the
software-metrics literature (e.g.
[arXiv 2411.05087](https://arxiv.org/abs/2411.05087), Brown et al., which uses "an
increasingly software-defined economy" once, in passing), but that paper is about
measuring software innovation from open-source data and provides **no conceptual
foundation** for anything in this repository. It is mentioned here only to pre-empt a
mistaken citation, not as support.

---

## Scope: what the SDE reference model is *not*

Open-SDE is a **general, cross-domain reference model** for authority, safety, and
evaluation. It is intentionally narrow in ambition and broad in applicability. The
non-goals below are firm.

- **Not another agent framework.** We do not ship an agent runtime, harness, or
  orchestration SDK. Those already exist in production (Microsoft Agent Framework,
  OpenAI's Agents SDK, the Claude Agent SDK, Google ADK). The reference model describes
  the controls such runtimes should sit behind, not a competing runtime.

- **Not a payment rail or settlement implementation.** We do not define a wire protocol
  for money movement. Payment and settlement rails already exist and are surveyed in
  [landscape-2026.md](landscape-2026.md). The reference model describes how authority
  is scoped and reconciled *across* whatever rail is used.

- **Token-, chain-, and product-neutral.** The model names no required token, no
  required blockchain, and no required vendor. Blockchain settlement is one possible
  execution substrate among several (physical, on-chain, or API), not a premise.

- **A reference model, not a running system.** Open-SDE is a research frame. It is not a
  standard, a certification scheme, or an operationally-ready product. Specific
  deployments appear only as **case studies** (see the Mossland crosswalk below), never
  as the model itself.

- **Not a replacement for law or organizational responsibility.** Encoding policy as
  code gives **policy-to-code traceability**; it complements — and never replaces —
  legal obligation, functional-safety procedure, and organizational accountability.

Where a system reasons over a model of the world, we call that model the
**Authoritative State Model** — the general term. A **Digital Twin** is its
*physical-domain specialization*; it is not forced onto digital-commerce or API
domains, where the authoritative state is a catalog, an account balance, or an order
book rather than a physical replica.

---

## Relationship to adjacent terms

The same underlying shift is described under several better-known labels. Open-SDE maps
onto that vocabulary rather than competing with it; its distinctive contribution is the
authority-and-safety spine, not a new name for the phenomenon.

| Adjacent term | Used by | Emphasis | How Open-SDE relates |
| --- | --- | --- | --- |
| **Agentic economy** | [a16z](https://a16z.com/newsletter/big-ideas-2026-part-1/), [Stripe](https://stripe.com/newsroom/news/stripe-openai-instant-checkout), [Circle](https://www.circle.com/pressroom/circle-launches-ai-infrastructure-to-power-the-agentic-economy), [WEF](https://www.weforum.org/publications/ai-agents-in-action-a-playbook-for-trusted-adoption-authorization-and-scaling/) | Agents transacting and paying at scale; the payment and authorization stack around them | Open-SDE supplies the *bounding* layer — mandates, PDP/PEP, reconciliation — that the agentic-economy stack assumes but does not itself specify |
| **Programmable economy** and **machine customers** | [Gartner](https://www.gartner.com/en/newsroom/press-releases/2025-09-10-gartner-unveils-top-emerging-technologies-to-support-autonomous-business) | Non-human buyers (agents, devices, vehicles) participating in commerce | Open-SDE treats "machine customers" as principals-under-delegation and asks how their authority is scoped, budgeted, and revoked |
| **Machine / agent economy** | [McKinsey](https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-automation-curve-in-agentic-commerce), WEF | Market sizing and the automation curve for agent-mediated commerce | Open-SDE is agnostic to the sizing debate; it focuses on the control model that must hold whether the market is large or small |

The through-line: mainstream discourse describes *that* agents will transact. Open-SDE
specifies *under what controls* they may.

---

## Where the SDE sits in the protocol stack

The reference model is protocol-neutral, but it is instantiated on top of real,
maturing protocols. The following are the ones most relevant to the loop's
authorization, coordination, and execution stages. **Maturity labels reflect the status
of the specification, not its adoption or its organic demand.**

| Protocol | Layer / role | Maturity | Basis for the label |
| --- | --- | --- | --- |
| **[MCP](https://blog.modelcontextprotocol.io/posts/2026-07-28-release-candidate/)** (Model Context Protocol) | Agent-to-tool/data context | **Stable** | Multiple finalized spec revisions; the 2026-07-28 revision (release candidate) hardens OAuth 2.1/OIDC authorization and standardizes human-in-the-loop |
| **[A2A](https://www.linuxfoundation.org/press/a2a-protocol-surpasses-150-organizations-lands-in-major-cloud-platforms-and-sees-enterprise-production-use-in-first-year)** (Agent2Agent) | Agent-to-agent discovery and delegation | **Stable** | Reached first stable v1.0 under the Linux Foundation (April 2026), with signed Agent Cards and 150+ organizations |
| **[UCP / AP2](https://blog.google/products-and-platforms/platforms/google-pay/agent-payments-protocol-fido-alliance/)** | Commerce catalog + payment authorization (mandates) | **Draft** | AP2 released as v0.2 and donated to the FIDO Alliance (April 2026); UCP is an early open-source common language |
| **[ACP](https://stripe.com/newsroom/news/stripe-openai-instant-checkout)** (Agentic Commerce Protocol) | Agent-initiated checkout; Shared Payment Tokens | **Stable** | Apache-2.0 open standard (OpenAI + Stripe, Sept 2025) in consumer-scale production behind ChatGPT Instant Checkout |
| **[x402](https://www.linuxfoundation.org/press/linux-foundation-is-launching-the-x402-foundation-and-welcoming-the-contribution-of-the-x402-protocol)** | HTTP-402 stablecoin micropayment settlement | **Stable** | Formalized as a Linux Foundation project (April 2, 2026); note that organic demand is still thin (see [landscape-2026.md](landscape-2026.md)) |
| **[MPP](https://stripe.com/blog/machine-payments-protocol)** (Machine Payments Protocol) | Machine-to-machine payment with spend-capped sessions | **Draft** | Launched by Stripe/Tempo (March 2026); the "sessions" spend-cap primitive is new and evolving |
| **[AuthZEN Authorization API 1.0](https://openid.net/specs/authorization-api-1_0.html)** | PDP/PEP authorization decision boundary | **Final** | An OpenID **Final Specification** (document dated 2026-01-11; approved by OIDF membership 2026-01-12) standardizing the API between a Policy Decision Point and a Policy Enforcement Point |
| **[ERC-8004](https://eips.ethereum.org/EIPS/eip-8004)** (Trustless Agents) | On-chain agent identity, reputation, validation | **Draft** | Standards-Track ERC, created 2025-08-13, still Draft as of mid-2026 — even though reference registries deployed to mainnet |

Two of these deserve emphasis because they map directly onto the reference model's
governance gate:

- **AuthZEN (Final)** underwrites the split of the gate into a **Policy Decision Point**
  (decides) and a **Policy Enforcement Point** (enforces). The PDP/PEP distinction
  originates in the ABAC/XACML tradition; AuthZEN standardizes the API between the two.
- **ERC-8004 (Draft)** is a candidate agent-identity substrate. Because it is still a
  Draft proposal, the reference model treats it as emerging, not settled, and does not
  build enforcement guarantees on its registry semantics.

---

## A worked instantiation: the Mossland crosswalk

Because Open-SDE is a reference model and not a system, the way to see it in operation
is through a case study. The
[Mossland crosswalk](case-studies/mossland-crosswalk.md) maps a concrete Mossland Lab
deployment onto the SDE-0 conformance requirements — showing which controls are present,
which are partial, and which are absent — without promoting the model to a standard or
the case study to a certification. It is one instantiation, kept deliberately separate
from the general model so that domain-specific implementation choices never leak back
into the cross-domain frame.

---

## Related

- [reference-architecture.md](reference-architecture.md) — the five-stage loop expanded
  into a concrete architecture.
- [authority-and-safety-model.md](authority-and-safety-model.md) — delegated authority,
  PDP/PEP, run-time assurance, and the explicit non-claims.
- [sde-0-conformance-profile.md](sde-0-conformance-profile.md) — the seven minimum
  conformance requirements referenced above.
- [concepts.md](concepts.md) — vocabulary and primitives.
- [landscape-2026.md](landscape-2026.md) — what is actually shipping across the loop.
- [case-studies/mossland-crosswalk.md](case-studies/mossland-crosswalk.md) — a worked
  instantiation.

See [references.md](references.md) for the full, annotated source list.
