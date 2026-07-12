# SDE-0: A Minimum Conformance Profile

*A working checklist for the smallest configuration in which a software agent may take a state-changing action under delegated authority — and be held to account for it.*

*Last updated: July 2026 · Part of the [Open-SDE](../README.md) research initiative.*

**Maturity: Draft.** This is a research artifact, not a certification scheme, an audit standard, or an operationally-ready compliance regime. Nothing here confers approval, attestation, or safety assurance on any system.

---

## What SDE-0 is

SDE-0 is the *floor*. It names the minimum set of controls that must be present before a software agent, [operating under delegated authority](./working-definition-and-scope.md), is allowed to allocate a scarce resource or initiate a state-changing action — a payment, an access change, a physical actuation, an on-chain transaction. It is deliberately modest: the "0" signals a baseline, not a target. A system that fails SDE-0 is not merely immature; it is missing the controls that make bounded autonomy *assured* rather than merely *claimed*.

The governing commitment of Open-SDE is **assured bounded autonomy**: reasoning, intent formation, and orchestration may be probabilistic, but *authorization, control, and settlement must be deterministic and auditable*. This mirrors the three-layer framework in the IMF's April 2026 note [*How Agentic AI Will Reshape Payments*](https://www.imf.org/en/publications/imf-notes/issues/2026/04/22/how-agentic-ai-will-reshape-payments-575560) (IMF Note No. 2026/004), which argues that adaptive AI belongs upstream in intent/orchestration while the authorization/control and settlement layers stay strictly deterministic and rules-bound. SDE-0 is a concrete way to check whether a given deployment actually maintains that separation.

## What SDE-0 is *not*

- **Not a certification.** Passing this checklist is a self-assessment aid, nothing more. There is no SDE-0 mark, badge, or accreditation, and this document does not grant one.
- **Not a safety case.** A conformant system can still cause harm. SDE-0 checks for the *presence* of controls, not their *sufficiency* for any particular domain, risk tier, or jurisdiction.
- **Not domain-complete.** High-consequence domains (finance, healthcare, critical infrastructure, embodied action) demand controls far beyond this floor. Risk-tiered frameworks such as Singapore's January 2026 [Model AI Governance Framework for Agentic AI](https://www.imda.gov.sg/resources/press-releases-factsheets-and-speeches/press-releases/2026/new-model-ai-governance-framework-for-agentic-ai) — which grades controls by reversibility, autonomy level, and data sensitivity — sit *above* SDE-0, not inside it.
- **Not a replacement for law or organizational responsibility.** These controls are policy-to-code traceability, not governance-as-a-substitute-for-governance. Code complements legal and organizational accountability; it never replaces it.

The reference architecture these requirements assume is set out in [reference-architecture.md](./reference-architecture.md); the failure modes they defend against, and the eight non-claims that bound what any of them can promise, are catalogued in [authority-and-safety-model.md](./authority-and-safety-model.md).

---

## The seven requirements

Each requirement below states the control, the failure mode it exists to prevent, the primary source or established practice it inherits from, and the JSON schema(s) in [`../schemas/`](../schemas/) that carry the corresponding evidence. The schemas are the machine-checkable expression of the profile; the prose here is the rationale.

### 1. Identifiable principal, owner/operator, and agent

**Requirement.** Every action traces to three distinct, durable, and verifiable identities: the **principal** (the human or organization on whose behalf authority is exercised), the **owner/operator** (the party that runs the agent and is accountable for its operation), and the **agent** itself (the software actor taking the action). None may be implicit or anonymous.

**Why it exists.** The failure mode is the *unattributable action*: something happened, money moved or a door unlocked, and no one can say whose authority it was under or who is answerable. Identity confusion is also an attack surface — OWASP's December 2025 [Top 10 for Agentic Applications](https://genai.owasp.org/2025/12/09/owasp-top-10-for-agentic-applications-the-benchmark-for-agentic-security-in-the-age-of-autonomous-ai/) lists *Identity & Privilege Abuse* (ASI03) among the top agentic risks. Durable, revocable machine identity is now the explicit focus of standardization: NIST's [AI Agent Standards Initiative](https://www.nist.gov/artificial-intelligence/ai-agent-standards-initiative) (announced February 2026, with an ITL "AI Agent Identity and Authorization Concept Paper") and the ITU-T [Focus Group on Trust and Identity for Humans and Agentic AI](https://www.itu.int/en/mediacentre/Pages/PR-2026-07-09-focus-group-agentic-AI.aspx) (FG-TIDA, announced July 2026) both treat agent identity as the prerequisite for authorization and accountability. On-chain identity substrates exist too — ERC-8004 ["Trustless Agents"](https://eips.ethereum.org/EIPS/eip-8004) defines an Identity Registry — but it remains a **Draft** proposal (Standards Track, created August 2025, still Draft as of mid-2026), so it should be treated as an emerging option, not a settled dependency. Note the non-claim: agent *authentication* is not *authorization* of any specific action (that is requirement 4).

**Backed by.** [`action-mandate.schema.json`](../schemas/action-mandate.schema.json) (principal, owner/operator, and agent identity fields) and [`policy-decision.schema.json`](../schemas/policy-decision.schema.json) (the subject a decision is rendered about).

### 2. A mandate stating scope, budget, duration, target, and revocation conditions

**Requirement.** Authority is expressed as an explicit, signed **mandate** that states, at minimum: the *scope* of permitted actions, a *budget* (spend or resource cap), a *duration* (validity window), a *target* (the systems or counterparties it applies to), and the *conditions under which it is revoked*. Authority that is not written down in these terms does not exist for SDE-0 purposes.

**Why it exists.** The failure mode is *unbounded authority* — an agent that, once trusted, can spend without limit, act indefinitely, or reach systems no one intended. Bounding delegated authority is the deterministic "authorization/control" layer the [IMF note](https://www.imf.org/en/publications/imf-notes/issues/2026/04/22/how-agentic-ai-will-reshape-payments-575560) keeps separate from probabilistic reasoning. In deployed practice this is the same primitive as AP2's cryptographically signed *Mandates* and the scoped tokens used across agentic-commerce protocols, and it aligns with the WEF/Capgemini [Agent Capability and Authorization Profile](https://www.weforum.org/publications/ai-agents-in-action-a-playbook-for-trusted-adoption-authorization-and-scaling/) (ACAP, 2026), which documents an agent's delegated power — permitted actions, contexts, conditions, oversight — as a single auditable record. A mandate is where "AI recommends, humans decide" is encoded: the human's role is to set the scope, budget, and revocation terms, not to approve each action.

**Backed by.** [`action-mandate.schema.json`](../schemas/action-mandate.schema.json) is the canonical carrier for this requirement — scope, budget, duration, target, and revocation are its core fields.

### 3. State inputs carrying provenance, timestamp, freshness, and uncertainty

**Requirement.** Every input the agent reasons over — a sensor reading, a market quote, a catalog entry, a chain state — arrives with **provenance** (where it came from), a **timestamp**, a **freshness** indication (how stale it may be), and an explicit **uncertainty** (confidence, error bounds, or a flag that it is unknown). Bare values without this metadata are not admissible state.

**Why it exists.** The failure mode is *acting on state that is stale, spoofed, or falsely precise*. This is where two of the eight non-claims bite: a Digital Twin (or any Authoritative State Model) is not ground truth, and blockchain immutability guarantees the integrity of a record, not the *truth* of the external input written into it. NIST's March 2026 final report [*Challenges to the Monitoring of Deployed AI Systems*](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.800-4.pdf) (NIST AI 800-4) frames operational monitoring — tracking the data an AI system consumes and produces after deployment — as one of its six categories precisely because dynamic inputs and non-determinism cause behavior that pre-deployment testing cannot anticipate. Provenance and freshness are what make an input *reconcilable* later (requirement 6). The general term for the state the agent reasons over is the **Authoritative State Model**; the **Digital Twin** is its physical-domain specialization — SDE-0 applies to both, and to purely digital/API domains where no twin exists.

**Backed by.** [`reality-signal.schema.json`](../schemas/reality-signal.schema.json) (per-input provenance, timestamp, freshness, uncertainty) and [`twin-state.schema.json`](../schemas/twin-state.schema.json) (the assembled state model the agent reads, in physical domains).

### 4. A PDP/PEP that is independent of the AI

**Requirement.** The decision to permit an action and the enforcement of that decision both happen **outside the model**. A **Policy Decision Point (PDP)** evaluates the request against the mandate and policy and returns a verdict; a **Policy Enforcement Point (PEP)** intercepts the action and admits it only if the PDP permitted it. The agent that *wants* to act is never the component that *authorizes* it.

**Why it exists.** The failure mode is *the agent judging its own authority* — which collapses entirely under adversarial pressure. OWASP's [Top 10 for Agentic Applications](https://genai.owasp.org/2025/12/09/owasp-top-10-for-agentic-applications-the-benchmark-for-agentic-security-in-the-age-of-autonomous-ai/) puts *Agent Goal Hijack* (ASI01) at the top: an attacker who redirects the agent's objective must not thereby gain its authority. Separating decision from enforcement, and both from the reasoning model, is the deterministic authorization layer of the [IMF](https://www.imf.org/en/publications/imf-notes/issues/2026/04/22/how-agentic-ai-will-reshape-payments-575560) three-layer framework. The PDP/PEP split itself is a settled pattern from the ABAC/XACML tradition; what is newly standardized is the *interface* between them: the OpenID [AuthZEN Authorization API 1.0](https://openid.net/specs/authorization-api-1_0.html), approved as an **OpenID Final Specification** in January 2026, defines a transport-agnostic API by which a PEP asks a PDP for an access decision without either knowing the other's internals. SDE-0 does not require AuthZEN specifically, but it does require the architecture AuthZEN standardizes.

**Backed by.** [`policy-decision.schema.json`](../schemas/policy-decision.schema.json) records the PDP's verdict (permit/deny/escalate), the mandate and policy it was evaluated against, and the obligations attached to a permit — the artifact the PEP enforces and auditors replay.

### 5. Execution with idempotency, rate limits, budget caps, and a safe fallback

**Requirement.** The execution path enforces **idempotency** (a retried action does not double-execute), **rate limits**, **budget caps** (honoring the mandate's budget at the point of action), and a **safe fallback** — a verified, deterministic behavior the system reverts to when a violation is imminent or the agent's output cannot be trusted.

**Why it exists.** The failure mode is *runaway or duplicated execution* — the same payment sent twice on retry, a loop that drains a budget, an agent whose degraded output is executed anyway. The safe-fallback element is not novel; it is inherited safety engineering. The **Runtime Assurance (RTA)** pattern — a trusted, verified safety monitor that bounds an untrusted or complex function and reverts to a verified-safe mode when a safety violation is imminent — originates in Lui Sha's Simplex Architecture and is codified in [ASTM F3269-21](https://store.astm.org/f3269-21.html), *"Standard Practice for Methods to Safely Bound Behavior of Aircraft Systems Containing Complex Functions Using Run-Time Assurance,"* developed expressly to permit AI/ML and other non-pedigreed complex functions in aircraft while preserving safety, and further advanced by DARPA's Assured Autonomy program. SDE-0 borrows the pattern directly: the deterministic execution layer bounds the probabilistic agent and falls back to a known-safe mode rather than executing on doubt.

**Backed by.** [`action-mandate.schema.json`](../schemas/action-mandate.schema.json) (the budget and rate constraints the executor enforces) and [`execution-receipt.schema.json`](../schemas/execution-receipt.schema.json) (the idempotency key and the record of whether the action executed or fell back).

### 6. Reconciliation of the execution receipt against the observed outcome

**Requirement.** After acting, the system compares the **execution receipt** (what the execution layer reports it did) against the **observed real-world outcome** (a fresh state signal about what actually happened) and reconciles the two. A receipt is not accepted as truth on its own.

**Why it exists.** The failure mode is the conflation captured in two of the eight non-claims: *transaction success is not real-action success*, and *settlement finality is not proof of a physical or economic outcome*. A payment can settle while the goods never ship; a robot can report task-complete while the shelf is still empty; an on-chain transfer can confirm against a false external input. The [IMF](https://www.imf.org/en/publications/imf-notes/issues/2026/04/22/how-agentic-ai-will-reshape-payments-575560) framework isolates settlement as its own deterministic layer precisely because finality there does not certify anything upstream or downstream of it. Reconciliation closes the loop: it feeds the observed outcome back as a new reality signal (requirement 3) and turns a one-shot action into something auditable and correctable.

**Backed by.** [`execution-receipt.schema.json`](../schemas/execution-receipt.schema.json) (what was claimed) reconciled against a fresh [`reality-signal.schema.json`](../schemas/reality-signal.schema.json) (what was observed); the delta is the reconciliation record.

### 7. Continuous monitoring, incident logging, recovery, and human override

**Requirement.** The system is **monitored continuously** across the dimensions that matter after deployment, **logs incidents**, has a defined **recovery** path, and preserves a **human override** — the ability to pause, revoke, or stop the agent at any time.

**Why it exists.** The failure mode is *undetected drift*: a system that passed every pre-deployment check and then degrades, is attacked, or produces harmful large-scale effects that no one is watching for. NIST AI 800-4, [*Challenges to the Monitoring of Deployed AI Systems*](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.800-4.pdf) (final report, March 2026), is the anchor: it proposes **six** post-deployment monitoring categories — *Functionality, Operational, Human Factors, Security, Compliance,* and *Large-Scale Impacts* — and argues that one-time pre-deployment evaluation is not enough because model non-determinism and dynamic inputs produce behavior that only shows up in operation. Human override is the other half: SDE-0 does not put humans "out of the loop"; it moves their role from per-action approval to setting authority, monitoring, and holding the revocation and stop controls — the "meaningful human control" for high-stakes actions that ITU-T [FG-TIDA](https://www.itu.int/en/mediacentre/Pages/PR-2026-07-09-focus-group-agentic-AI.aspx) names as a design goal.

**Backed by.** [`execution-receipt.schema.json`](../schemas/execution-receipt.schema.json) and [`policy-decision.schema.json`](../schemas/policy-decision.schema.json) supply the audit trail monitoring consumes; the incident, recovery, and override procedures live in [`../assurance/incident-and-recovery.md`](../assurance/incident-and-recovery.md), the [failure taxonomy](../assurance/failure-taxonomy.md), and the [hazard log](../assurance/hazard-log.yaml).

---

## Conformance checklist

A deployment meets SDE-0 only when **all seven** rows hold. The maturity labels (Idea / Draft / Reviewed / Prototype / Evaluated) are for a self-assessor to record honestly against each row; the profile itself is at **Draft**.

| # | Requirement | Prevents | Anchored in | Backing schema(s) |
| --- | --- | --- | --- | --- |
| 1 | Identifiable principal, owner/operator, and agent | Unattributable action | NIST Agent Standards Initiative; ITU FG-TIDA; ERC-8004 (Draft) | `action-mandate`, `policy-decision` |
| 2 | Mandate: scope, budget, duration, target, revocation | Unbounded authority | IMF 2026/004 authorization layer; WEF ACAP | `action-mandate` |
| 3 | State inputs with provenance, timestamp, freshness, uncertainty | Acting on stale/spoofed/false-precision state | NIST AI 800-4 (operational monitoring) | `reality-signal`, `twin-state` |
| 4 | PDP/PEP independent of the AI | Agent authorizing itself; goal hijack | OpenID AuthZEN 1.0 (Final); IMF 2026/004; OWASP ASI01 | `policy-decision` |
| 5 | Idempotency, rate limits, budget caps, safe fallback | Runaway or duplicated execution | ASTM F3269-21 / Simplex RTA; DARPA Assured Autonomy | `action-mandate`, `execution-receipt` |
| 6 | Reconcile receipt against observed outcome | Transaction success mistaken for real success | IMF 2026/004 settlement layer; reconciliation principle | `execution-receipt` + `reality-signal` |
| 7 | Continuous monitoring, incident logging, recovery, human override | Undetected post-deployment drift | NIST AI 800-4 (six monitoring categories) | `execution-receipt`, `policy-decision` |

**How to read a "pass."** All seven present and wired together means the deployment has the *shape* of assured bounded autonomy. It does not mean the deployment is safe, compliant, or fit for any particular use — see the non-claims in [authority-and-safety-model.md](./authority-and-safety-model.md). SDE-0 is a floor to build up from, not a ceiling to rest on.

---

## Related

- [working-definition-and-scope.md](./working-definition-and-scope.md) — the definition of a Software-Defined Economy that this profile operationalizes.
- [reference-architecture.md](./reference-architecture.md) — the five-node loop these requirements are checkpoints on.
- [authority-and-safety-model.md](./authority-and-safety-model.md) — the failure modes and the eight non-claims that bound what any control here can promise.
- [`../schemas/`](../schemas/) — the machine-checkable schemas that carry SDE-0 evidence: `action-mandate`, `reality-signal`, `twin-state`, `policy-decision`, `execution-receipt`.
- [`../assurance/`](../assurance/) — the failure taxonomy, hazard log, and incident-and-recovery procedures behind requirement 7.

See [references.md](./references.md) for the full, annotated source list.
