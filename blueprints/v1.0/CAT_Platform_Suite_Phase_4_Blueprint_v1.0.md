# Phase 4 Blueprint — Guardian Federation (M9–M15)
## CAT-Aligned Platform Suite, Development Phase Series v1.0

**Phase designation:** Phase 4, the first post-development phase. It begins only if the Phase-3 exit gate passed and the boundary contract (Phase 3 §7.3) was signed and hash-chained. A Phase-3 gate that failed is a Phase-4 entry that does not exist.
**Scope:** Digest-quorum enforcement mixing Linux/Windows Servers, Mini-ARM edge Servers, and paired MCU edges; promotion of the Phase-3 pre-arm instruments from monitor-only to enforcement under a constitution that has not moved; fleet-scale reciprocity, fire-rate choke, and contraction gating; continuity of the falsifier registry (F1–F5) as an open empirical question.
**Inheritance:** D1.x–D3.x, T1.x–T3.x, V1-*–V3-*, kernel contracts, and the statistical rules of Phase 1 §6.3 and Phase 2 §4.6 remain normative. This document adds Phase-4 deltas only (D4.x, T4.x, P4.x, V4-*).
**Audience:** the conformance and guardian teams that operate the fleet after development ends, plus the operators who will hold break-glass and quorum seats.
**Epistemic labels:** unchanged — [NORM] · [DER] · [RIGOR] · [MAP] · [CORR] · [SPEC] (SIMTIME-gated, never load-bearing) · [EMPIRICAL] (named instrument + named falsifier).

**Phase thesis, stated honestly:** Phase 3 measured the fleet and withheld enforcement quorum. Phase 4 may grant that quorum only to digest voters who never hold key bytes, only inside a constitution frozen at Phase-3 exit, and only while the contraction, reciprocity, and saturation instruments that watched for the preceding interval still publish inside their pre-registered bands. Federation is not a new ethics. It is the same ten axioms, now applied to the watcher itself at fleet scale. If the watcher cannot pay its own ε, the phase fails closed and enforcement stays in monitor-only mode.

---

## 1. Phase Mission & Workstream Decomposition

### 1.1 Mission statement

Phase 4 delivers a **digest-quorum guardian federation**: enforcement acts that require a view of `3f+1` digest voters, tolerate `f` liars, carry no key material, and fail closed when the Phase-3 instruments diverge from their certificates. The exit condition is not “the guardian can act.” It is that every federated act is an impulse on the ledger, replayable by a disaligned party with the issuer offline, priced in the guardian’s own ε term, and void if `λ_net ≥ 1`, `ℛ < ℛ*`, or the fire-rate share crosses `f*`.

The control-plane budget of the phase is published before any enforcement flag is armed. Federation code is the control plane. If its byte share of the accountability plane exceeds the declared 1% bound, A3 vetoes promotion of the enforcement flag. That is the 160/166 lesson applied to the program that claims to have learned it.

### 1.2 Workstreams

| ID | Workstream | Deliverable | Primary bindings | Exit owner |
|---|---|---|---|---|
| W4.1 | Digest quorum | View-change and commit path over provenance only; key-bearing payloads rejected at parse | R2 #79, #30; D3.7 | Federation team |
| W4.2 | Enforcement gate | Constitution-locked promotion of monitor-only instruments to enforcement; dry-run write-set = ∅ on every writer including migrations | R2 #80, #84; T1.13, T3.11 | Guardian team |
| W4.3 | Priced watcher | Fleet fire-rate choke, independent budget meter, shadow-price publication, own-ε registration | R1 #5, #82; R2 #77, #82; T1.5, T3.9 | Budget team |
| W4.4 | Reciprocity & contraction | Continuous ℤ₂ canary games under an A3 game budget; Ω-point lock on promotion | R1 #6, #13, #81; R2 #77; T3.7, T3.8 | Governance team |
| W4.5 | External replay & drills | Issuer-offline δ_R replay; kill-switch continuity; break-glass impulse; chaos calendar on non-key limbs | R2 #83, #91, #96, #78 | Conformance team |

### 1.3 Non-goals (published, per U5)

Phase 4 MUST NOT: place key bytes in a quorum message, ledger row, receipt, or vote; treat differential-privacy fleet exports as enforcement inputs (R2 #73 remains an external reporting construct); cite ZK range proofs (R2 #74) as a new security theorem; attach QKD appliances or reinterpret `qkd: absent` as `present`; amend A1–A10 or soften any Phase-1/2/3 acceptance test; or run reciprocity games without a published A3 budget. A federation that needs a new axiom to justify itself has already failed A6.

---

## 2. Definitions & Notation (D4.x)

Cumulative on D1.x–D3.x. Conflicts resolve to the stricter definition.

- **D4.1 (Digest voter).** A platform authorized to sign a provenance digest and an ε snapshot. Eligible classes: Linux Server, Windows Server, Mini-ARM edge Server. MCU edges contribute limbs and feature vectors only; they are not voters. A voter’s signing key is an attestation key, not a data-plane key.
- **D4.2 (Quorum view).** A set of `3f+1` digest voters with a leader, a tip hash, and an epoch tuple. View-change messages carry the tip hash and the voter set hash. A message containing key bytes is not a view-change; it is a parse failure.
- **D4.3 (Enforcement certificate).** The object a voter signs: `{policy_hash, inputs_hash, class, ε_sum, λ_price, λ_net, ℛ, tip, epoch_tuple}`. It is not a capability token and cannot authorize key release.
- **D4.4 (Federation flag).** A constitution parameter, default `monitor`. Transition to `enforce` is a policy impulse requiring dry-run write-set equality, a green contraction reading, and a green independent budget-meter delta. The reverse transition (back to `monitor`) is unilateral and fail-closed.
- **D4.5 (Game budget).** The A3 cap on reciprocity canary injections per epoch. Excess is a guardian fire-rate event, not an extra test.
- **D4.6 (Issuer-offline replay).** Verification of δ_R that does not require the issuer’s online signature. The bundle format alone suffices (R2 #83).

Notation: `f` Byzantine tolerance; `Q = 3f+1`; `f*` the saturation threshold from D1.4; `ℛ* ≈ 1.15`.

---

## 3. Formal Mathematics of Phase 4 (Theorems T4.x)

### T4.1 — Digest-Only Quorum Theorem [DER]

**Statement.** A federation commit is admissible only if every vote parses as an enforcement certificate (D4.3) and the type system rejects key-material fields. Safety: no two conflicting certificates commit in the same epoch tuple. Liveness under partial synchrony: a correct leader commits when at least `2f+1` correct voters are reachable inside the A4 latency bound.

**Proof sketch.** Custody is structural: the CBOR schema has no key field, and an unknown field is a hard parse error, not an ignore (the extension rule of T1.8). Safety follows the standard quorum intersection of two `2f+1` sets inside `3f+1`. Liveness is conditional on the causal cone (A4): a commit stamped faster than `d/c_n + t_attest + t_quorum` is refused as an impossible object (T1.9). Key absence is not a performance property; a single key byte voids the view.

**Prediction P4.1.** In V4-QUORUM, 100% of key-bearing votes are dropped before tally; conflicting certificates do not both commit; a commit under a forged instant stamp is refused.

**Binding.** R2 #79, #30; `ccl-replay` closed schema; Phase-3 shadow drills (V3-QUORUM) are the entry corpus.

### T4.2 — Monitor-to-Enforce Promotion Theorem [DER]

**Statement.** The federation flag may move from `monitor` to `enforce` only when, in the same epoch: (i) dry-run write-set equals the declared set (R2 #80); (ii) measured `λ_net < 1` with `C_S ≥ 0.8` (T3.7); (iii) `ℛ ≥ ℛ*` on the pre-registered window; (iv) independent budget-meter delta is inside tolerance (R2 #82); (v) the flag change itself is an impulse naming the signer set. Any missing conjunct refuses. The reverse edge does not require the conjuncts.

**Proof sketch.** (i) is A6: a migration that writes an undeclared key is legislation. (ii)–(iii) are the Ω-point and reciprocity locks already proven as promotion gates. (iv) stops the guardian grading its own budget. (v) is A1: a mode change without an impulse is unlogged decay. Asymmetry of the reverse edge is fail-closed discipline — returning to monitor must not depend on the health of the thing being demoted.

**Prediction P4.2.** A promotion attempt missing any conjunct fails with a typed receipt naming the conjunct. A demotion under a stalled ledger still completes or the watchdog resets the node (R2 #72).

**Binding.** R2 #80, #82; T3.7, T3.11; D4.4.

### T4.3 — Watcher Saturation at Federation Scale [DER]

**Statement.** Let detectors `i = 1…m` have fire rates `f_i`. The federation charge is `ε_guardian = Σ_i (f_i · ε_act,i + f_i · p_fp,i · ε_wrong,i)`. Enforcement is admissible only while `ε_guardian` does not dominate `ε_total` and no aggregate of individually sub-threshold detectors crosses `f*`. The choke is itself a limb: choke failure vetoes enforcement (A8).

**Proof sketch.** Linearity of the sum is T1.1 applied to the watcher. Aggregate dominance is the Phase-3 five-detector case (T3.9) restated as a commit predicate rather than a monitor alarm. Making the choke a limb closes the regression in which the safety mechanism fails open.

**Prediction P4.3.** V4-CHOKE: five sub-threshold detectors that jointly dominate are choked within one epoch; a forced choke failure vetoes the next enforcement certificate.

**Binding.** D1.4; R1 #82; R2 #82; T1.5, T3.9.

### T4.4 — Budgeted Reciprocity Theorem [DER]

**Statement.** Agent-swap canaries are admissible only inside the game budget (D4.5). A rule that fails swap invariance is refused before execution, and the refusal is an impulse. Uncapped game loops are a fire-rate event.

**Proof sketch.** A2 requires the symmetry; A3 requires the test to pay. An uncapped red-team is a guardian that manufactures violations — the failure A7 exists to stop. Pre-execution refusal is the Phase-3 T3.8 result promoted from evidence to gate.

**Prediction P4.4.** Swap-violating rules in V4-SWAP never execute; game rate above cap trips the choke; both events reconcile 1:1 with ledger rows.

**Binding.** R1 #13; R2 #77; T3.8.

### T4.5 — Issuer-Offline Replay Theorem [DER]

**Statement.** For every federated enforcement certificate, a third party with the bundle and the constitution hash, and without the issuer online, recomputes δ_R. Non-zero δ voids the certificate and every sanction that depends on it. Issuer signature is not an input to the recomputation.

**Proof sketch.** A10 names the third party, not the issuer, as the verifier. If replay required the issuer, the issuer would be a permanent trust state, which A9 forbids. Bundle closure (policy, inputs, clock, RNG, ε sum, honesty enum, tip) is already T1.8; Phase 4 adds only that the voter set hash is inside the inputs hash so a changed quorum cannot be replayed as the original act.

**Prediction P4.5.** V4-OFFLINE: issuer processes stopped; δ_R = 0 on unmodified bundles; any mutated voter-set hash voids.

**Binding.** R2 #83, #96; T1.8, T3.10.

### T4.6 — Break-Glass Impulse Theorem [DER]

**Statement.** Emergency access is representable only as a threshold-unlocked, time-boxed impulse with a person-identity, a reason code, and an expiry. No shell, token, or quorum override exists outside that impulse. Expiry kills the session even if the signature still verifies.

**Proof sketch.** A hidden root is authority created ex nihilo (U1) and authority without a time-reverse (A9). Threshold unlock prevents a single operator from minting the impulse. Logging is the impulse itself; a session the ledger does not show cannot be opened, because the open path checks the tip.

**Prediction P4.6.** V4-GLASS: open attempts without an impulse fail; sessions past expiry fail closed; the impulse names a person and a reason on 100% of successful opens.

**Binding.** R2 #91; Phase-3 Stage P2 rehearsal.

### T4.7 — External-Report Isolation Theorem [SPEC]

**Statement.** Fleet dashboards may emit a declared differential-privacy aggregate. That aggregate is not an enforcement input. A path from a DP export or a model score to a sanction sink without an A6 constitution lookup fails CI.

**Label.** Engineering isolation, not a privacy theorem and not a security proof. R2 #73 and #74 stay SPEC.

**Prediction P4.7.** An export containing raw per-node QBER fails the privacy schema. A seeded model score that triggers sanction without an A6 lookup fails CI.

**Binding.** R2 #73, #74, #84.

---

## 4. Platform Roles Inside the Federation

| Platform | Federation role | May vote | Must not |
|---|---|---|---|
| Linux Server | Reference voter and conformance witness | Yes | Hold data-plane keys in the vote path |
| Windows Server | Enterprise voter; TPM PCR quote in the bundle | Yes | Treat vTPM as hardware-equivalent without priced `d_impl` |
| Mini ARM | Edge voter on RK3588 class; RPi4 class votes only if its own capacity card is advertised | Yes, class-separated | Advertise the RK3588 card from an RPi |
| macOS | Small-fleet voter | Yes | Hide SIP/extension gaps |
| Android / iOS | Clients and episodic nodes; never voters | No | Receive a voter key by backup or migration |
| MCU | Limb source and paired edge; feature vectors only | No | Self-certify novelty or sign a certificate |

Quorum geography SHOULD prefer vertex-disjoint sites. A single-bridge voter set publishes `WATCH` and MUST NOT admit new OTP-class sessions (R2 #86), which in this fleet means it must not admit any session whose honesty field has been coerced toward `present`.

---

## 5. Drills, Vectors, and Gates

### 5.1 V4-* vectors

| Vector ID | Content | Proves | Pass condition |
|---|---|---|---|
| V4-QUORUM | Key-byte injection, conflicting certificates, instant stamps | T4.1 | Drop, no double-commit, refuse |
| V4-PROMOTE | Five conjuncts independently removed | T4.2 | Typed refusal naming the conjunct |
| V4-CHOKE | Five-detector aggregate plus choke-failure | T4.3 | Choke ≤ 1 epoch; veto on choke failure |
| V4-SWAP | Swap-violating rules and uncapped game loop | T4.4 | Pre-execution refusal; choke on excess |
| V4-OFFLINE | Issuer stopped; third-party replay | T4.5 | δ_R = 0; mutation voids |
| V4-GLASS | Break-glass with and without impulse | T4.6 | Impulse required; expiry kills |
| V4-DP | Raw QBER export; model-score sanction seed | T4.7 | Schema fail; CI fail |
| V4-KILL | Scheduled halt on staging federation | R2 #96 | Third party matches ∫σ; no unlogged decay |

All V1-*–V3-* vectors stay green. A Phase-4 change that weakens an earlier vector reopens that phase gate.

### 5.2 Exit criteria (M15)

| # | Metric | Go threshold | No-go action |
|---|---|---|---|
| G1 | T4.1–T4.6 acceptance | 6/6 green; T4.7 quarantined as SPEC | Remain in monitor-only |
| G2 | V4 corpus plus prior vectors | No vector weakened | Reopen the affected phase |
| G3 | Issuer-offline replay | δ_R = 0 with issuer down on the staging sample | Do not arm `enforce` |
| G4 | Own-ε and independent meter | Guardian term published; meter delta inside tolerance | Flag is unrepresentable |
| G5 | Key custody | Zero key bytes in votes, receipts, and ledger rows across the corpus | Release blocked |
| G6 | Constitution stability | A1–A10 and Phase-1/2/3 gates unchanged since the boundary signature | Amendment path only; no silent edit |

**Rollback.** Any failed gate leaves the federation flag at `monitor`. There is no partial enforcement mode. A declared degraded federation publishes the reduction in every attestation, exactly as a node publishes a reduced profile.

---

## 6. Honesty Ledger (Phase 4 — kept)

1. **Federation does not create quantum security.** `qkd` remains `present`, `absent`, or `degraded` per tunnel. No voter can upgrade `absent` by quorum.
2. **The watcher is inside its own equations.** ε_guardian, λ_price, and the independent meter are commit predicates, not dashboard ornaments.
3. **SPEC items stay out of the promotion logic.** DP exports and range proofs do not gate enforcement.
4. **F1–F5 stay open.** A green federation is an instrument reading. It is not a verdict that CAT is true.
5. **This document prices itself.** Six falsifiable theorems, one quarantined SPEC, eight vectors, six gates. Softening any of them without a signed constitution amendment voids the phase.

**STATUS:** Federation specified as digest quorum · enforcement flag default monitor · 6 theorems armed · issuer-offline replay required · 0 key-custody paths · 0 silent promotions representable.

**COHERENCE:** Fleet-measured CI, ε decomposition, ℛ, and λ_net continue to publish every quarter. Phase 4 adds only the commit predicate that refuses to act when those series leave their pre-registered bands.
