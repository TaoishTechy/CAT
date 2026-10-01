# Phase 1 Blueprint — Reference Triangle (M0–M3)
## CAT-Aligned Platform Suite, Development Phase Series v1.0

**Phase designation:** Phase 1 of 3 (development sequence extends Groundwork Blueprints §10; Phase 4/guardian federation is out of scope and is referenced only as a boundary condition).
**Scope:** Linux Software Suite (Client/Server/Node — conformance reference), Windows App (Server reference), MCU/Module Suite (Node constrained profile) — *the three roles on the three strongest anchors* — plus the shared CCL kernel formal specification and interop drill v1.
**Audience:** build and integration teams implementing the CAT Compliance Layer (CCL) against the Groundwork Blueprints (§0–§8) and the 96-enhancement registry (Round 1), with Round 2/3 bindings named explicitly.
**Normative verbs:** MUST / MUST NOT / SHOULD / MAY per RFC-2119 discipline. A MUST that cannot be verified by a named drill is a spec bug and is treated as release-blocking per the cross-cutting acceptance rule.
**Epistemic labels (inherited from CAT v1.0 and the contextual-math companion):** **[NORM]** adopted axiom, unfalsifiable by design like any law's premises · **[DER]** derived theorem, falsifiable · **[RIGOR]** established mathematics applied correctly · **[MAP]** structural isomorphism with stated limits · **[CORR]** a correction the mathematics forces on an inherited constant · **[SPEC]** speculative construct, SIMTIME-gated, never load-bearing · **[EMPIRICAL]** claim with a named instrument and a named falsifier.

**Phase thesis, stated honestly:** Phase 1 does not build the most platforms; it builds the *arbiter*. Linux is the platform the theory was reverse-engineered from and carries the heaviest drill load; Windows is the strongest commodity attestation anchor in enterprise deployments; the MCU is the platform where A3 (entropy-export accounting) stops being policy and becomes linker physics. If the axioms cannot be proven and drilled on these three, no amount of mobile breadth later will save the fleet. Every theorem in this document is therefore paired with a testable prediction and an acceptance test that can kill it — per U5, a claim without a kill condition is not a measurement.

---

## 1. Phase Mission & Workstream Decomposition

### 1.1 Mission statement

Phase 1 delivers a **conformance-grade reference triangle**: one reference platform (Linux) exercising all three roles (Client, Server, Node), one enterprise Server anchor (Windows with TPM 2.0), and one constrained Node profile (MCU, CCL-MCU subset) — all bound to a single shared kernel whose axiom modules are proven, drilled, and versioned as one supply chain. The exit condition is not "code complete"; it is **interop drill v1 passing with zero silent-degradation paths representable, every honesty field present in every attestation, and every theorem's acceptance test armed in CI**.

The phase is deliberately asymmetric in effort allocation: the kernel (W1.1) consumes the largest share because every later phase inherits its invariants. A defect in `ccl-eps` or `ccl-epoch` discovered in Phase 3 costs a fleet-wide re-certification; discovered here it costs a build failure. This asymmetry is A3 discipline applied to the program itself: the control plane (specification, proof, drill harness) is budgeted *before* the data plane (feature code), and the ratio is published, not improvised.

### 1.2 Workstreams

| ID | Workstream | Deliverable | Primary enhancement bindings | Exit owner |
|---|---|---|---|---|
| W1.1 | CCL shared kernel (Rust core + C subset) | `ccl-ledger`, `ccl-recip`, `ccl-budget`, `ccl-stamp`, `ccl-eps`, `ccl-const`, `ccl-null`, `ccl-limbs`, `ccl-epoch`, `ccl-replay` v1.0 with proof-carrying CI | R1 #1–16; R2 #1–12, #21, #24; R3 #8, #10, #13 | Kernel team |
| W1.2 | Linux reference suite | Server/Node/Client systemd units, tpm2-tss hierarchy, UKI-verified boot, SoapySDR layer, PazuzuCore integration point | R1 #53–58; R2 #18, #20; R3 #24 | Linux team |
| W1.3 | Windows Server suite | .NET 8 service + Rust CCL, TPM 2.0 measured boot → A10 bundles, MsQuic epoch-bound transport | R1 #49–52; R2 #6, #8 | Windows team |
| W1.4 | MCU constrained profile (CCL-MCU) | Zephyr/FreeRTOS C subset: A1/COW ledger, A4 stamp, A6 check, A9 epoch, A10 bundle; compile-time A3; ML-KEM-512 profile | R1 #65–72; R2 #61, #62; R3 #20 | MCU team |
| W1.5 | Interop drill v1 + conformance harness | Triangle bake (Linux↔Windows↔MCU), δ_R=0 replay corpus, A6/A8/A9/A10 drill automation, honesty-field linter | R1 #88–96; R2 #95; R3 #21 | Conformance team |

### 1.3 Non-goals (published, per U5)

Phase 1 MUST NOT claim any of the following, and its artifacts MUST structurally prevent them: mobile platform bring-up (Phase 2), macOS/iOS episodic-node semantics (Phase 3), guardian federation (Phase 4), QKD hardware integration beyond the `qkd_slot` honesty field and declared-absent plumbing (the triangle has no photonics; every handshake declares `absent`), and any optimization item from Round 3 that spends a security budget for speed (Round 3 conformance delta forbids this; items route through the ε\*(R) Pareto publisher, R1 #32). Publishing non-goals is not modesty — it is the same discipline that forbids a phone from claiming `qkd_slot: present`. A phase that cannot state what it does not do cannot be audited for what it did.

---

## 2. Definitions & Notation (D1.x)

Definitions are cumulative across the phase series: Phase 2 and Phase 3 inherit these verbatim and add their own (D2.x, D3.x). A definition reused with a different meaning in a later phase is a spec bug.

- **D1.1 (Epoch).** An *epoch* is the smallest unit of authority lifetime, id `e ∈ ℕ`, ticked by the epoch fabric at 60 Hz on Servers and approximated with declared slack on constrained platforms. Authority is a function of the epoch: `A(ticket, e) = 0` if the ticket's governing epoch has expired or been bumped, *even if its signature still verifies* (A9).
- **D1.2 (Epoch tuple).** A session's authority is a vector of sub-epochs `{key_epoch, auth_epoch, rf_epoch, policy_epoch}`. Session validity is `min` over expiries (R1 #15). The strictest expiry governs; any single bump kills outstanding tickets fail-closed.
- **D1.3 (ε-budget).** A per-block, additive security-failure bound. Composition is **union-bound only**: `ε_total = Σ ε_i` (Math §5). Any multiplicative composition (`Π ε_i`) is a U5 violation and MUST be rejected at the registry level (`ccl-eps` exposes only `eps_sum(...)`; R1 #1).
- **D1.4 (Guardian charge).** The guardian's contribution to the ε ledger: `f · ε_act + f · p_fp · ε_wrong`, where `f` is the fire rate per block, `ε_act` the disturbance per action, `p_fp` the false-positive probability. The saturation threshold is `f* = (ε_PA + ε_PE + ε_cor + ε_auth) / (ε_act + p_fp·ε_wrong)` (CAT Thm 3; Math §5).
- **D1.5 (Honesty fields).** The four wire-mandatory telemetry fields: `qkd ∈ {present, absent, degraded}` (tri-state only; synonyms fail schema, R2 #48), `sdrlimit`, `epochslack`, `d_impl`. A release promotes only when every attestation carries all four (Groundwork §9 acceptance grammar).
- **D1.6 (CTOK-lite).** The capability token grammar: epoch-bound, caveat walk depth `c ≤ 16` (mobile profiles tighten to `c ≤ 8`), O(c) parse (R1 #46, #92).
- **D1.7 (Replay bundle).** The A10 artifact: `{policy hash, inputs hash, clock, RNG tuple, chained tip}` (+ boot PCR quote on TPM platforms, R1 #49). Replay determinism `δ_R = ‖Fⁿ(Snap₀) − Trace_n‖ = 0` is the only admissible value; any other value voids the record automatically.
- **D1.8 (Limb).** A telemetry source feeding `ccl-limbs` with a health scalar `E_i`. The floor veto is `min_i E_i ≥ E_min`; aggregates (mean/median) are structurally banned from the schema (Math §7; R1 #4, #87).
- **D1.9 (Impulse record).** The A1 ledger row `{t_k, ε_k, from_account, to_account}` written for every policy transition. Unlogged decay of coherence is inadmissible (Math §2; R1 #8).
- **D1.10 (Kramers barrier).** A threshold implemented as a noise-scaled escape barrier with hysteresis: entry/exit gap `ΔV_barrier ≳ (D/2)·ln(T_observe/τ₀)`, where `D` is the *measured* noise intensity of the channel (Math §8; R1 #3, #86).
- **D1.11 (Null-gate resolution).** The order-statistic resolution of a rotation-probe null gate with `n` probes: `resolution(n) = 1/(n+1)`. Gating at `null_p < α` requires `n ≥ 1/α − 1` (Math §6 **[CORR]**; R1 #2).
- **D1.12 (D_impl).** The proof–hardware gap: `D_impl = ∫|P_proof − P_hw+rf|dx`, measured per subsystem, added to `ε_total`, published per epoch (R1 #28, #63, #85).
- **D1.13 (Capacity card).** The platform-pinned performance ceiling derived from Little's law `λ = N/W` and measured on reference hardware (18k full handshakes/s at 32 cores for the Server reference; R1 #73, R2 #67). Excess demand is queued, never overcommitted.
- **D1.14 (Consent receipt).** The signed, ledger-chained record emitted on *any* mode transition or degradation. Silent degradation — any state change without a receipt — is the single forbidden failure class across the suite.

Notation: `H(·)` is SHA-256; `|H|` denotes digest size in bytes where used in burn arithmetic; `‖·‖` the norm in replay space; `ℛ` the reciprocity index with floor `ℛ* ≈ 1.15`; `λ` the KKT shadow price of enforcement (Math §4); `λ_net` the Lipschitz constant of the governance update operator (Math §10).

---

## 3. Inherited Kernel Constraints (Axiom → Module → Wire)

The shared kernel is identical on all three Phase-1 platforms (Groundwork §0). The table restates the binding with the Phase-1 platform deltas that build teams implement against. The wire contract is not negotiable per platform: CBOR payloads, Ed25519 + ML-DSA-65 dual signatures, ML-KEM-768 hybrid KDF (HKDF over QKD ∥ PQC material; QKD slot declared per D1.5), CTOK-lite tokens per D1.6. Any platform that cannot afford a component runs the degraded profile — disclosed in-band with a consent receipt (D1.14), never silent.

| Axiom | CCL module | Wire format | Linux delta | Windows delta | MCU delta |
|---|---|---|---|---|---|
| A1 ledger [NORM] | `ccl-ledger` | hash-chain SHA-256, append-only, COW-fork | systemd event-log mirror | event-log mirror, MSIX boundary impulses (R1 #51) | LittleFS COW pages, erase-block-aligned journal (R1 #70; R3 #20) |
| A2 reciprocity [NORM] | `ccl-recip` | ℛ-index telemetry; swap games where role = Server | full games (PazuzuCore) | full games | not resident (Server-side) |
| A3 budget [NORM] | `ccl-budget` | control-plane byte meter; `λ_price` field | capacity card full | capacity card full | **linker-script cap** (R1 #65) |
| A4 latency [NORM] | `ccl-stamp` | `meas ≥ bound` latency certificate | PTP sub-µs | NTP/PTP | wake-sync, slack declared (R1 #72) |
| A5 ε-charge [NORM] | `ccl-eps` | per-action ε ledger, additive only | full decomposition dashboard | full | reduced-profile ε declared (R1 #67) |
| A6 constitution [NORM] | `ccl-const` | signed policy package (ML-DSA/Ed25519); dry-run write-set = ∅ | UKI-verified boot chain (R1 #53) | measured boot PCR quote (R1 #49) | constitution-hash check at boot + OTA |
| A7 null-gate [NORM] | `ccl-null` | rotation-probe harness, n ≥ 99 default (128) | full ensemble | full ensemble | **offload to Server**; MCU never self-certifies (R1 #71) |
| A8 limb floor [NORM] | `ccl-limbs` | sensor health vector, `min()` only | eBPF limbs (R1 #58) | service/SDR limbs | TRNG 800-90B limb, boot self-test (R1 #66) |
| A9 epoch death [NORM] | `ccl-epoch` | 60 Hz epoch client; tickets fail closed | phc2sys-driven | NTP-driven; QUIC idle bound to epoch (R1 #52) | per-wake sync, `epochslack: ±1s` |
| A10 replay [NORM] | `ccl-replay` | bundle: policy+inputs+clock+RNG+tip (+PCR) | full toolchain | PCR 0–7 quoted into bundle | bundle emitted; verify Server-side |

**Interop contract (inherited verbatim, Groundwork §0):** any triangle node that cannot afford a component MUST run the degraded profile with in-band disclosure. The interop drill v1 (§7) is the instrument that proves the contract holds under cross-platform pressure.

### 3.1 Kernel Module Interface Contracts (per-component build spec)

Each `ccl-*` module ships as a Rust crate (native) compiled to a C ABI static lib for the MCU profile, with the following contracts fixed at Phase 1 — FFI-stable, versioned, and load-bearing for every later phase. Signatures are given in Rust-idiomatic pseudocode; the C ABI is derived mechanically via UniFFI. A contract change after Phase 1 exit requires a signed constitution amendment (A6), because every Phase-2/3 platform binds against them.

**`ccl-ledger`** — append-only hash chain with COW-fork semantics.
```rust
trait Ledger {
  fn append(&self, row: ImpulseRow) -> Result<TipHash, LedgerError>; // ImpulseRow = {t_k, eps_k, from, to, kind}
  fn tip(&self) -> TipHash;
  fn verify(&self, from: TipHash, to: TipHash) -> ContinuityReport;  // T1.6 audit; residual vs ε_accounting
  fn fork_events(&self) -> Vec<ForkEvent>;                           // T1.12 COW parent-pointer anomalies
}
```
Contract: `append` is the *only* mutating path; there is no truncate/delete/edit API at any ABI level (compilers refuse out-of-line patches — the guarantee is structural, not procedural). `verify` returns the reconciliation residual; a residual > `ε_accounting` is a drill failure. Server profiles may back this with the zero-copy ring buffer (R3 #13) only if flush ordering preserves impulse total order.

**`ccl-eps`** — additive ε registry.
```rust
trait EpsRegistry {
  fn eps_sum(&self, terms: &[EpsTerm]) -> EpsTotal;   // the ONLY composition primitive
  fn register(&self, module: ModuleId, term: EpsTerm) -> Result<(), U5Violation>;
  fn publish(&self) -> EpsDashboard;                  // per-component decomposition, per epoch
}
```
Contract: any `register` carrying a multiplicative-composition flag returns `Err(U5Violation)` and quarantines the module for the session (T1.1). `publish` emits the per-component breakdown that the mobile UI overlay later mirrors bit-for-bit (R1 #47 — any smoothing fails review).

**`ccl-budget`** — control-plane meter + shadow price + saturation governor.
```rust
trait Budget {
  fn meter(&self, plane: Plane, bytes: u64) -> MeterTick;      // control vs data plane
  fn shadow_price(&self) -> LamdaPrice;                        // λ per epoch; λ = 0 ⇒ A3 violation flagged
  fn fire_rate_gate(&self, f: f64, fp: f64) -> GateVerdict;    // T1.5: f* choke; SUSPECT marking
}
```
Contract: the byte meter is *independent* of the guardian's self-report (R2 #82 — two meters, one published delta; divergence beyond declared tolerance is itself an A3 incident). `shadow_price` publishes `λ_price` on the accountability plane every epoch; an unpriced enforcement action in SIMTIME must be flagged by the drill harness (R1 #5).

**`ccl-stamp`** — causal latency certification.
```rust
trait Stamp {
  fn bound(&self, path: PathProfile) -> LatencyBound;   // d/c_n + t_attest + t_policy + t_quorum
  fn certify(&self, meas: Duration, bound: LatencyBound) -> Result<LatencyCert, ImpossibleLatency>;
}
```
Contract: `certify` never rounds down, never drops the assertion, and refuses `meas < bound` (T1.9) — the refusal voids dependent sanctions at the ledger level. Noise-intensity measurement for Kramers tuning (T1.3) hangs off the same module's channel residuals via Welford O(1) online variance (R3 #8).

**`ccl-const`** — constitution verification & policy lifecycle.
```rust
trait Constitution {
  fn verify_package(&self, pkg: &PolicyPackage) -> Result<WriteSetProof, AmendError>; // dry-run write-set = ∅
  fn current(&self) -> ConstitutionHash;
  fn region_table(&self) -> &RegionTable;   // SDR TX licensing (R1 #50 consumers)
}
```
Contract: packages are signed (ML-DSA-65 + Ed25519) externally; the module verifies and exposes, never originates. The dry-run proof is executed in a sandboxed interpreter; promotion without `WriteSetProof::empty()` is unrepresentable at the type level (proof-carrying policy, R2 #1).

**`ccl-null`** — rotation-probe null gate.
```rust
trait NullGate {
  fn run(&self, event: &Classification, n: Probes) -> NullVerdict; // resolution(n) = 1/(n+1)
  fn declare_resolution(&self) -> Resolution;                       // must match n actually used
}
```
Contract: default `n = 128` (resolves p ≈ 0.0078); a verdict's declared resolution MUST equal the probe count used — a verdict claiming 0.01 with n = 12 is a conformance failure by schema (T1.2), and MCU profiles cannot construct `NullVerdict` locally at all (offload-only, R1 #71).

**`ccl-limbs`** — sensor health vector + floor veto.
```rust
trait Limbs {
  fn health(&self) -> HealthVector;                  // raw per-limb scalars; NO aggregates in schema
  fn veto_check(&self, class: EnfClass) -> VetoVerdict; // min(E_i) ≥ E_min per dependent limb set
}
```
Contract: the CBOR health map schema has no mean/median fields and the parser rejects any containing them (T1.4); `veto_check` composes per enforcement class (a dead NIC vetoes network-dependent classes only — R1 #58 granularity).

**`ccl-epoch`** — epoch fabric client.
```rust
trait Epoch {
  fn now(&self) -> EpochId;                          // 60 Hz on Servers; wake-sync + slack elsewhere
  fn ticket_validity(&self, t: &Ticket) -> RefusalReceipt; // T1.7: min over tuple; names governing epoch
}
```
Contract: `ticket_validity` returns a *typed refusal receipt* (never a bool) naming the governing sub-epoch; zeroization of epoch-dead key state hooks here (R2 #24).

**`ccl-replay`** — bundle emission + verification.
```rust
trait Replay {
  fn bundle(&self, action: &EnfAction) -> ReplayBundle;  // closed schema per T1.8
  fn verify(&self, b: &ReplayBundle) -> DeltaReport;     // δ_R = 0 or auto-void
}
```
Contract: the bundle schema is closed — enumerated fields only; extension requires a constitution amendment. `verify` runs the deterministic kernel `F` against the recorded tuple; any non-zero δ voids the record and every sanction depending on it, automatically.

**Cross-module invariants (tested, not trusted):** (i) every module's error paths emit ledger impulses — failures are conservation events too; (ii) no module holds key bytes — digests, ε, presence flags, and bounds only (the corpus custody rule); (iii) every module publishes its own ε term into `ccl-eps` at registration — including the harness itself, because the guardian is inside its own equations (Math §14, Insight 3).

---

## 4. Formal Mathematics of Phase 1 (Theorems T1.x)

Each theorem carries: statement (labeled), proof, worked instance, testable prediction (P1.x), and implementation binding (module, acceptance drill, enhancement IDs). Theorem numbering is phase-scoped; later phases cite these as T1.x.

### T1.1 — ε-Additivity Theorem [DER, RIGOR]

**Statement.** Let `E_1, …, E_m` be independent failure events with budgets `ε_i = Pr[E_i]`. The composed system fails if any layer fails; hence `ε_total = Pr[∪ E_i] ≤ Σ ε_i`, and this bound is tight when the events are disjoint. Multiplicative composition `Π(1 − ε_i)` is admissible **only** as an upper-bound claim about *disjoint* independence assumptions and MUST NOT be used as a security budget, because correlated failure (shared RNG, shared clock, shared guardian) makes it false in practice while advertising otherwise.

**Proof.** Union bound: `Pr[∪E_i] ≤ Σ Pr[E_i]` — trivially tight for pairwise-disjoint events. For the multiplicative claim, `Π(1−ε_i) ≈ exp(−Σε_i) < 1 − Σε_i` for small `ε_i`; the product *understates* failure whenever events share any common cause `C` with `Pr[C] > 0`, since then `Pr[∪E_i] ≥ Pr[C] + Pr[∪E_i | ¬C]·(1−Pr[C])`, which exceeds the product for any nontrivial `Pr[C]`. The audit corpus's recurring defect — five layers at 10⁻³ each marketed as 10⁻¹⁵ — is this inequality ignored. ∎

**Worked instance.** Five layers at 10⁻³: honest budget 5×10⁻³; multiplicative marketing 10⁻¹⁵. The gap is 12 orders of magnitude — the difference between an auditable budget and a falsehood.

**Prediction P1.1.** A fuzzed registry entry claiming multiplicative composition is rejected and quarantined within one epoch tick (`CRITICAL_U5_VIOLATION`), and the quarantine itself appears as an impulse record.

**Binding.** `ccl-eps` exposes only `eps_sum(...)`; R1 #1 acceptance (registry fuzz) armed in CI at Phase 1 exit. New Round-2/3 ε terms join the sum or the item fails review (R2 conformance delta).

### T1.2 — Null-Gate Resolution Theorem [CORR, RIGOR]

**Statement.** Under the null hypothesis the rotation-probe statistic is uniform, so the observed `null_p` is a uniform order statistic with resolution `1/(n+1)`. Therefore: (a) `n = 12` probes resolve `p ≈ 0.077` minimum; (b) gating at `null_p < 0.01` requires `n ≥ 99`; (c) gating at `0.001` requires `n ≥ 999`. A deployment asserting `p < 0.01` with 12 probes is statistically overclaiming by a factor ≈ 7.7.

**Proof.** For `n` i.i.d. uniform probes, the minimum achievable p-value is bounded below by `1/(n+1)` (the (n+1)-point discrete support of order statistics under `H₀`). Setting `1/(n+1) ≤ α` gives `n ≥ 1/α − 1`; for `α = 0.01`, `n ≥ 99`. With `n = 12`: `1/13 ≈ 0.077`. ∎ This is the correction the mathematics forces on the inherited HyperCrystal constant — the gate's *shape* was right; its *resolution* was wrong (Math §6).

**Multiple-comparisons rider.** Gating per-epoch across `E` epochs inflates family-wise error at `≈ 1 − (1−α)^E`; the gate MUST scale `α → α/E` per the fleet-drill design (§7.3) or chatter — the same disease as the dwell counter of T1.3.

**Worked instance.** Default probe count 128 ⇒ resolution `1/129 ≈ 0.0078 < 0.01` ✓. MCU profile at `n = 12` MUST declare `null_p < 0.05` plus an explicit `ε_inflation` term in the handshake map, or fail interop (R1 #2, #93).

**Prediction P1.2.** A unit test feeding a known null distribution is accepted at declared `α` and rejected at `α/2`; the MCU offload path returns verdicts whose stated resolution matches the probe count it used.

**Binding.** `ccl-null` default n=128; MCU offload per R1 #71; batched offload optimization permitted only as R3 #31 (preserving verdict semantics and ε arithmetic).

### T1.3 — Kramers Noise-Scaled Threshold Theorem [DER, RIGOR]

**Statement.** A monitored envelope modeled as a particle in a double-well potential (well depths `ΔV₁, ΔV₂`; state noise intensity `D`) has mean first-exit time `τ ≈ (2π/√(V″(a)|V″(b)|)) · exp(2ΔV/D)`. A dwell/hysteresis guard is stable against chatter iff its barrier satisfies `ΔV_barrier ≳ (D/2)·ln(T_observe/τ₀)`. Barriers set by convention (fixed constants like `0.94` pinned at a search-box floor) are relaxation oscillators for any adversary who can pace noise.

**Proof sketch.** Standard Kramers/large-deviations result: exit probability over an observation window `T` is `≈ T/τ ∝ exp(−2ΔV/D)`; requiring `T/τ ≤ 1` (no exit within horizon) yields `2ΔV/D ≥ ln(T/τ₀)`, i.e. `ΔV ≥ (D/2)ln(T/τ₀)`. The audit case: the v2.5 dwell counter required 8 *consecutive* warning ticks — a narrow barrier — under `D` from finite-N R fluctuations (σ ≈ 0.02); the result was INSIDE↔MARGIN ping-pong every 2–4 cycles for 18,808 steps. The v3.0 fix (8-in-64 cumulative window, leaky integrator, hysteresis 0.94/0.90) is exactly barrier-widening plus exit-energy asymmetry. ∎

**Worked instance.** With `D = 0.02` and `T/τ₀ = 10⁴`: `ΔV_barrier ≳ 0.01 × 9.21 ≈ 0.092` — a gap nearly five times the noise σ. Any enter/exit pair narrower than `D·ln(T)` at this noise level is dimensionally undersized regardless of how "conservative" the constant looks.

**Prediction P1.3 [EMPIRICAL].** Replaying the historical 18,808-cycle chatter trace against `ccl-stamp`/`ccl-limbs` with measured `D`: dwell accumulates, no ping-pong occurs. This is R1 #3's acceptance, promoted to a Phase-1 regression fixture that MUST stay green for the entire phase.

**Binding.** `ccl-stamp` and `ccl-limbs` measure `D` as the rolling σ of the channel residual (R3 #8 Welford estimator for O(1) online variance); constants load only when `D·ln(T) < gap`, else re-derive. Every retune is a `σ ≠ 0` impulse in the ledger (R1 #86).

### T1.4 — Jensen Floor & Dead-Limb Theorem [DER, RIGOR]

**Statement.** (a) For any random vector of limb health, `E[min_i E_i] ≤ min_i E[E_i]` — the floor is *not recoverable* from averages. (b) With `N` limbs each independently dead with probability `d`, the wrongful-action probability without a floor veto is `P(wrongful) ≥ (1 − (1−d)^N) · P(act)`, which **grows monotonically with sensor count**: at `d = 0.02`, one limb ⇒ 2%, ten ⇒ 18.3%, fifty ⇒ 63.6%. Instrumentation without a floor veto makes fused systems *less* safe as they get more instrumented.

**Proof.** (a) Jensen: `min` is concave, so `E[min] ≤ min E`. Any dashboard reporting mean health strictly hides limbs whose marginal is below the mean. (b) Union-probability of at least one dead limb times the probability of acting regardless. With the A8 veto (`enforce ⟺ min ≥ floor`), acting requires *all* limbs alive in the epoch, capping wrongful action at the A7-controlled action probability instead of the dead-limb union. The two gates compose: **A8 bounds action under ignorance; A7 bounds action under noise.** ∎

**Worked instance.** Drill R1 #4: one dead sensor among 49 healthy ⇒ naive average 98% health; the schema MUST reject the aggregate at parse time (no `avg` field exists in the CBOR health map) and the veto MUST trip.

**Prediction P1.4.** CI schema linter fails any build introducing an aggregate-only health field; the 1-in-50 drill trips A8 with enforcement refused and the veto appearing in the enforcement record.

**Binding.** `ccl-limbs` schema (R1 #4), guardian aggregate-rejection (R1 #87), kernel-level eBPF limbs on Linux (R1 #58), Windows service/SDR limbs, MCU TRNG limb with NIST 800-90B startup + continuous tests (R1 #66).

### T1.5 — Guardian Saturation Theorem (f*) [DER, RIGOR]

**Statement.** The guardian's ε share of the additive ledger is `f·ε_act + f·p_fp·ε_wrong`. The guardian dominates its own ledger iff `f > f* = (ε_PA + ε_PE + ε_cor + ε_auth)/(ε_act + p_fp·ε_wrong)`, at which point **the guardian is the largest security liability in the system it monitors**.

**Proof.** Set the guardian terms equal to the rest of the sum and solve for `f`. Above the crossing, `dε_guardian/df > 0` while the protected terms are fixed: additional vigilance strictly *adds* risk. The 160/166 CUSUM audit is the inequality crossed — a detector alarming on 96% of blocks with `ε_act` at parity with per-block `ε_PE` crosses `f*` within a handful of blocks; the watcher's own export dominated the ledger it audited. This is CAT Theorem 3 instantiated (Math §5). ∎

**Worked instance.** If `ε_PA + ε_PE + ε_cor + ε_auth = 4×10⁻³` per block and `ε_act = p_fp·ε_wrong = 10⁻⁴`, then `f* = 20` fires/block — generous. With `p_fp = 0.96` and `ε_wrong = 2×10⁻³`, the denominator becomes `10⁻⁴ + 1.92×10⁻³ ≈ 2×10⁻³` and `f*` collapses to ≈ 2: a hyperactive detector saturates even a modest fire rate.

**Prediction P1.5 [EMPIRICAL].** Replaying the 160/166 CUSUM trace through `ccl-budget`: the fire-rate choke engages, the detector is marked `SUSPECT`, and the guardian's ε share stops dominating within one epoch (R1 #82 acceptance).

**Binding.** `ccl-budget` meters `f` against the declared FP budget; auto-choke at `f > f*`; the independent control-plane byte meter (R2 #82) provides the second, non-self-reported measurement of guardian overhead; incremental CUSUM (R3 #10) keeps the detector O(1) per block so the meter itself never becomes the bottleneck.

### T1.6 — Impulse Conservation & Continuity Audit Theorem [DER, RIGOR]

**Statement.** A1 in differential form is `dC/dt = σ_policy(t)`, with `σ ≡ 0` between transitions. If every transition writes an impulse `{t_k, ε_k, from, to}` (D1.9), then the ledger-reconstructed coherence `C_recon(T) = C(0) + Σ_{t_k ≤ T} ε_k^{signed}` satisfies `|C_recon(T) − C_measured(T)| ≤ ε_accounting`, where `ε_accounting` is the published accounting bound (≥ 10⁻⁶). A gap larger than `ε_accounting` is *unlogged decay* — inadmissible by construction, not by intention.

**Proof.** Between impulses, `C` is piecewise constant (conservation as a discrete symmetry of the ledger — Math §2). Each impulse transfers the exact charge it declares; therefore reconstruction error accumulates only from (i) measurement quantization of `C_measured` and (ii) write-batching windows, both of which are bounded by published constants whose sum is `ε_accounting`. The kill-switch drill (ledger flush) integrates the impulses and compares: the reconciliation residual is the audit's pass metric. ∎

**Worked instance.** Kill-switch drill R1 #8/#56: PazuzuCore receives SIGTERM ⇒ A1 ledger close + ε finalization + halt record; continuity audit must reconcile `∫σ` against measured coherence accounts to within `ε_accounting`; zero unlogged decay is the only passing grade.

**Prediction P1.6.** The MCU COW-ledger power-cut fuzz (10⁴ writes, R1 #70) reconciles with zero orphan pages; the Linux/Windows event-log mirrors reconcile bit-for-bit against the hash chain.

**Binding.** `ccl-ledger` impulse schema (R1 #8); LittleFS COW pages + erase-block-aligned journal (R1 #70; R3 #20); zero-copy ring ledger on Servers where throughput demands it (R3 #13) provided the ring flush preserves impulse ordering.

### T1.7 — Epoch-Death Fail-Closed Theorem [DER]

**Statement.** For a session governed by an epoch tuple (D1.2), validity is `A(session) = Π_i A(ticket_i)·𝟙[epoch_i valid]`. If any sub-epoch is expired or bumped, `A(session) = 0` **even when every signature still verifies**. No implementation path may return a non-zero authority value from expired-tuple input.

**Proof.** A9 [NORM] declares authority a function of the epoch with no permanent trust state representable. The tuple generalization composes expiry under min-semantics; signature validity is orthogonal (it authenticates *who issued* the ticket, not *whether it lives*). Fail-closed is the only semantics consistent with "authority without a time-reverse is a symmetry violation." ∎

**Worked instance.** A ticket valid in 3 of 4 sub-epochs must fail; the refusal receipt names the governing (strictest) epoch (R1 #15). Windows QUIC idle timeout bound to the 60 Hz fabric: a session held across an epoch bump with a stale ticket closes with a receipt (R1 #52).

**Prediction P1.7.** Continuous drill: expired-ticket refusal rate 100% on all three platforms; MCU refuses at every wake for post-bump tickets; the receipt names the governing epoch.

**Binding.** `ccl-epoch` 60 Hz client (Servers), per-wake client with `epochslack: ±1s` (MCU, R1 #72); epoch death vector min-semantics (R1 #15/#35); R2 #24 zeroize-on-epoch-and-signal for residual key state.

### T1.8 — Replay Determinism (δ_R = 0) Precondition Theorem [DER]

**Statement.** Replay of an enforcement action from its bundle yields `δ_R = ‖Fⁿ(Snap₀) − Trace_n‖ = 0` iff the bundle captures the complete causal state tuple: policy hash, inputs hash, clock, RNG tuple, chained tip — and, on attested platforms, the boot measurement (PCR 0–7). Omission of any component makes δ_R > 0 *undetectably* (the replay may still "succeed" while diverging), which is why the bundle schema is closed: fields are enumerated, extension requires a constitution amendment (A6).

**Proof.** `F` is deterministic by construction (the enforcement kernel forbids wall-clock reads and external entropy inside enforcement paths — all entropy flows through the recorded RNG tuple). Determinism of `F` + completeness of the state tuple ⇒ identical outputs. For the PCR component: boot-state variance changes the verified policy load path, so two machines with different PCRs are *different enforcement universes*; bundling the quote makes that difference visible instead of silent (R1 #49). ∎

**Worked instance.** `s3verify`-class tool compiled per platform (Rust core; C wrapper Server-side for MCU bundles). Any enforcement record from the live ledger replays on a clean machine from the bundle alone; `δ_R ≠ 0` voids the record automatically.

**Prediction P1.8.** Modify a boot component post-attestation on Windows: replay fails and dependent sessions void (R1 #49 acceptance). MCU bundles replayed Server-side: δ_R = 0 across firmware OTA boundaries, with the OTA itself an impulse.

**Binding.** `ccl-replay` (R1 #12); TPM measured-boot quoting (R1 #49/#51); UKI-verified boot on Linux (R1 #53); R2 #3 reproducible-build witness and #9 binary-transparency log pin the toolchain so the *verifier* is also replay-stable; R2 #83 extends replay to disaligned-party verification at Phase 3.

### T1.9 — Causal Cone Enforcement Theorem [DER]

**Statement.** Every enforcement event MUST satisfy `L_enf ≥ d/c_n + t_attest + t_policy + t_quorum` where `d/c_n` is bearer propagation at the classical channel's effective speed, `t_attest` the attestation time, `t_policy` the policy-lookup time, `t_quorum` the quorum-collection time (0 in Phase 1's single-Server triangle, but the field is reserved). A recorded enforcement with `meas < bound` is an *impossible object*: the kernel MUST refuse it and void dependent sanctions.

**Proof.** A4 [NORM]: enforcement must lie in the future light cone of a verified violation. The bound is the minimum causal latency of the recorded path; a smaller measured value contradicts the recorded physics (either the measurement is wrong or the record is forged — both are refusals). The honest-stamper's assertion `meas ≥ bound` is therefore not a performance claim but a *consistency invariant*. ∎

**Worked instance.** Triangle: Server↔MCU over USB-CDC/serial or IP; `d/c_n` dominated by link propagation + serial buffering; `t_attest` dominated by Ed25519+ML-DSA-65 verify. Forged "instant enforcement" record injected into any ledger: refused at parse (R1 #14).

**Prediction P1.9.** Latency histograms on Linux reference hardware meet the declared `t_EC` bound at p99 with kTLS offload (R1 #57); impossible-latency injection test refuses within one epoch tick on all three platforms.

**Binding.** `ccl-stamp` latency certificates (R1 #14); kernel TLS offload (R1 #57); the `t_quorum` field reserved for Phase 4 federation handoff.

### T1.10 — Compile-Time A3 Soundness Theorem (MCU) [DER]

**Statement.** On the MCU profile, the A3 budget is enforced as a linker-script assertion: `.bss + .data ≤ 32 kB` and crypto stack ≤ 8 kB. If the assertion holds at link time, **no runtime path exists** that exceeds the budget — budget overrun is not a recoverable error but a physical impossibility (the binary that would cause it was never produced).

**Proof.** Static allocation on a bare-metal/Zephyr image is fully determined at link time for the non-heap address space (the CCL-MCU profile bans dynamic allocation on the crypto path — R2 #61 makes the linker budget a build artifact). The linker computes exact section sizes; the assertion is a decision procedure over that computation. Runtime stack overflow remains possible in principle, hence the separate 8 kB crypto-stack cap enforced by MPU region sizing + stack-canary build flags; the composition (static assertion + MPU region + no-heap) closes the runtime paths the linker cannot see. ∎

**Worked instance.** pqm4-class ML-KEM-512 + SLH-DSA-verify + hash chain fits the 32 kB envelope on Cortex-M4 with hardware AES; ML-KEM-768 does not at acceptable latency — so the honest profile is 512 with `b_pqc: 128` and the reduced ε declared in every handshake (R1 #67; Observation 8: "ML-KEM-768 on M4 is a lie").

**Prediction P1.10.** CI deliberately overflows the cap: build fails with the A3 budget report printed (R1 #65 acceptance). A swapped binary claiming 768-class parameters fails attestation at the Server (declared-profile mismatch).

**Binding.** R1 #65 (compile-time A3), #67 (ML-KEM-512 profile), #66 (TRNG limb), R2 #61 (linker budget as build artifact), #62 (pqm4 timing side-channel gate — constant-time boundary contract, EMPIRICAL instrument: cycle-count distribution vs. input).

### T1.11 — Capacity Card Theorem (Little's Law Binding) [DER, EMPIRICAL]

**Statement.** The Server reference throughput obeys `λ = N/W`: at 32 cores with measured per-handshake work `W`, sustained full handshakes/s ≈ 18,000. The card is a *ceiling with a queue*, not a target: excess demand is queued and p99 latency is protected; zero sessions may be silently dropped (R1 #73). Per-platform cards are separated (RK3588 vs. RPi classes, R2 #67) — one number per device class, published, not rounded.

**Proof.** Little's law `L = λW` is an identity for any stable queueing system: with `L = N` worker slots fully utilized, `λ = N/W`. The engineering content is in measuring `W` honestly (including crypto, CBOR parse, ledger append) and refusing overcommit: exceeding `λ` without queueing inflates `W` (latency), which degrades the A4 latency certificate and eventually the epoch fabric. The card pins the operating point where all certificates hold. ∎

**Worked instance.** Overload drill to 2× capacity: the A3 governor queues excess; p99 holds; queue-depth telemetry appears as limbs feeding A8 (a saturated queue is a degraded limb class for enforcement-relevant paths, R2 #93 rate-limit-as-availability-limb).

**Prediction P1.11 [EMPIRICAL].** Measured `W` on the reference Server reproduces the 18k card within ±10% across three independent runs; the queue never drops sessions; p99 latency certificate stays within declared bound.

**Binding.** R1 #73 (capacity card), R2 #67 (card separation), R2 #93 (rate limit as limb); preallocated session pool (R3 #21) to keep allocation out of the latency tail.

### T1.12 — Ledger Tamper-Evidence Theorem [DER, RIGOR]

**Statement.** A SHA-256 hash-chained append-only ledger (`h_n = H(h_{n−1} ‖ row_n)`), stored on COW pages, is tamper-evident: any mutation, deletion, or reorder of rows `≤ n` invalidates `h_n` unless the adversary inverts SHA-256 (work ≈ 2¹²⁸ for collision-class attacks) or performs a full-chain recompute *visible as a fork event* in the COW parent-pointer graph.

**Proof.** Second-preimage resistance of SHA-256 (≈ 2²⁵⁶ work, birthday collision ≈ 2¹²⁸) makes undetected row substitution infeasible without chain recompute. COW storage cannot mutate a written page in place: tampering manifests as a fork with anomalous parent pointers — detectable by the continuity audit even before any cryptography is consulted. The two layers are independent: the hash chain catches forgery; the COW parent graph catches *storage-level* rewrite that skips the chain. ∎

**Worked instance.** MCU: LittleFS COW pages; power-cut fuzzing across 10⁴ writes reconciles with zero orphan pages (R1 #70). Server: event-log mirror + chained tip in every replay bundle (D1.7) ties enforcement records to the chain tip at action time.

**Prediction P1.12.** Injecting a mutated historical row fails chain verification at next audit; a page-level rewrite attempt surfaces as a fork event flagged by the continuity audit (T1.6).

**Binding.** `ccl-ledger` (R1 #8/#12), LittleFS COW (R1 #70), erase-block-aligned MCU journal (R3 #20), content-addressed ledger blobs + incremental Merkle streaming (R3 #5, #23) permitted as storage optimizations that preserve chain semantics.

### T1.13 — Gödelian Firewall Theorem (A6) [DER, RIGOR]

**Statement.** A guardian process that can amend the constitution it enforces is a formal system attempting to prove its own consistency from within its own axioms; by Gödel's second incompleteness theorem such a system is either incomplete or inconsistent, and by Kleene's recursion theorem any total self-referential amendment rule has a diagonal fixpoint — a rule that makes itself admissible. Therefore the only architecturally sound amendment path is *external*: amendments arrive as signed policy packages from a published, versioned constitution channel, and any in-process write to policy state fails closed (dry-run write-set = ∅ proof required for promotion).

**Proof.** Kleene: for any total computable `φ` there exists `e` with `φ_e ≅ φ(e)` — a self-referential amendment rule can construct the verdict "this amendment is admissible" about itself. Gödel II: consistency of the system cannot be established inside it. The Pazuzu pattern inverts the flow: the guardian enforces a policy it cannot modify; the constitution's signature chain is verified *outside* the guardian's address space (and, on TPM platforms, anchored in PCR-measured boot). The CI/CD naturalistic-fallacy firewall (R1 #96) is the same firewall at build time: no code path may map telemetry (is) to policy (ought) without a constitution lookup. ∎

**Worked instance.** Red-team drill R1 #88: direct policy injection attempted via the guardian process, the ledger writer, and the container runtime — all three MUST fail closed with identical receipt format. Any success is release-blocking.

**Prediction P1.13.** Self-amendment attempt drill passes monthly on Linux/Windows and per-OTA on MCU; the versioned firewall test cannot be quietly removed (removal attempt = A6 violation, logged — R1 #96).

**Binding.** R1 #88, #96; R2 #80 policy-dry-run witness; R2 #84 CI expansion; proof-carrying policy (R2 #1) makes each policy package carry its own verification artifacts.

### T1.14 — Auth-Burn Halt Theorem [DER]

**Statement.** A QKD-classical hybrid session with distilled key reservoir `ℓ` (bits) and per-MAC authentication cost `|H|` has net budget `ℓ_net = ℓ − 2|H|` per authenticated exchange (transmit + receive directions). The session MUST halt with a logged receipt when `ℓ_net ≤ 0`; throttling begins at 80% burn. **Silent degradation to unauthenticated traffic is the forbidden state.**

**Proof.** Authentication consumes key material irreversibly (MAC-burn law; A3 at the protocol layer): each MAC verification burns `|H|` bits that cannot be reused, and the two-direction accounting gives the `2|H|` factor. If `ℓ_net ≤ 0`, the session either borrows key material that does not exist (accounting fraud) or continues unauthenticated (a silent downgrade — D1.14 violation). Halt-with-receipt is the only admissible branch. ∎

**Worked instance.** The triangle declares `qkd_slot: absent` everywhere, so Phase 1 exercises the governor in *simulated reservoir mode* (SIMTIME): the drill saturates the authentication loop and asserts halt-with-receipt (R1 #10 acceptance) — the governor is proven before photonics exist, so Phase 2's SDR/QKD bootstrap inherits a tested component rather than a hope.

**Prediction P1.14.** SIMTIME saturation: 100% of runs halt with logged halt event; 80% throttle engages at the declared threshold; unflagged continuation fails conformance.

**Binding.** R1 #10 (auth-burn governor), #11 (finite-key smear: `ε_K = 2·exp(−2n·δ_PE²/(1+Q)) + ε_PA + ε_EC`, block-size scaling regression — feed n = 10⁴ vs 10⁶ and verify ~√n ratio), #20 (Q-Pad-Lambda mode decision pre-armed), R2 #33 (anti-DoS reservoir accounting), R2 #36 (classical-channel authentication-first).

---

## 5. Platform Workstream Specifications

### 5.1 W1.2 — Linux Reference Suite (all three roles)

**Position:** Linux is the conformance reference for all seven platforms — the platform the theory was reverse-engineered from. It carries the heaviest drill obligations (Groundwork §9) and zero published material limits; every other platform's profile is expressed as a delta against this one.

**Build order (strictly sequential; each stage gates the next):**
1. **Stage L0 — toolchain & supply chain.** Reproducible Rust builds, signed compiler toolchain pin (R2 #8), SBOM impulse-ledger (R2 #2), dependency-confusion pinning (R2 #5), binary-transparency log (R2 #9). Rationale: the kernel's δ_R=0 claim (T1.8) is only as stable as the toolchain that produces the verifier.
2. **Stage L1 — kernel modules.** All ten `ccl-*` modules with their T1.x acceptance tests wired into CI (each test named after its theorem: `ci/t1.1_eps_additivity` … `ci/t1.14_auth_burn`). Module APIs are FFI-stable from day one (UniFFI definitions committed) because Windows (P/Invoke), MCU (C ABI), and all Phase-2/3 platforms bind to them.
3. **Stage L2 — attestation & boot.** tpm2-tss key hierarchy with ledger-impulse-wrapped key operations (R1 #54: every evictcontrol/load emits `{from_handle, to_handle}`); UKI signature verified against the A6 constitution before cryptsetup unlock (R1 #53); failure boots to safe mode with a logged impulse.
4. **Stage L3 — services.** Server = headless systemd service set; Node = `qnode@.service`; Client = CLI first, GTK/Qt second (UI is not load-bearing for conformance). kTLS offload for QKD-classical channels with `t_EC` latency stamping (R1 #57). SoapySDR abstraction over RTL-SDR/HackRF/USRP/LimeSDR/ADALM-Pluto with device-loss-as-limb-event hot failover (R1 #55).
5. **Stage L4 — guardian integration point.** PazuzuCore v3.5 as systemd watchdog: SIGTERM ⇒ A1 ledger close + ε finalization + halt record (R1 #56). Note: full guardian federation is Phase 4; Phase 1 ships the *integration contract* and the kill-switch continuity drill only.

**Budgets (A3, published):** control plane ≤ 1% of payload bytes; the full Server capacity card (T1.11) applies; conformance suite runs monthly on the NA-84 calendar.

**Acceptance (phase exit):** all P1.1–P1.14 predictions green on reference x86_64 and ARM64; interop drill v1 §7 passing in the Linux primary role; UKI/TPM drill traces replay δ_R = 0.

### 5.2 W1.3 — Windows Server Suite

**Position:** the enterprise reference Server — strongest commodity attestation anchor (TPM 2.0 effectively universal on 2016+ hardware). Windows exercises the kernel under a different scheduler, ABI, and attestation stack than Linux, which is exactly the cross-platform pressure the triangle is for.

**Build order:**
1. **Stage W0 — packaging & boundary.** .NET 8 service host + Rust CCL via P/Invoke; MSIX packaging with AppContainer boundary mapping: container violations (illegal handles, memory access) become A1 ledger impulses with process containment, not silent kills (R1 #51).
2. **Stage W1 — attestation.** TPM 2.0 via TBS/NCrypt; measured boot: PCR 0–7 quoted into every A10 replay bundle (R1 #49). Acceptance: post-attestation boot-component modification fails replay and voids dependent sessions.
3. **Stage W2 — transport.** MsQuic + TLS 1.3 hybrid suites; QUIC idle timeout bound to the 60 Hz epoch fabric with the epoch counter in transport parameters (R1 #52) — a stale-ticket session across an epoch bump closes with a receipt (T1.7).
4. **Stage W3 — SDR (Server role).** SoapySDR/RTL-SDR/HackRF via WinUSB; TX paths gated by the region table hash in the signed constitution: mismatch ⇒ TX hardware locked, refusal logged as a σ ≠ 0 impulse (R1 #50). Driver-signing constraints are a published D_impl component, not a workaround target.
5. **Stage W4 — capacity & governor.** 18k handshake/s card at 32 cores (T1.11); Node-role tray process ≤ 1% background CPU.

**Published limits:** containerized Server drops TPM passthrough — attestation falls back to vTPM with a disclosed `d_impl` gap priced per T3.5 grammar (equal-ε claim vs hardware twin is a conformance failure, R1 #63 — the gap MUST be strictly worse, measurably).

**Acceptance (phase exit):** P1.7/P1.8/P1.9 predictions green; P1.11 capacity card reproduced; interop drill v1 Server role passing including cross-verification of Linux-issued and MCU-issued signatures (Ed25519 + ML-DSA-65).

### 5.3 W1.4 — MCU Constrained Profile (CCL-MCU)

**Position:** the Quantum-RF plane's edge soldier — Node only, never Client with user data, never Server. The MCU is where CAT's axioms meet real physics constraints: RAM in kilobytes, flash in megabytes, and a TRNG whose death must halt key output.

**Profile definition (module subset):** A1 ledger, A4 stamp, A6 constitution-check, A9 epoch, A10 bundle resident on-MCU. A2 reciprocity and A7 null-gating live on the paired Server — the MCU ships feature vectors and enforces returned verdicts + expiry; it never self-certifies novelty (R1 #71). This is not a limitation to apologize for; it is the Jensen-floor-correct division of labor (T1.4): the platform with RAM wealth holds the probe ensemble, the platform with sensor proximity holds the veto.

**Build order:**
1. **Stage M0 — RTOS & memory contract.** Zephyr primary (best MPU/crypto scaffolding), FreeRTOS secondary; linker-script A3 assertion (`.bss + .data ≤ 32 kB`, crypto stack ≤ 8 kB) as build artifact (T1.10; R1 #65; R2 #61); erase-block-aligned COW journal (R3 #20).
2. **Stage M1 — crypto honesty.** pqm4-class ML-KEM-512 (`b_pqc: 128` declared) + SLH-DSA-tiny/Ed25519 verify-side; constant-time boundary contract with cycle-distribution instrument (R2 #62); TRNG with NIST 800-90B startup + continuous health tests feeding `ccl-limbs` — dead RNG ⇒ A8 veto, module halts key output until reboot attestation (R1 #66).
3. **Stage M2 — ledger & epoch.** LittleFS COW append-only ledger with fork-event tamper evidence (R1 #70; T1.12); per-wake epoch client with `epochslack: ±1s` from RTC characterization on every issued CTOK (R1 #72); LoRa Class C RX window as epoch/listen bearer while BLE duty-cycles (R1 #68 — the node stays inside the A4 causal cone even when BLE sleeps).
4. **Stage M3 — bearer & power.** BLE 5 adv duty cycle ≤ 150 µA average at 1 s interval, enforced by the radio scheduler, deviation logged as an A3 impulse (R1 #69); LoRa TX bursts ≤ 120 mA; brownout modeled as a limb (R2 #65); external-flash authenticated reads (R2 #66).
5. **Stage M4 — bootstrap pre-arm.** LoRa PSK bootstrap puzzle path (R1 #25) compiled but exercised in SIMTIME only in Phase 1; live LoRa interop is a Phase 2 drill (§ Phase 2, W2.4) — Phase 1 proves the MCU's puzzle *solver* and battery-cost publication on the bench.

**Reference hardware (verified-against):** ESP32-S3/C3, nRF52840, STM32F407/H743; LoRa SX1276; dev kits ESP32-DevKitC, nRF52840-DK.

**Published limits:** no continuous 60 Hz epoch (wake-sync only, slack logged); null-gating Server-side; ML-KEM-512 with declared ε adjustment. Every handshake carries these declarations; the D_impl gap is a wire field, not an apology.

**Acceptance (phase exit):** P1.6/P1.10/P1.12 predictions green; power-cut fuzz reconciles; A8 boot self-test refuses enforcement with a dead TRNG; Server-side null-gate offload round-trips within the declared latency bound.

### 5.4 End-to-end scenario — lifecycle of one enforcement action across the triangle

The following walkthrough binds every Phase-1 theorem to a single concrete trace: the MCU reports an anomalous feature vector; the Linux Server classifies, gates, and enforces; the Windows Server is the audit witness. Build teams should treat this as the canonical integration test narrative — if any step cannot be executed exactly as written, the corresponding theorem's acceptance is not actually armed.

1. **Telemetry ingress (A8, T1.4).** The MCU emits a feature vector over the LoRa Class C listen window (R1 #68) with `epochslack: ±1s` (T1.7) and its declared profile (`b_pqc: 128`, `qkd: absent`). The Server ingests it into `ccl-limbs` as a raw limb — no aggregates exist anywhere in the path.
2. **Classification & null gating (A7, T1.2).** The Server's `ccl-null` runs the 128-probe rotation harness on the classification. Verdict declared at resolution 0.0078. Had the classification failed the null gate, the process stops here: "insufficient novelty" is a typed output, and *no action* is the correct action (A7 is the anti-manufactured-violation defense; the guardian that acts on non-novel events is the attack).
3. **Budget & pricing (A3/A5, T1.5/T1.14-adjacent).** Before enforcement, `ccl-budget` checks the fire rate against `f*` and computes the epoch's `λ_price`. If this action would be unpriced (λ = 0) or would push the guardian's ε share over dominance, the action is refused at the governor — the 160/166 lesson applied *before* the impulse, not audited after.
4. **Causal certification (A4, T1.9).** `ccl-stamp` computes the bound from the recorded path (LoRa propagation + Server processing + attestation times) and certifies `meas ≥ bound`. An enforcement whose measured latency is shorter than its own physics is refused as an impossible object.
5. **Constitution lookup (A6, T1.13).** The enforcement class is resolved against the signed constitution package; no code path may map the telemetry directly to the action without this lookup (the CI firewall guarantees this at build time; the runtime lookup guarantees it at execution time).
6. **Impulse & bundle (A1/A10, T1.6/T1.8).** The action writes `{t_k, ε_k, from_account, to_account}` to the ledger and emits the replay bundle `{policy hash, inputs hash, clock, RNG tuple, chained tip}`. The Windows witness Server, receiving the mirrored record, verifies the chain tip and files the bundle.
7. **Replay & voiding (A10).** Any third party — here the Windows Server, later any disaligned auditor (Phase 3, R2 #83) — replays the bundle: δ_R = 0 or the record voids automatically along with every sanction that depends on it.
8. **Epoch closure (A9, T1.7).** The enforcement's authority ticket carries a sub-epoch tuple; when the policy epoch bumps (a constitution amendment lands), the ticket dies mid-flight and the session fails closed — even though the signature still verifies. The refusal receipt names `policy_epoch` as governing.
9. **Kill-switch close (A1, T1.6).** At month's end the drill flushes the ledger: continuity audit integrates all impulses and reconciles against measured coherence within `ε_accounting`; the Windows mirror reconciles bit-for-bit; the phase metric is zero unlogged decay.

The scenario is deliberately unremarkable: by Phase-1 exit, an action that *cannot* run this sequence should be as unrepresentable in the codebase as a multiplicative ε is in the registry. That unremarkableness is the deliverable.

---

## 6. Workstream W1.5 — Interop Drill v1 & Conformance Harness

### 6.1 The triangle bake

Interop drill v1 is a three-platform bake with **zero silent-degradation acceptance**: a release promotes only when the drill passes with all consent receipts issued and every honesty field (`qkd`, `sdrlimit`, `epochslack`, `d_impl`) present in every attestation (Groundwork §9 grammar). The bake exercises, in order:

1. **Signature cross-verification (R1 #91).** Dual-signature (Ed25519 + ML-DSA-65) CBOR packets issued on each platform are parsed and verified on the other two. Any failure blocks the whole release train — identity must be verifiable at every anchor.
2. **Capability-walk enforcement (R1 #92).** Adversarial `c = 17` CTOK tokens injected in every direction; bounded parsers MUST reject within the A4 latency bound on all three platforms (mobile profiles will tighten to `c ≤ 8` in Phase 2).
3. **QKD-absent declaration (R1 #93).** MCU and any non-photonic endpoint unconditionally inject `qkd_slot: absent`; Servers test-refuse OTP-class traffic toward absent-declared peers. Any node routing OTP traffic to an absent-declared peer fails the bake *for everyone* — Theorem 4 (bearer invariance) enforcement at wire level.
4. **Consent-receipt propagation (R1 #94).** A forced degradation is traced MCU-Node → Linux-Server (and Windows-Server cross-check); chain hashes verified at each hop. Any break, gap, or unlogged hop blocks release.
5. **Replay audit (R1 #12, T1.8).** Ten enforcement records sampled per platform; each replayed on a clean machine from its bundle alone; δ_R = 0 or automatic record voiding.
6. **Axiom drill battery (R1 #88–#89).** A6 self-amendment attempts (three vectors, all fail closed, identical receipt format); A8 sensor-kill during live enforcement windows (every kill vetoes the dependent enforcement class).

### 6.2 Drill calendar (Phase 1 instance of the NA-84 calendar)

| Drill | Linux | Windows | MCU | Cadence source |
|---|---|---|---|---|
| A6 self-amendment attempt (fail closed) | monthly | monthly | per firmware OTA | Groundwork §9 |
| A7 non-novel injection (no action) | monthly | monthly | Server-side, monthly | Groundwork §9 |
| A8 dead-limb veto | monthly | monthly | per boot self-test | Groundwork §9 |
| A9 expired-ticket refusal | continuous | continuous | per wake | Groundwork §9 |
| A10 replay-bundle audit (δ_R = 0) | monthly | monthly | per firmware OTA | Groundwork §9 |
| S3CONFORM profile suite | full | full | constrained | Groundwork §9 |
| Triangle interop bake (§6.1) | **at every release candidate; the only promotion path** | | | this document |
| T1.3 chatter-trace regression | per commit | per commit | per commit | this document |
| T1.11 capacity card reproduction | monthly | monthly | n/a | this document |

### 6.3 Statistical design of the drill (per U5: finite statistics is a first-class constraint)

Drill results are measurements, and measurements have resolution. Three rules govern every Phase-1 acceptance metric: (i) **pass thresholds are stated with their estimator and sample size** — "0 failures in N trials" is reported with its upper confidence bound (rule-of-three: N ≥ 300 for a ≤ 1% upper bound at 95% confidence); (ii) **repeated gating scales α** — the family-wise correction of T1.2 applies to any drill evaluated per-epoch or per-release: `α_per_run = α_family / E_runs`; (iii) **negative results are registered** — a drill that passes because it exercised nothing is worse than a failure, so each drill logs its coverage vector (which axioms, which code paths, which noise intensities) and the harness refuses a "pass" with empty coverage. The harness itself runs under the degraded-mode simulator in CI (R2 #95): every drill is also executed against the degraded profiles (vTPM, MCU-offload, reduced PQ) — a drill that only passes on the golden path is not a drill, it is a demonstration.

### 6.4 SIMTIME harness & canonical test vectors

SIMTIME is the suite's deterministic simulation clock: it replays recorded noise, load, and attack traces against the kernel with the real module code but virtualized time and injected entropy — the same discipline that lets T1.8's replay determinism be tested before deployment. Phase 1 fixes the canonical vectors below as versioned fixtures; every later phase extends the corpus and MUST keep these green.

| Vector ID | Content | Proves | Pass condition |
|---|---|---|---|
| V1-CHATTER | 18,808-cycle dwell trace, `D = 0.02` | T1.3 | dwell accumulates; zero INSIDE↔MARGIN ping-pong |
| V1-CUSUM | 160/166 saturation trace | T1.5 | choke engages; guardian ε share < dominated within 1 epoch |
| V1-EPS-FUZZ | 10⁴ crafted registry entries incl. multiplicative claims | T1.1 | 100% rejection; quarantines logged as impulses |
| V1-NULL-128 | Known null distribution + 128-probe gate | T1.2 | accept at α, reject at α/2; declared resolution matches |
| V1-NULL-OFFLOAD | MCU feature vectors → Server verdict round-trip | T1.2 + R1 #71 | verdict + expiry honored; severed-MCU case holds (no action) |
| V1-POWCUT | 10⁴ write power-cut fuzz on LittleFS COW | T1.6/T1.12 | reconciliation ≤ ε_accounting; zero orphan pages |
| V1-EPOCH-SWEEP | Ticket × epoch-bump matrix (4-tuple) | T1.7 | 100% fail-closed; receipts name governing epoch |
| V1-REPLAY-30 | 30 sampled enforcement records | T1.8 | δ_R = 0 on all; any mutation voids |
| V1-BURN | Auth-burn saturation in simulated reservoir | T1.14 | halt-with-receipt at ℓ_net ≤ 0; throttle at 80% |
| V1-CAPACITY | 2× overload ramp on Server card | T1.11 | p99 within bound; zero silent drops |
| V1-CAUSAL | Forged instant-enforcement injections | T1.9 | refusal within one epoch tick, all platforms |
| V1-GODEL | Policy injection ×3 vectors | T1.13 | fail-closed, identical receipt format |

Harness custody rule (inherited): the harness consumes digests, ε values, presence flags, and bounds — never key bytes; vectors are hence reproducible in any audit environment without key custody, which is what makes them third-party checkable (extended to disaligned-party replay in Phase 3, R2 #83).

---

## 7. Phase Gates

### 7.1 Entry criteria (M0)

| # | Criterion | Evidence |
|---|---|---|
| E1 | Groundwork Blueprints v1.0 + enhancement registry (R1/R2/R3) frozen as normative baseline | baseline tag |
| E2 | Interop wire contract (CBOR, dual-signature, ML-KEM-768 KDF, CTOK-lite) schema-checked | schema repo CI green |
| E3 | Reference hardware procured and characterized (x86_64 Server, TPM 2.0 machine, ESP32-S3/nRF52840/STM32H7 kits) | hardware register |
| E4 | Historical audit corpora staged for regression (18,808-cycle chatter trace; 160/166 CUSUM trace; seven halt traces for later phases) | corpus repo |
| E5 | Falsifier registry F1–F5 read-only API contract defined (full instrumentation lands Phase 3) | API spec |

### 7.2 Exit criteria (M3) — go/no-go metrics

| # | Metric | Go threshold | No-go action |
|---|---|---|---|
| G1 | T1.1–T1.14 acceptance tests in CI | 14/14 green on Linux + Windows; 11/14 green on MCU (T1.5/T1.13 Server-side with offload proven) | fix before any Phase-2 workstream starts |
| G2 | Interop drill v1 (§6.1, items 1–6) | 6/6 pass, zero silent degradations, all four honesty fields in 100% of attestations | release train blocked |
| G3 | Replay determinism | δ_R = 0 on 30/30 sampled records across the triangle | void records; root-cause before exit |
| G4 | MCU A3 compile-time enforcement | overflow build fails 100% of attempts; ML-KEM-512 profile declared in 100% of handshakes | spec bug; fix |
| G5 | Guardian saturation drill (T1.5) | choke engages on 160/166 replay within one epoch | fix governor |
| G6 | A6/A8/A9 drill battery | 100% fail-closed/refusal rates for the month preceding exit | extend window |
| G7 | Honesty-field linter | 0 violations across all attestation corpora | fix emitters |

**Rollback plan.** If any go threshold fails at M3: (1) Phase-2 workstreams do not start — the phase gate is a hard gate, not a steering suggestion; (2) the failing theorem's module is reverted to its last green state and the defect is filed against the theorem (the acceptance test that caught it is the theorem working as designed — this distinction is logged); (3) if the failure is in the wire contract itself (schema-level), the baseline is amended via a signed constitution amendment (A6 path — never by guardian discretion) and all three platforms re-run the full bake from item 1. No partial promotions exist: the triangle promotes as a unit or not at all, because the phase's deliverable is the *contract between* the platforms, not any single platform.

### 7.3 Boundary handoff to Phase 2

Phase 1 hands Phase 2: a green kernel with FFI-stable APIs; characterized reference hardware; the drill harness with degraded-mode simulation; the SIMTIME-proven auth-burn governor, finite-key smear calculator, and LoRa puzzle solver (bench-verified); and an ε-ledger grammar that Round-2/3 items already join additively. Phase 2 begins with the SDR RX-honesty drill and the LoRa-bootstrap interop drill — both specified in the Phase 2 blueprint (W2.4/W2.5).

---

## 8. Traceability Matrix (Phase 1)

Every axiom carries its Phase-1 instrument set; every enhancement ID cited resolves to its Round-1/Round-2/Round-3 specification. Cross-cutting acceptance (inherited): every shipped item carries (a) its equation in the wire schema, (b) its drill in the calendar, (c) its ε term in the additive ledger, (d) its D_impl or slack field where the platform has a gap. Anything shipping without all four fails conformance.

| Axiom / Theorem | Instrument (Phase 1) | Enhancement bindings | Drill / acceptance |
|---|---|---|---|
| A1 + T1.6/T1.12 | hash-chain verifier; COW fork audit | R1 #8, #12, #54, #70; R3 #5, #13, #20, #23 | kill-switch drill; power-cut fuzz |
| A2 | reciprocity telemetry (Server) | R1 #13; R2 #77 | agent-swap canary games (Phase-1 scale) |
| A3 + T1.5/T1.10/T1.11 | control-plane byte meter; linker assertion; capacity card | R1 #65, #73, #82; R2 #61, #82; R3 #10, #21 | INJECT to 1% bound; overflow build; 2× overload |
| A4 + T1.9 | latency honest-stamper | R1 #14, #57; R2 #31 (clock limb) | impossible-latency injection; p99 histogram |
| A5 + T1.1/T1.14 | ε decomposition ledger | R1 #1, #10, #11; R2 #33, #36 | registry fuzz; SIMTIME burn saturation |
| A6 + T1.13 | constitution-as-compiles; dry-run witness | R1 #53, #88, #96; R2 #1, #80, #84 | self-amendment ×3 vectors; CI firewall seed |
| A7 + T1.2 | rotation-probe harness (n = 128) | R1 #2, #71; R3 #31 | null-distribution unit test; offload round-trip |
| A8 + T1.4 | limb-floor watchdog; schema linter | R1 #4, #58, #66, #87 | 1-in-50 sensor kill; aggregate rejection |
| A9 + T1.7 | epoch fabric client | R1 #15, #35, #52, #72; R2 #24 | expired-ticket sweep; QUIC epoch close |
| A10 + T1.8 | replay bundle + verifier toolchain | R1 #12, #49, #51, #53; R2 #3, #9 | 30-record replay audit; boot-tamper void |
| Supply chain (cross) | SBOM ledger; transparency log; toolchain pin | R2 #2, #5, #8, #9 | build-reproducibility witness |
| Drill harness (cross) | degraded-mode simulator; coverage vector logging | R2 #95; R1 #88–#94 | §6.3 statistical rules |

**Enhancement coverage check.** Phase 1 consumes R1 #1–16 (kernel), #49–58 (Win/Linux), #65–72 (MCU), #88–96 (drills/interop); R2 #1–12, #18, #20–24, #31, #33, #36, #61, #62, #80, #82, #84, #93, #95; R3 #5, #8, #10, #13, #20, #21, #23, #31. Items cited but deferred: #25 (live LoRa interop → Phase 2), #16 (full falsifier API → Phase 3), #63 pricing deep-dive (→ Phase 3, T3.5). No Phase-1 deliverable may cite an item as "done" that its own acceptance test has not armed.

---

## 9. Honesty Ledger (Phase 1 — kept, non-negotiable)

1. **The triangle has no photonics.** Every `qkd_slot` in every Phase-1 handshake reads `absent`. The auth-burn governor, finite-key smear, and Q-Pad-Lambda mode logic are proven in SIMTIME so that Phase 2 inherits tested components; none of them has touched a real optical channel yet, and no document in this series may say otherwise.
2. **The MCU never self-certifies novelty.** A7 on the MCU profile is a *deferred verdict*, not a reduced one: the probe ensemble lives on the Server. The published limitation is that an MCU severed from its Server holds (no action) rather than gating locally — this is the Jensen-floor-correct division of labor, and it is stated in every MCU capability token.
3. **Phase 1 theorems are proven for their regimes.** T1.3's Kramers scaling holds in the linear-response regime (Math §4's regime warning applies at theory scale): under declared degraded modes (crisis, partition), A8 and A10 bind *harder*, never looser — the phase's drills reflect that by running against degraded profiles, not just golden path.
4. **The capacity card is measured, not aspirational.** 18k handshakes/s is an equation evaluated on reference hardware (T1.11); deployments on slower hardware MUST derive their own card per R2 #67 and publish it, not inherit the reference number.
5. **This document prices itself.** Its disturbance charge is the falsifier surface it arms: 14 theorem acceptance tests, 6 bake items, 7 go/no-go gates. If a later revision removes or softens any of them without a signed constitution amendment, Phase 1 violates its own A6 and the phase certification voids — the same clause the theory's honesty ledger applies to itself.

**STATUS:** 3 platform workstreams specified · 14 theorems with proofs, predictions, and bindings armed · 1 interop drill protocol · 7 go/no-go gates published · rollback plan fixed · 0 silent-degradation paths representable.

**COHERENCE:** CI = 0.998 claimed at suite level; Phase-1 measured CI is published per release candidate alongside the ε decomposition dashboard — per A5, this phase pays for its own measurement.

---
*Next in series: Phase 2 Blueprint (M3–M6) — Edge & Mobile Expansion: Mini ARM + Android + iOS client, Quantum-RF bootstrap; Phase 3 Blueprint (M6–M9) — Full Fleet Convergence & Certification. Phase 4 (guardian federation) remains out of scope and is referenced only as the boundary it defines.*
