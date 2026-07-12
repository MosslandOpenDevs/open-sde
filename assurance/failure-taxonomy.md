# SDE Failure Taxonomy

*A structured catalogue of the ways an assured-bounded-autonomy loop breaks — organized by loop stage, and mapped to the control that should have caught it.*

*Last updated: July 2026 · Part of the [Open-SDE](../README.md) research initiative.*

> **Maturity: Draft.** This taxonomy is an early, opinionated organizing device, not a
> validated hazard database. It is meant to be argued with, extended, and superseded by
> incident data. It is not a certification checklist.

---

## Purpose and framing

Open-SDE's governing thesis is **assured bounded autonomy**: the reasoning, intent
formation, and orchestration performed by a software agent under delegated authority may
be probabilistic, but the *authorization, control, and settlement* around it must be
deterministic and auditable. This mirrors the three-layer framework in the IMF's
April 2026 note [*How Agentic AI Will Reshape Payments*](https://www.imf.org/en/publications/imf-notes/issues/2026/04/22/how-agentic-ai-will-reshape-payments-575560),
which concentrates probabilistic reasoning in the intent/orchestration layer while
keeping authorization/control and settlement strictly deterministic.

Failures in such a system are therefore rarely "the model was wrong." They are failures
of the *bounding machinery* — the state inputs the agent reasons over, the Policy
Decision Point (PDP) that decides, the Policy Enforcement Point (PEP) that enforces, the
execution guardrails, and the reconciliation that compares what was recorded against what
actually happened.

Each entry below records:

- **Description** — what goes wrong.
- **Loop stage** — where in the SDE reference loop it originates.
- **SDE-0 requirement** — which item of the
  [SDE-0 minimum conformance profile](../docs/sde-0-conformance-profile.md) is meant to
  address it (numbered 1–7).
- **Monitoring category** — the relevant post-deployment monitoring dimension from NIST's
  final report [NIST AI 800-4, *Challenges to the Monitoring of Deployed AI Systems*](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.800-4.pdf)
  (March 2026), whose six categories are Functionality, Operational, Human Factors,
  Security, Compliance, and Large-Scale Impacts.

Two families of failure are drawn from the OWASP GenAI Security Project's first
[Top 10 for Agentic Applications](https://genai.owasp.org/2025/12/09/owasp-top-10-for-agentic-applications-the-benchmark-for-agentic-security-in-the-age-of-autonomous-ai/)
(December 2025): **Agent Goal Hijack (ASI01)** and unsafe halting / **kill-switch
cascade (ASI08)**.

### The loop stages referenced

```
Reality Signals → Authoritative State Model → Agent Decision
   → Governance Gate (PDP → PEP) → Execution → Settlement → Reconciliation
   → Continuous Monitoring
```

The *Authoritative State Model* is the general term for the state the agent reasons over;
the *Digital Twin* is its physical-domain specialization. Do not read "Digital Twin" as
implying a physical system where the domain is digital commerce or an API.

---

## Taxonomy

### F-01 · Stale or invalid state input

- **Description.** The agent reasons over a state input that is out of date, out of
  range, or internally inconsistent — a cached inventory count, a price quote past its
  freshness window, a sensor reading that never refreshed. The decision may be locally
  reasonable but is anchored to a world that no longer exists.
- **Loop stage.** Reality Signals → Authoritative State Model.
- **SDE-0 requirement.** #3 — state inputs must carry provenance, timestamp, freshness,
  and uncertainty, so a stale or invalid input can be detected and rejected before it
  reaches the agent.
- **Monitoring category (NIST AI 800-4).** Functionality (input-drift and data-quality
  monitoring); Operational (freshness/latency of upstream feeds).

### F-02 · Provenance spoofing

- **Description.** An input arrives bearing forged or unverifiable provenance — a signal
  claiming to come from a trusted oracle, twin, or upstream service that it did not.
  Blockchain immutability does not help here: an immutable record of a spoofed input is
  still a spoofed input. (See the non-claim "blockchain immutability ≠ truth of external
  inputs" in the [authority and safety model](../docs/authority-and-safety-model.md).)
- **Loop stage.** Reality Signals → Authoritative State Model.
- **SDE-0 requirement.** #3 — provenance must be carried *and verified*; unverifiable
  provenance is treated as absent provenance.
- **Monitoring category (NIST AI 800-4).** Security (integrity/authenticity of inputs);
  Functionality (anomaly detection on input sources).

### F-03 · Prompt injection / Agent Goal Hijack (OWASP ASI01)

- **Description.** Adversarial content embedded in a document, email, web page, tool
  response, or RAG corpus redirects the agent's objective — the tool-using evolution of
  prompt injection. OWASP ranks
  [Agent Goal Hijack (ASI01)](https://genai.owasp.org/2025/12/09/owasp-top-10-for-agentic-applications-the-benchmark-for-agentic-security-in-the-age-of-autonomous-ai/)
  as the leading agentic-security risk. The correct defense is *not* to make the model
  more trustworthy but to keep the authorization decision outside the model.
- **Loop stage.** Agent Decision.
- **SDE-0 requirement.** #4 — the PDP/PEP must be independent of the AI. A hijacked agent
  can *request* a harmful action, but the deterministic gate, which is not itself subject
  to injection, must still refuse anything outside the mandate.
- **Monitoring category (NIST AI 800-4).** Security (adversarial-input monitoring);
  Human Factors (detecting manipulation of agent behavior).

### F-04 · Authorization bypass

- **Description.** An action reaches an effector without a valid PDP decision — because a
  code path skips the gate, a default-allow rule is misconfigured, a token is over-scoped,
  or the PEP trusts an agent's self-asserted authority. Note the non-claim: *agent
  authentication ≠ authorization of a specific action.* Knowing which agent is calling
  does not tell you the call is permitted.
- **Loop stage.** Governance Gate (PDP → PEP).
- **SDE-0 requirement.** #4 — decision and enforcement must sit outside the model, with
  the PEP fail-closed. The PDP/PEP split follows the ABAC/XACML tradition standardized by
  the [OpenID AuthZEN Authorization API 1.0](https://openid.net/specs/authorization-api-1_0.html)
  (a Final OpenID specification, dated January 2026), which formalizes the API between a
  PEP that intercepts and a PDP that decides.
- **Monitoring category (NIST AI 800-4).** Security (access-control monitoring);
  Compliance (evidence that every action was authorized).

### F-05 · Budget overrun

- **Description.** Cumulative spend, resource consumption, or commitment exceeds the
  mandate's stated budget — through many individually-permitted small actions, a
  race between concurrent agents, or a missing running-total check at the gate.
- **Loop stage.** Governance Gate / Execution.
- **SDE-0 requirement.** #2 (the mandate must state a budget) and #5 (execution must
  enforce budget caps). Budget is a first-class deterministic control, not a soft prompt
  instruction.
- **Monitoring category (NIST AI 800-4).** Operational (resource/consumption tracking);
  Compliance (adherence to the mandated limit).

### F-06 · Rate or idempotency failure

- **Description.** The same intended action is executed more than once (a retried request
  with no idempotency key, a duplicated event), or actions fire faster than downstream
  systems can safely absorb. In a settlement context this is a double-spend or duplicate
  order.
- **Loop stage.** Execution.
- **SDE-0 requirement.** #5 — execution must provide idempotency and rate limits so a
  retry or burst cannot become a duplicated or runaway real-world effect.
- **Monitoring category (NIST AI 800-4).** Operational (throughput and duplicate-action
  monitoring); Functionality (effect matches intent exactly once).

### F-07 · Partial execution

- **Description.** A multi-step action completes some steps and not others — payment
  captured but order not placed, robot picked but did not place, first leg of a transfer
  settled and the second failed — leaving the world in an inconsistent intermediate state.
- **Loop stage.** Execution.
- **SDE-0 requirement.** #5 (a safe fallback / defined roll-back or compensating action)
  and #6 (reconciliation surfaces the inconsistency rather than assuming success).
- **Monitoring category (NIST AI 800-4).** Operational (transaction completeness);
  Functionality (end-state consistency).

### F-08 · Settlement-versus-outcome mismatch

- **Description.** The execution receipt says success, but the observed real-world outcome
  differs — the transaction cleared but the goods never shipped, the on-chain transfer
  confirmed but the counterparty never delivered. This is the direct expression of the
  non-claim *transaction success ≠ real action success.*
- **Loop stage.** Reconciliation.
- **SDE-0 requirement.** #6 — the execution receipt must be reconciled against the
  observed outcome; a receipt alone never closes the loop.
- **Monitoring category (NIST AI 800-4).** Functionality (outcome verification);
  Large-Scale Impacts (systematic gaps between recorded and real outcomes across many
  transactions).

### F-09 · Sim-to-real gap

- **Description.** A policy validated against a model, simulation, or twin behaves
  differently in reality because the model omitted a relevant dynamic. Expresses two
  non-claims: *Digital Twin ≠ ground truth* and *passing simulation ≠ real-world safety
  certification.* Most acute in physical domains but present wherever the Authoritative
  State Model diverges from the world it represents.
- **Loop stage.** Authoritative State Model / Execution.
- **SDE-0 requirement.** #3 (state inputs carry uncertainty, so twin-vs-reality divergence
  is representable) and #6 (reconciliation measures the realized gap).
- **Monitoring category (NIST AI 800-4).** Functionality (model-vs-reality error);
  Human Factors (operator overtrust in simulation results).

### F-10 · Monitor or PEP failure

- **Description.** The safety monitor or enforcement point is itself down, degraded, or
  bypassed — a crashed PEP that fails *open*, a monitor that stops emitting, a gate that
  silently allows while unhealthy. The bounding machinery, not the agent, is the point of
  failure. Runtime-assurance practice (Lui Sha's Simplex Architecture; ASTM F3269-21,
  [*Standard Practice for Methods to Safely Bound Behavior of Aircraft Systems Containing
  Complex Functions Using Run-Time Assurance*](https://store.astm.org/f3269-21.html))
  requires the monitor to be the *trusted, verified* component precisely because the whole
  safety argument rests on it.
- **Loop stage.** Governance Gate / Continuous Monitoring.
- **SDE-0 requirement.** #4 (an independent, fail-closed PEP) and #7 (continuous
  monitoring must include monitoring *of the monitor*).
- **Monitoring category (NIST AI 800-4).** Operational (health/liveness of the control
  plane); Security (integrity of the enforcement path).

### F-11 · Kill-switch cascade (OWASP ASI08)

- **Description.** A revocation, halt, or kill-switch — intended as the safe response —
  itself causes harm: halting one agent strands in-flight commitments, a mass revocation
  cascades through dependent agents, or a stop leaves the system in an unsafe partial
  state. Corresponds to the unsafe-halting / control-loss class in OWASP's
  [Top 10 for Agentic Applications (ASI08)](https://genai.owasp.org/2025/12/09/owasp-top-10-for-agentic-applications-the-benchmark-for-agentic-security-in-the-age-of-autonomous-ai/).
  Runtime assurance frames the correct target as *reversion to a verified-safe mode*, not
  a bare stop.
- **Loop stage.** Continuous Monitoring / Recovery.
- **SDE-0 requirement.** #7 — human override, recovery, and incident handling must revert
  to a defined safe state, not merely terminate, and must account for in-flight and
  dependent work.
- **Monitoring category (NIST AI 800-4).** Large-Scale Impacts (cascade and
  interdependency effects); Operational (recovery behavior under halt).

---

## Cross-cutting notes

- **Independence is the load-bearing property.** F-03, F-04, and F-10 all reduce to the
  same design commitment: the decision and enforcement of authority must be separable from
  the probabilistic component. An agent that can be hijacked must not also be the thing
  that authorizes its own actions.
- **Reconciliation is not optional.** F-07, F-08, and F-09 are only *detectable* if the
  loop compares receipts against observed outcomes (SDE-0 #6). Without it, these failures
  are silent.
- **The safe response is itself a hazard.** F-11 is a reminder that revocation and halting
  are actions with consequences and must be designed, tested, and monitored like any other
  execution path.

This taxonomy is not exhaustive. It is a starting structure; new categories should be
added as incidents are observed and logged (see
[incident-and-recovery.md](incident-and-recovery.md)).

---

## Related

- [hazard-log.yaml](hazard-log.yaml) — a starter hazard log instantiating these categories.
- [incident-and-recovery.md](incident-and-recovery.md) — detection, response, and recovery.
- [../docs/sde-0-conformance-profile.md](../docs/sde-0-conformance-profile.md) — the seven requirements referenced above.
- [../docs/authority-and-safety-model.md](../docs/authority-and-safety-model.md) — the PDP/PEP model, runtime assurance, and the eight non-claims.

See [../docs/references.md](../docs/references.md) for the full, annotated source list.
