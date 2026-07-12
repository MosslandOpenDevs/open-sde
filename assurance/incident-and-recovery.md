# Incident and Recovery

*How an assured-bounded-autonomy loop detects that something has gone wrong, contains it, reverts to a verified-safe state, and learns from it.*

*Last updated: July 2026 · Part of the [Open-SDE](../README.md) research initiative.*

> **Maturity: Draft.** This document describes an incident-handling model, not an
> operational runbook. It is a research artifact meant to be adapted to a specific
> deployment. Open-SDE is a research frame, not a certification scheme or an
> operationally-ready system.

---

## Why incident handling is a first-class concern

In a Software-Defined Economy, software agents act under delegated authority to initiate
state-changing actions. When one of the failure modes in
[failure-taxonomy.md](failure-taxonomy.md) occurs, the loop must move quickly from
*detection* to *containment* to *recovery* — and, crucially, the recovery action must
itself be safe. The recurring discipline throughout is the one NIST sets out in its
final March 2026 report
[NIST AI 800-4, *Challenges to the Monitoring of Deployed AI Systems*](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.800-4.pdf):
pre-deployment testing cannot anticipate every real-world outcome, so continuous
post-deployment monitoring across multiple dimensions is required to surface incidents at
all.

Two design commitments frame everything below:

1. **Separate probabilistic from deterministic.** Detection may lean on probabilistic
   anomaly signals, but *response* — revoking a mandate, blocking at the PEP, reverting to
   a safe mode — runs through the deterministic control plane, independent of the agent.
   This is the same separation the IMF's April 2026 note
   [*How Agentic AI Will Reshape Payments*](https://www.imf.org/en/publications/imf-notes/issues/2026/04/22/how-agentic-ai-will-reshape-payments-575560)
   draws between the probabilistic intent layer and the deterministic authorization and
   settlement layers.
2. **Recovery means reversion to a verified-safe mode, not a bare stop.** This is
   inherited runtime-assurance practice — Lui Sha's Simplex Architecture and ASTM
   F3269-21, [*Standard Practice for Methods to Safely Bound Behavior of Aircraft Systems
   Containing Complex Functions Using Run-Time Assurance*](https://store.astm.org/f3269-21.html)
   — in which a verified safety monitor reverts an untrusted complex function to a
   verified baseline when a violation is imminent. Simply halting can itself be a hazard
   (see F-11, kill-switch cascade).

---

## Incident classes

Incident classes group the failure taxonomy into response-relevant buckets. The point of
grouping is that incidents in the same class share a containment and recovery pattern.

| Class | Taxonomy entries | Primary response pattern |
| --- | --- | --- |
| **C1 · Corrupted state / input** | F-01 stale input, F-02 provenance spoofing, F-09 sim-to-real gap | Quarantine the suspect input source; freeze decisions that depend on it; fall back to last-known-good or human-sourced state. |
| **C2 · Compromised decision** | F-03 agent goal hijack (ASI01) | Treat the agent as untrusted; rely on the independent PDP/PEP to refuse out-of-mandate actions; revoke the mandate; isolate the agent. |
| **C3 · Authority violation** | F-04 authorization bypass, F-05 budget overrun | Block at the PEP; revoke or narrow the mandate; reconcile what was already permitted against what was authorized. |
| **C4 · Execution fault** | F-06 rate/idempotency failure, F-07 partial execution | Stop further execution; run compensating/rollback actions; use reconciliation to determine true end-state. |
| **C5 · Outcome divergence** | F-08 settlement-vs-outcome mismatch | Open a reconciliation exception; withhold downstream actions that assumed success; escalate to human exception handling. |
| **C6 · Control-plane failure** | F-10 monitor/PEP failure, F-11 kill-switch cascade (ASI08) | Fail closed; revert dependent agents to safe mode in a controlled sequence; restore the monitor before resuming autonomy. |

---

## 1 · Detection

Detection is the continuous-monitoring obligation of SDE-0 requirement #7, organized along
the six NIST AI 800-4 monitoring categories so that no failure family is unobserved:

- **Functionality monitoring** — decision quality, input drift, and, above all,
  reconciliation signals: does the observed outcome match the receipt? (Catches C1, C4,
  C5.)
- **Operational monitoring** — latency, throughput, duplicate-action rates, budget
  burn-down, and the *liveness of the control plane itself*. (Catches C3, C4, C6.)
- **Human Factors monitoring** — signs of manipulation or operator overtrust, e.g.
  behavior consistent with goal hijack, or unquestioned reliance on simulation output.
  (Catches C1, C2.)
- **Security monitoring** — input authenticity, access-control violations, and integrity
  of the enforcement path. (Catches C2, C3, C6.)
- **Compliance monitoring** — evidence that every executed action carried a valid
  authorization and stayed within mandate. (Catches C3, C5.)
- **Large-Scale Impacts monitoring** — systematic patterns across many transactions:
  correlated outcome divergence, or cascade effects during a halt. (Catches C5, C6.)

Detection produces an **incident signal** carrying: the failure taxonomy id, the affected
mandate(s) and agent(s), the loop stage, and the state and receipts in scope. Because
detection can be probabilistic, a signal is a *trigger for deterministic response*, not a
verdict.

---

## 2 · Response (containment)

Response runs entirely through the deterministic control plane. The three primary levers,
in escalating order:

1. **Revoke or narrow the mandate.** Every mandate states its revocation conditions
   (SDE-0 requirement #2). Revocation removes the agent's delegated authority at the
   source: with no valid mandate, the PDP returns deny. This is the cleanest containment
   because it does not depend on trusting the agent to stop.
2. **Block at the Policy Enforcement Point.** The PEP intercepts the agent's requests and
   enforces the PDP verdict; the PDP/PEP split follows the ABAC/XACML tradition
   standardized by the [OpenID AuthZEN Authorization API 1.0](https://openid.net/specs/authorization-api-1_0.html)
   (a Final OpenID specification, dated January 2026). Blocking at the PEP stops in-flight
   and future actions regardless of what the (possibly hijacked) agent intends — the
   correct containment for C2 and C3, where the agent itself cannot be trusted.
3. **Revert to a verified-safe mode.** Per runtime assurance, the system hands control from
   the untrusted complex function to a verified baseline: a conservative fallback policy, a
   read-only posture, or a defined hold state (SDE-0 requirement #5's safe fallback). For
   control-plane failures (C6) this reversion must be *sequenced* across dependent agents
   to avoid a kill-switch cascade (F-11) — revert, confirm safe state, then proceed to the
   next dependency, rather than issuing a single system-wide stop.

Throughout containment, the human role is the one Open-SDE assumes everywhere: humans set
authority, budgets, and revocation policy and handle exceptions — "AI recommends, humans
decide." A person can trigger revocation or a safe-mode reversion at any point; they are
not out of the loop, their role has moved from per-action approval to override and
exception handling.

The agent-identity substrate that revocation depends on is still maturing: registries such
as ERC-8004 are in **Draft** status ([ERC-8004 *Trustless Agents*](https://eips.ethereum.org/EIPS/eip-8004),
created August 2025 and still Draft as of mid-2026), and government work such as NIST's
[AI Agent Standards Initiative](https://www.nist.gov/artificial-intelligence/ai-agent-standards-initiative)
(announced February 2026) is still shaping how agent identity and authorization will be
standardized. Treat identity-and-revocation plumbing as unfinished, not settled.

---

## 3 · Recovery

Recovery restores the system to a correct, consistent state — not merely a stopped one.

- **Establish true end-state through reconciliation.** Before undoing anything, compare
  execution receipts against observed outcomes (SDE-0 requirement #6). A partial execution
  (F-07) or an outcome divergence (F-08) can only be corrected once the real end-state is
  known; a receipt alone is insufficient.
- **Apply compensating actions.** For C4 execution faults, run the defined rollback or
  compensating transaction (refund, cancel, re-issue) rather than re-running the original,
  which idempotency controls should already have made safe to retry.
- **Rebuild trustworthy state.** For C1, discard quarantined inputs and rebuild the
  Authoritative State Model from verified provenance before decisions resume.
- **Restore the control plane first.** For C6, the monitor/PEP must be verified healthy
  *before* autonomy is re-enabled — never resume agent action while the bounding machinery
  is degraded.
- **Resume autonomy incrementally.** Re-grant a narrowed mandate (reduced scope, budget,
  or duration), observe under heightened monitoring, and widen only as confidence returns.

Recovery is complete when reconciliation shows receipts and outcomes agree, the control
plane is verified healthy, and the agent is operating under a valid, appropriately scoped
mandate.

---

## 4 · Post-incident review

Every incident feeds back into the assurance artifacts:

- **Log the incident.** Record the taxonomy id, timeline, detecting monitoring
  category, containment levers used, recovery actions, and residual impact. Incident
  logging is itself SDE-0 requirement #7.
- **Update the hazard log.** Adjust likelihood/severity in
  [hazard-log.yaml](hazard-log.yaml), change a hazard's status, or add a new entry if the
  incident revealed a category the taxonomy did not name.
- **Extend the taxonomy.** If the failure did not fit an existing F-NN category, add one
  — [failure-taxonomy.md](failure-taxonomy.md) is explicitly non-exhaustive.
- **Close the loop on controls.** Ask which SDE-0 requirement was absent or inadequate,
  and whether a PDP policy, an execution guardrail, or a monitoring category needs to
  change. This mirrors NIST AI 800-4's central point: monitoring exists to surface what
  pre-deployment testing could not, and its findings must flow back into the system.

Post-incident review deliberately avoids treating "the model made a mistake" as a root
cause. In an assured-bounded-autonomy frame, the informative question is why the
*deterministic bounding machinery* did not contain the probabilistic component — because
that machinery is what the system's safety argument actually rests on.

---

## Related

- [failure-taxonomy.md](failure-taxonomy.md) — the failure categories referenced by incident class.
- [hazard-log.yaml](hazard-log.yaml) — the hazard log this process updates.
- [../docs/sde-0-conformance-profile.md](../docs/sde-0-conformance-profile.md) — the seven requirements, including monitoring and override.
- [../docs/authority-and-safety-model.md](../docs/authority-and-safety-model.md) — PDP/PEP, runtime assurance, and the eight non-claims.

See [../docs/references.md](../docs/references.md) for the full, annotated source list.
