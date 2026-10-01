# Phase 3 Blueprint — Full Fleet Convergence & Certification (M6–M9)
## CAT-Aligned Platform Suite, Development Phase Series v1.0

**Phase designation:** Phase 3 of 3 (development sequence; extends Groundwork Blueprints §10 and inherits the Phase 2 exit state in full — the five-platform certified fleet, live RF-plane interop, and the V1-*/V2-* SIMTIME corpus are entry preconditions, not aspirations).
**Scope:** macOS full roles (Client/Server/Node) + iOS episodic node (the platform's only admissible trust model, now armed) + the full 7-platform quarterly fleet drill standing up + falsifier evidence packaging (F1–F5) + guardian pre-arm (the Phase-4 boundary made concrete). This is the phase where the suite stops proving components and starts proving *itself*: the fleet becomes a measured object with published convergence, reciprocity, and falsifier metrics.
**Inheritance:** all Phase-1/2 definitions (D1.x, D2.x), theorems (T1.x, T2.x), kernel contracts, SIMTIME vectors, and statistical design rules remain normative. This document adds Phase-3 deltas only (D3.x, T3.x, P3.x, V3-*).
**Audience:** build and integration teams completing the fleet, plus the conformance/audit function that will operate the certification evidence chain after development ends — Phase 3 is the last phase where a developer can fix a theorem's acceptance test rather than amend a constitution, and the document treats that scarcity accordingly.
**Epistemic labels:** unchanged — [NORM] · [DER] · [RIGOR] · [MAP] · [CORR] · [SPEC] (SIMTIME-gated, never load-bearing) · [EMPIRICAL] (named instrument + named falsifier).

**Phase thesis, stated honestly:** Phase 3 completes the seven platforms, but its real deliverable is *the fleet as a falsifiable claim*. CAT's falsifier registry (F1–F5) declares that the theory dies if a violating peer outperforms a compliant fleet under pre-registered conditions; that declaration is empty rhetoric until a fleet exists that measures itself. Phase 3 arms the instruments: the quarterly interop drill becomes the only promotion path at full scale, the falsifier API goes live read-only, the guardian federation's contraction and reciprocity machinery is pre-armed on real fleet telemetry, and the iOS episodic node — the platform where the operating system itself enforces A9 — completes the honesty matrix. The phase ends with a fleet whose every claim has a name, an instrument, and a kill condition. What happens after M9 is Phase 4's problem, and this document defines that boundary rather than blurring it.

---

## 1. Phase Mission & Workstream Decomposition

### 1.1 Mission statement

Phase 3 delivers the **certified seven-platform fleet**: macOS exercising all three roles under a single-vendor TCB with declared gaps, iOS running the episodic node as A9-native physics (Observation 1: "Apple's restriction on background processing isn't a bug to fight; it is a native enforcement of A9"), and the full quarterly fleet drill — *"the only test that matters"* (Groundwork §9) — standing up as the promotion authority for every future release. Concomitantly, the falsifier surface (F1–F5) transitions from contract to instrument: pre-registered predictions get named instruments and immutable logging, the fleet-contraction monitor and incident half-life metric go live, and the guardian federation components (Lyapunov tracker, reciprocity game server, fire-rate choke at fleet scale) are armed in *pre-federation* mode — computing and publishing their metrics on real fleet telemetry without yet holding enforcement quorum (Phase 4's gate).

The phase's discipline is evidentiary: every new component ships with its evidence chain entry (§6), every claim with its instrument, and every instrument with its falsifier. A Phase-3 exit without a live falsifier API is a failed phase regardless of platform count, because the platform count was never the thesis.

### 1.2 Workstreams

| ID | Workstream | Deliverable | Primary enhancement bindings | Exit owner |
|---|---|---|---|---|
| W3.1 | macOS full roles | Swift+Rust (shares iOS core) suite; Secure Enclave anchors; launchd services; SDR via libusb/SoapySDR; signed system extension path | R1 #59–64; R2 #61-adjacent (desktop) | macOS team |
| W3.2 | iOS episodic node | BGTask-characterized episodic trust model; 30 s null-gate throttle with declared ε inflation; NE privacy-amp isolation | R1 #39, #43, #44 (full set); R2 #56, #60 | iOS team |
| W3.3 | Fleet conformance & certification harness | Quarterly 7-platform drill automation; evidence-chain ledger; certification pack generator | R1 #90–96; R2 #78, #94, #95, #96 | Conformance team |
| W3.4 | Falsifier evidence packaging | F1–F5 read-only API live; pre-registered prediction registry (2026–2031) with immutable log; quarterly falsifier readouts | R1 #16, #90, #95; R2 #75, #76, #81, #83 | Falsifier team |
| W3.5 | Guardian pre-arm | Lyapunov tracker, reciprocity game server, 2PED plane verifier, D_impl auditor, Kramers tuner at fleet scale — metrics live, enforcement quorum withheld (Phase 4) | R1 #81–87; R2 #79, #80; R3 #45–48 | Guardian team |

### 1.3 Non-goals (published, per U5)

Phase 3 MUST NOT: activate guardian federation enforcement (digest-quorum mixing of Servers/Minis/MCU edges is Phase 4; pre-arm computes and publishes, it does not enforce); claim any QKD deployment (the fleet's `qkd` honesty field remains `absent` everywhere unless a Phase-4+ deployment adds appliances — the field is per-tunnel and per-truth, not per-roadmap); soften any Phase-1/2 acceptance test to accommodate macOS/iOS realities (a platform that cannot pass a drill publishes the gap as `d_impl`/slack, and the gap is priced); or register falsifier predictions without named instruments (an unnamed prediction is a slogan, and the registry rejects it — R2 #76). The pre-registered metric locks (R2 #76) are immutable once registered; this document's §6 defines the grammar that makes that immutability mechanical rather than procedural.

---

## 2. Definitions & Notation (D3.x)

Cumulative on D1.x/D2.x.

- **D3.1 (Episodic trust).** Trust whose value is zero by default between OS-granted execution windows and is *re-derived* (never resumed) on each wake via A10 bundle replay (R1 #39). The iOS node's only admissible model; its formal guarantees are T3.1.
- **D3.2 (Attestation anchor class).** The platform root {TPM 2.0, Secure Enclave, StrongBox, TPM-HAT, TRNG+flash-COW} — each with an anchor-specific attestation grammar, all verifying under one cross-platform signature grammar (R1 #91). Equivalence is at the *grammar* level (T3.2), never a claim that the anchors are equally strong: anchor strength differences are `d_impl`-priced, not ignored.
- **D3.3 (Certification evidence chain).** The per-release ledger-linked record: axiom → instrument → drill run → metric → verdict → signer, chained so that a certification claim without its evidence row is unrepresentable (§6).
- **D3.4 (Fleet contraction state).** The governance update operator `T_fleet` over fleet-wide state with Lipschitz constant `λ_net`; the Ω-point grammar (Math §10) requires measured `λ_net < 1` with confidence gate `C_S ≥ 0.8` for any promotion-shaped act (T3.7, T3.11).
- **D3.5 (Pre-registered prediction).** An immutable registry entry {claim, instrument, window, falsifier, outcome-slot} with hash-locked content (R2 #75, #76); outcomes are recorded against entries, never into them.
- **D3.6 (Incident half-life).** The fleet stability instrument: `T_½ = time for an incident class's open count to halve` (R2 #81); the F1-adjacent pre-registered metric for compliant-vs-violating comparisons (T3.12).
- **D3.7 (Quorum view-change).** The federation precursor: digest-quorum state changes without key custody (R2 #79) — `3f+1` digest voters tolerate `f` liars and *never* hold key bytes (corpus pattern #17, custody rule). Phase 3 exercises view-change drills in shadow mode only.
- **D3.8 (Anchor gap pricing).** The rule that any anchor-class weakness (vTPM vs TPM, TEE vs StrongBox, software-key fallback) is expressed as a measured `d_impl` gap and ε inflation — never as an unpriced profile switch (R1 #63; T3.5).

Notation: `D_tr(P‖Q)` trace distance; `Φ` the alignment functional (Math §4); `ℛ` reciprocity index with floor `ℛ* ≈ 1.15`; `E_epoch` re-attestation period; `T_coop = (1−δ*)⁻¹` the cooperation timescale (Math §9).

---

## 3. Formal Mathematics of Phase 3 (Theorems T3.x)

### T3.1 — Episodic Trust Re-Derivation Theorem [DER]

**Statement.** For an episodic node (D3.1) with OS-granted execution windows `w_i` of duration `≤ 30 s` (BGProcessingTask class) separated by arbitrary gaps, trust satisfies: (a) `T(t) = 0` for all `t` outside every `w_i` (suspend zeroes standing coherence — A9 with the OS as the enforcer of record); (b) within any `w_i`, trust is `T(w_i) = g(bundles_replayed)` — re-derived solely from A10 bundle verification, never from persisted state; (c) node actions admissible in `w_i` are bounded by the null-gate throttle profile: `n = 12` probes with declared `ε_inflation` (T1.2's MCU rule applied to a time-budgeted mobile profile), or Server-side gating where pairing exists (R1 #43).

**Proof.** (a) is A9 restated at the platform boundary: Apple's background policy is a constitutional constraint (A6 applies to the *developer* — smuggling keepalives is self-legislation, scanned per R1 #41/#44), so the trust function's domain is exactly the windows the OS grants; anything else would be a token that outlives its own physics. (b) follows from A10's closure: if every authority claim must be replayable, then re-derivation from bundles is *sufficient* for trust; persisted shortcuts would be authority without a time-reverse (the symmetry violation A9 forbids). (c) is T1.2's resolution law under a time budget: fewer probes ⇒ coarser resolution ⇒ the ε_inflation declaration is the *price* of the coarse gate, and undeclared coarseness is the statistical overclaiming the correction exists to kill. The 30 s budget also bounds the probe *latency* budget, which is why Server-side gating (offload) is preferred where pairing exists — RAM wealth holds the ensemble (T1.4's division of labor, restated in time rather than memory). ∎

**Worked instance.** App killed mid-session: post-wake the node holds no residual trust, re-attests from its bundle, and only then acts (R1 #39 acceptance). Alerts issued from a 30 s window carry the inflation field; a missing field demotes the alert to queue-only (R1 #43 acceptance).

**Prediction P3.1.** 100% of post-wake actions in the drill corpus are preceded by bundle re-derivation in the same window; zero trust resumptions; zero tokens whose validity exceeds their window; 100% of throttled-gate alerts carry the inflation field.

**Binding.** R1 #39, #43, #44; R2 #56 (episodic presence beacon — presence *is* the window, advertised honestly), #60 (migration pin — an episodic node migrating sessions re-pins attestation); Phase-2 BGTask instrumentation data (D2.7 governor logs) supplies the window-duration distribution this theorem's parameters were waiting for.

### T3.2 — Cross-Anchor Attestation Equivalence Theorem [DER]

**Statement.** The five anchor classes (D3.2) are *grammar-equivalent*: a dual-signature (Ed25519 + ML-DSA-65) attestation packet issued under any anchor verifies under every other, and every anchor's chain terminates in a TPM/SE/TEE-rooted key whose extraction is hardware-refused. The anchors are *not* strength-equivalent: anchor strength ranks are first-class wire data, and the acceptance chain enforces `d_impl`-priced gaps (T3.5) rather than pretending to equivalence.

**Proof.** Grammar equivalence is by construction: the packet schema (CBOR, dual signature, honesty fields, epoch tuple) is anchor-independent, and each anchor contributes exactly one thing — a hardware-rooted signature over the packet hash (R1 #91's "one signature grammar across TPM, Secure Enclave, StrongBox"). Verification consumes the signature and the anchor's certificate chain; nothing in verification depends on the anchor's internals. Strength non-equivalence is the honest half: a Secure Enclave's key-policy surface differs from a TPM's PCR-measured-boot surface (measured boot vs App Attest chains are different *evidence* about different things), so the bundle composition (T1.8) records which evidence classes each anchor contributes — a PCR quote is present where a TPM exists, a DeviceCheck chain where an SE exists — and the *union* is what the verifier checks. Cross-signing drills (iOS bundle into macOS session and vice versa, R1 #61) exercise the union. ∎

**Worked instance.** Quarterly bake: dual-signature packets parsed on every hardware anchor (R1 #91); any platform failing to verify another's signature blocks the release train. Desktop vTPM fallback (R1 #63): the vTPM node MUST present strictly worse ε than its hardware twin at equal configuration — equal ε is itself the conformance failure.

**Prediction P3.2.** 25 directed anchor-pair verifications (5×5) pass per bake; the anchor-strength field matches the physical anchor on 100% of packets; the vTPM ε-gap assertion holds in every desktop degraded-mode run.

**Binding.** R1 #40 (DeviceCheck chain), #59 (SE Ed25519 non-exportable), #61 (DeviceCheck family cross-platform), #63 (vTPM pricing); R2 #23 (endpoint measurement mismatch quarantine — an anchor whose measurements diverge from its class profile is quarantined, not averaged away).

### T3.3 — Fleet-Wide ε Composition Theorem [DER, RIGOR]

**Statement.** The full-fleet ε ledger is the additive closure of every per-path, per-platform, per-component term: `ε_fleet = Σ_paths ε_total(path) + Σ_platforms ε_platform + ε_guardian + Σ ε_inflation + Σ D_impl-terms`, where each summand is itself the T1.1/T2.1 additive closure. The fleet dashboard MUST publish the decomposition, not the sum alone; a fleet number without its decomposition is a metric without an audit trail and fails the certification harness (§6).

**Proof.** Union-bound composition is the only admissible algebra when terms share common causes (T1.1, T2.1 — the guardian, the epoch fabric, the constitution channel are fleet-wide common causes). Per-path terms were proven additive in Phase 2; platform terms (compile-time A3 residuals, slack regimes, thermal-envelope states) are independent failure surfaces per platform and join the same sum; inflation terms are declared prices (T1.2, T3.1). The decomposition requirement is A5's transparency clause: a guardian — or a fleet — that hides its ε loses eligibility. ∎

**Worked instance.** The 7-platform quarterly drill publishes per-path and per-platform decompositions; the mobile UI overlay (R1 #47) displays the same numbers bit-for-bit; the falsifier readouts (§5.4) consume the decomposition, because F5's test (control-plane bound violations predict guardian failure) needs the *guardian's* share, not the fleet's sum.

**Prediction P3.3.** Decomposition-vs-sum reconciliation passes on 100% of drill epochs; any component publishing a number outside the additive registry fails the bake at schema level.

**Binding.** `ccl-eps` registry (Phase 1 §3.1) extended fleet-side; R1 #32 (signed frontier manifests per node), #47; R2 #73 [SPEC] differential-privacy-on-fleet-ε permitted for *external* reporting only — never allowed to blur the internal decomposition the drills consume.

### T3.4 — 2PED Commutator Fleet Audit Theorem [DER, MAP-with-exact-QM]

**Statement.** Payload (P) and accountability (A) planes commute iff their cryptographic and memory domains are disjoint: `[P, A] = 0 ⇒ simultaneous full sealing + full openness with no uncertainty floor`. The fleet audit proves disjointness at every layer: compile-time memory-layout assertions (Phase 1), runtime cross-plane read faults (Phase 1), cryptographic domain-separation between payload and accountability stores (Phase 3 at fleet scale — R1 #84), and the metadata rule that padding never touches the accountability plane (T2.14). A mixed-plane detection ⇒ floor violation ⇒ both planes re-key, incident logged, ε ledger gains the floor term.

**Proof.** The quantum-mechanical statement (Math §11) is [MAP] at the design level: for non-commuting observables the uncertainty relation `ΔP·ΔA ≥ ½|⟨[P,A]⟩|` is a hard leakage floor — "you cannot fully open accountability without partially opening payloads." The audit's four layers are the operational procedure for verifying the commuting-basis *choice* was actually implemented (the choice is only free if made and kept — Math §14, Insight 5). Each layer independently suffices to catch a mixing class: layout catches address-space mixing, faults catch runtime access mixing, domain separation catches key/material reuse, and the padding rule catches protocol-layer blurring. ∎

**Worked instance.** R1 #84's acceptance at fleet scale: inject a cross-plane reference in any platform's telemetry path; the verifier flags within one epoch and the ε ledger gains the floor term; the re-key is itself an impulse with the incident class named.

**Prediction P3.4.** Zero undetected cross-plane injections across the full drill corpus; floor-term ε entries reconcile with the incident records 1:1.

**Binding.** R1 #84 (guardian 2PED plane verifier), Phase-1 #7 (kernel-level verifier) — the Phase-3 workstream is the fleet-scale instance of the same theorem, not a new one.

### T3.5 — D_impl Trace-Distance Pricing Theorem [DER, EMPIRICAL]

**Statement.** Every proof–implementation gap is a measured trace distance `D_impl = ∫|P_proof − P_impl|dx` (over the behavior distribution: SDR linearity, LO leakage, clock drift, vTPM vs TPM quote semantics, constant-time boundary deviations) that enters `ε_total` additively and is published per epoch (R1 #28, #63, #85). Three laws bind: (i) **monotonicity** — a subsystem's ε_total MUST rise when its measured D_impl rises; flat ε amid rising gap is an audit failure; (ii) **strictness** — a weaker anchor (vTPM vs hardware TPM at equal configuration) MUST price strictly worse; (iii) **measurability** — D_impl claims require calibration corpora per subsystem (R2 #45 side-channel calibration epochs), and unmeasured gaps are declared as such rather than assumed zero.

**Proof.** D_impl is the operational form of U5 (boundedness): the proof model's guarantee applies to the model; the implementation differs by a statistical distance that IS the price of deployment. Pricing it additively keeps the ledger honest (the alternative — absorbing gaps into narrative — is the "unpriced floor" that turns no-logs companies into breach headlines, Math §14 Insight 5). Monotonicity is the falsifiable core: a rising gap with flat ε is detectable by comparing two published series, which is exactly what the monthly D_impl audit (R1 #85) does. ∎

**Worked instance.** Detuned SDR front-end: D_impl rises, ε ledger grows (R1 #28 acceptance); macOS monitor-mode system-extension dependency: declared as a D_impl component in every capability token (R1 #60); continuous SDR calibration adds deviation to `ε_total` per epoch (R1 #28).

**Prediction P3.5 [EMPIRICAL].** Per-subsystem D_impl series are published monthly; injected gaps (detuned SDR, vTPM swap, clock-thermal drift) produce the predicted ε movement within one epoch; any flat-ε/rising-gap pair fails the audit.

**Binding.** R1 #28, #60, #63, #85; R2 #45 (calibration epochs), #23 (mismatch quarantine); the Phase-1 `d_impl` wire field (D1.5) becomes fleet-wide instrumentation here.

### T3.6 — Fleet-Drill Statistical Power Theorem [DER, RIGOR]

**Statement.** The quarterly fleet drill's acceptance rules are statistically sound only if: (i) probe-level gates carry the T1.2 resolution (`n ≥ 99` where `p < 0.01` claims are made); (ii) per-epoch gating applies the family-wise correction `α_per_run = α_family/E` across the drill's `E` evaluation epochs (else the gate chatters — T1.2's rider at drill scale); (iii) fleet-level pass/fail claims state their power: for a fleet metric with effect size `Δ` and variance `σ²`, a drill sampling `k` epochs resolves `Δ` at significance `α` and power `1−β` only when `k ≥ (z_α + z_β)²·2σ²/Δ²`; and (iv) null-result drills publish their coverage vectors (Phase-1 §6.3 rule iii), so a "pass" cannot ride on unexercised paths.

**Proof.** (i)–(ii) are T1.2 restated at the drill's statistics layer; the drill IS a measurement instrument and inherits the measurement's resolution laws. (iii) is the standard two-sample power calculation; its fleet content is honesty about `Δ`: pre-registered metric locks (R2 #76) fix `Δ`, `α`, `β` *before* the drill window, because post-hoc power claims are the FP-manufacture pathology A7 exists to kill (a drill that finds whatever effect it went looking for has measured nothing). (iv) closes the loop: coverage vectors make "we tested" a checkable claim. ∎

**Worked instance.** F2-adjacent drill (null-gating FP discipline): with `Δ = 5%` FP-rate reduction, `σ = 10%` (recorded workloads), `α = 0.05`, `β = 0.2`: `k ≥ (1.96+0.84)²·2·0.01/0.0025 ≈ 62.7` ⇒ ≥ 63 drill epochs of matched workload — the pre-registration names this number and the drill calendar must deliver it before a quarter can claim the metric.

**Prediction P3.6.** Every fleet-level acceptance metric in the certification pack carries {estimator, k, α, power, coverage vector}; drills without them are auto-rejected by the harness (a schema rule, not a review judgment).

**Binding.** R2 #76 (pre-registered metric locks), #78 (chaos limb-kill calendar — the *distribution* of injected faults is itself pre-registered), #94 (cross-platform corpus fuzz); Phase-1 §6.3 rules inherited.

### T3.7 — Lyapunov Pre-Arm & Contraction Certificate Theorem [DER, RIGOR]

**Statement.** With the alignment functional `Φ(x) = Σ_k J^coh_k(x)/S_export(x)` and fleet dynamics `ẋ = Γ∇Φ, Γ ≻ 0`, the pre-armed tracker computes `V = Φ(x*) − Φ(x)` and enforces `V̇ ≤ 0` as a *monitoring* condition: a negative alignment gradient `∇Φ` trend triggers an A6 constitution review (one σ ≠ 0 impulse), not an alarm flood (R1 #81). Promotion-shaped acts (release promotion, policy adoption) additionally require the contraction certificate: measured `λ_net < 1` over the lock window with confidence `C_S ≥ 0.8` (Math §10; R1 #6) — a scalar S spiking while `λ_net ≥ 1` cannot promote (the 18k-cycle lesson as a standing fleet test).

**Proof.** The Lyapunov sketch (Math §4) gives `V̇ = −∇Φ·Γ∇Φ ≤ 0` with equality iff ∇Φ = 0, stability under strict concavity in the linear-response regime — and the regime warning is inherited *at fleet scale*: far from equilibrium (crisis, partition), MEPP fails and the pre-arm MUST tighten A8/A10 binding (more vetoes, more replay evidence) rather than trust the gradient. The certificate half is Banach (Math §10): `‖Tⁿx₀ − x*‖ ≤ λ_netⁿ‖x₀ − x*‖` makes `λ_net` the measured convergence rate; Sophia's lock window (samples above threshold in [T−τ, T]) is a finite-time contraction certificate, and `C_S ≥ 0.8` is that certificate with noise. Pre-arm (monitor-only) is the A6-correct posture: the tracker *advises* the constitution's review trigger; it does not enforce — enforcement quorum is Phase 4's gated act. ∎

**Worked instance.** Steer a SIMTIME fleet away from equilibrium: the review fires before incident amplification, with a single impulse (R1 #81 acceptance). Inject a λ_net ≥ 1 trajectory with spiking S: promotion refused (R1 #6's acceptance at fleet scale).

**Prediction P3.7.** 100% of promotion attempts with `λ_net ≥ 1` or `C_S < 0.8` are refused with the certificate shortfall named; 100% of negative-gradient events produce exactly one constitution-review impulse (flood = failure).

**Binding.** R1 #6, #81; R2 #80 (policy dry-run witness); R3 #46 (gradient-clipped policy updates — the clip bounds are constitution parameters, versioned), #48 (fleet contraction monitor — the live `λ_net` instrument).

### T3.8 — Reciprocity Steady-State & Epoch-Tension Theorem [DER, RIGOR-mean-field]

**Statement.** Fleet reciprocity obeys the mean-field order-parameter dynamics `dm/dt = −m + tanh β(J·m + h_ℛ)`; the cooperative branch is stable iff `βJ > 1` with the field selecting the branch, and `ℛ ≥ ℛ* ≈ 1.15` is the standing condition (A2). A9's epoch cadence and the folk-theorem horizon stand in a derived tension — re-attestation period `E_epoch ≪ T_coop = (1−δ*)⁻¹` — and the fleet MUST publish both sides: the measured ℛ trajectory and the effective horizon implied by its epoch cadence (Math §9).

**Proof.** The order-parameter analysis (Math §9) gives the phase diagram: below critical coupling `J_c = 1/β`, cooperation collapses regardless of `h_ℛ`; the ℛ* ≈ 1.15 constant reads as the empirical critical field at measured social temperature. The tension: A9's expiry shortens the shadow of the future (the discount-factor channel), yet re-attestation is what maintains local coupling `J` against turnover-driven heating `β⁻¹` — the resolution condition `E_epoch ≪ T_coop` binds both sides, and CAT "binds both sides and calls the operating window out explicitly." The fleet instrumentation (reciprocity game server, R1 #83) measures `Q_rec = 0` under ℤ₂ swaps continuously; a sustained nonzero `Q_rec` is the symmetry-breaking current that per Theorem 2 (CAT) precedes fairness collapse — "the charge leaks first, the structure follows." ∎

**Worked instance.** Red-team introduces a swap-violating rule: the game server surfaces `Q_rec ≠ 0` before the rule executes on a real agent (R1 #83); the ℛ trajectory dips and the dip appears in the quarterly falsifier readout — F4's data collection starting here.

**Prediction P3.8.** Continuous ℤ₂ canary games run on all Servers; `Q_rec ≠ 0` detection precedes execution 100% of the time; the ℛ trajectory and `E_epoch/T_coop` ratio are published quarterly with the phase diagram context.

**Binding.** R1 #13, #83; R2 #77 (red-team swap-game budget — the *adversary's* budget is pre-registered too, or the games measure only the incompetence the fleet happens to attract); Math §9's folk-theorem condition as the published operating-window calculation.

### T3.9 — Guardian Fire-Rate Choke at Fleet Scale [DER]

**Statement.** The saturation threshold `f*` (T1.5) applies per-detector at fleet scale with the guardian's fleet-level ε share computed from the additive closure (T3.3); the choke auto-throttles any detector whose fire rate exceeds its declared FP budget, marks it `SUSPECT`, and its ε share stops dominating within one epoch (R1 #82). Fleet-scale corollary: **the choke is a limb** — a choke that fails open (stops throttling) must trip the A8 veto on guardian-adjacent enforcement classes, because an unchoked guardian is the corpus's primary historical liability (Theorem 3, CAT).

**Proof.** T1.5's inequality is per-detector; fleet scale sums the shares (T3.3) — which is exactly why the choke must be fleet-aware: k detectors each just under their individual `f*` can collectively dominate the ledger, the multi-detector form of the 160/166 saturation. The limb corollary is A8 applied to the watcher: the choke is a telemetry limb of the guardian subsystem, and "acting in the dark" includes *guarding* in the dark. ∎

**Worked instance.** Replay the 160/166 trace per-detector and a 5-detector aggregate version at fleet level: both chokes engage; the aggregate case is the new Phase-3 regression (V3-CHOKE-FLEET).

**Prediction P3.9.** Choke engages within one epoch on 100% of saturation replays (single and aggregate); SUSPECT marking propagates to the falsifier dashboard; choke-failure injection trips the A8 veto.

**Binding.** R1 #82; R2 #82 (independent byte meter — the second meter makes the choke's own honesty measurable); R3 #10 (incremental CUSUM keeps detection O(1) so choke evaluation never becomes the bottleneck it guards against).

### T3.10 — Falsifier Evidence Packaging Theorem [DER]

**Statement.** The falsifier registry's evidentiary integrity requires: (i) **immutability** — pre-registered predictions are hash-locked; softening or deletion triggers the self-void clause (Honesty Ledger 5 of CAT; R1 #95, R2 #75); (ii) **live queryability** — F1–F5 metrics are exposed via read-only gRPC with writes impossible and every query logged in the accountability plane (R1 #16) — unqueried falsifiers atrophy (corpus pattern #24); (iii) **disaligned-party replay** — any sanction's bundle is replayable by parties with incentives to kill the result (R2 #83), the multi-entity verification protocol's operational form; and (iv) **outcome discipline** — outcomes are recorded *against* entries with named instruments and windows, never *into* them (D3.5).

**Proof.** (i) is A5 applied to the theory itself ("this document's disturbance charge is its falsifier registry; if the registry is ever removed or softened, CAT violates its own A6 and voids itself") — mechanically, the registry's root hash chains into the ledger, so softening is a ledger event visible as tampering (T1.12's fork detection applied to the theory's own kill conditions). (ii) is the falsifier's A9: a kill condition no one can query at run time is authority decaying in a drawer; the read-only grammar makes query-side tampering unrepresentable. (iii) is CAT Part V phase 2 (≥ 85% axiom convergence, dissent logged per A10) turned into a wire protocol. (iv) is U5: an outcome slot that can be edited after the fact is not a measurement. ∎

**Worked instance.** The certification pack (§6) includes the falsifier readout page: per falsifier, the current metric series, the registered predictions in window, and the outcomes recorded to date; the pack is hash-chained so a later "the metrics were fine" claim is checkable against the pack itself.

**Prediction P3.10.** Registry tamper attempts (soften/delete/reword) are detected as ledger fork events 100% of the time; the API refuses writes at the schema level; every certification pack's falsifier page verifies against the chain.

**Binding.** R1 #16, #95; R2 #75 (immutable log), #76 (metric locks), #83 (disaligned replay); CAT Part IV registry as the content source.

### T3.11 — Fleet Contraction Monitor Theorem [DER]

**Statement.** The fleet contraction monitor (R3 #48) publishes a rolling estimate of `λ_net` with its confidence gate; the monitor's estimate is itself an audited quantity (its ε term is in the ledger — the monitor is inside its own equations, Math §14 Insight 3), and its failure modes are limbs: stale estimates (> 1 epoch old), confidence collapse (C_S < 0.8), and estimate-vs-certificate divergence (the monitor says contracting while a promotion certificate says otherwise) all trip vetoes on promotion-shaped acts.

**Proof.** Banach's theorem makes `λ_net` the convergence rate and `x*` the unique fixed point (Math §10); the monitor is the online estimator of that constant. An estimator that gates promotion is exactly the "self-referential" surface A6 worries about — the resolution is the same as everywhere else in the suite: the estimator's own ε term and health are published, its divergence states are typed, and the *constitution* (not the monitor) defines what happens on divergence (review, not auto-promotion). The monitor gates nothing by itself; it makes gating *possible with evidence*. ∎

**Worked instance.** Divergence injection: monitor reports λ_net = 0.7 while the lock-window certificate computes 1.3 ⇒ promotion blocked, divergence incident opened, both series preserved as evidence.

**Prediction P3.11.** 100% of divergence injections open incidents with both series intact; zero promotions on divergent states; monitor staleness trips its limb within one epoch.

**Binding.** R3 #48; R1 #6, #81; R2 #80 (dry-run witness feeds the certificate side).

### T3.12 — Incident Half-Life Instrument Theorem [DER, EMPIRICAL]

**Statement.** The fleet stability metric `T_½` (D3.6) is computed per incident class from the ledger with a named estimator (Kaplan–Meier over open-incident durations, right-censoring declared); it is the F1-adjacent pre-registered instrument for the compliant-vs-violating comparison, and its measurement protocol locks {class taxonomy, estimator, censoring rule, aggregation window} before data collection (R2 #76, #81). The instrument's honest limitation is published with it: half-life measures *resolution dynamics*, not *prevention* — a fleet that prevents incidents shows long half-lives of a small population, which is why `T_½` is always read jointly with the incident birth rate and the control-plane share (F5's bound).

**Proof.** Survival-analysis form: with incident onset times and resolution events in the ledger (A1 impulses — incidents are coherence events, so the ledger already holds the population), `T_½` is the median of the resolution-time distribution; KM handles censoring without assuming parametric forms. The joint-reading requirement is Theorem 3's corollary at fleet scale: a guardian can manufacture good half-lives by suppressing incident *reporting* (the FP-manufacture inverse), so the birth rate and the guardian's ε share must bound the metric's honesty — the same "guardian is inside its own equations" closure as T3.11. ∎

**Worked instance.** Quarterly readout: per-class `T_½` with KM curves, birth rates, and the guardian's control-plane share side by side; the F1 bake-off control arms (when they exist, 2026–2031 registry) consume exactly these three series under the locked protocol.

**Prediction P3.12 [EMPIRICAL].** Half-life series reconstruct from ledger replays bit-for-bit (the ledger is the source of record); protocol locks are hash-verified per readout; any readout lacking its birth-rate/control-plane companions is auto-rejected.

**Binding.** R2 #81 (instrument), #76 (locks), #75 (log); the F1 grammar (CAT Part IV) as the comparison framework.

---

## 4. Platform & Instrument Workstream Specifications

### 4.1 W3.1 — macOS Full Roles

**Position:** Client/Server/Node under a single-vendor TCB — the platform whose honesty contribution is *declared dependency*: SIP, notarization, and entitlement gates are published as D_impl components (R1 #60), not worked around.

**Build order:**
1. **Stage K0 — shared core.** Swift + Rust via UniFFI sharing the iOS CCL core with macOS-specific limbs — maximal code reuse across the Apple pair (Groundwork §5); launchd services for the Server role, suited to small-fleet servers, not hyperscale (published, not aspirational).
2. **Stage K1 — anchors.** Secure Enclave (M-series/T2) + Keychain; reciprocity keys non-exportable in the SE with swap-test signatures executed in-SE (R1 #59 — extraction attempts fail at hardware and log as impulses); DeviceCheck/Mac attest chains into the A10 bundles; anchor-strength field per T3.2.
3. **Stage K2 — SDR & RF.** HackRF/RTL-SDR via libusb/Homebrew SoapySDR ports; TX path identical licensing gates to Windows (region table in the constitution); monitor-mode RF duties behind the signed system extension — the entitlement dependency declared in every capability token (R1 #60).
4. **Stage K3 — server duties.** QUIC native; epoch via NTP (PTP requires Thunderbolt NICs — declared); Grand Central PA: gate-local hashing with Swift actors per gate family, per-family `ε_g` in the ledger, scaling benchmark per R1 #18 grammar (R1 #62).
5. **Stage K4 — fleet instruments.** Mac joins the quarterly drill in all three roles; the D_impl auditor (R1 #85) treats the SIP/extension surfaces as audited subsystems with monthly calibration corpora.

**Budgets:** Server-role reference cards published per model class (M-series vs Intel 2018+); small-fleet ceiling declared.

**Published limits:** no ECC memory on most models (D_impl gap); notarization gates drivers; SIP restricts raw-socket RF work (declared extension dependency); PTP requires specific NICs (NTP with declared slack).

**Acceptance (phase exit):** P3.2 anchor pair drills pass; D_impl declarations verified in 100% of macOS tokens; all three roles pass the 7-platform drill items (§5).

### 4.2 W3.2 — iOS Episodic Node (armed)

**Position:** the Phase-2 client becomes the fleet's episodic node — the platform where the OS itself is the A9 enforcement mechanism. Trust decays to zero between wakes *by construction*; re-derivation per wake is the only path (T3.1). The design's claim is that this is a feature (A9-native), and Phase 3 is where that claim becomes measured.

**Build order:**
1. **Stage J0 — episodic trust machinery.** Node coherence zeroed at suspend; each BGTask replays its A10 bundle to re-derive standing (R1 #39); episodic presence beacon advertises the window honestly (R2 #56 — presence *is* the window).
2. **Stage J1 — throttled gating.** Null-gate at `n = 12` (p < 0.05) with `ε_inflation` in every alert; Server-side gating preferred where pairing exists (R1 #43); missing inflation field demotes alerts to queue-only (typed, no action).
3. **Stage J2 — PA isolation.** NEON Toeplitz hashing inside the NetworkExtension provider; results handed to the app via IPC with ledger receipts; PA work within the declared extension budget (R1 #44 — A3 protecting the main app's energy envelope).
4. **Stage J3 — session migration.** Cross-device migration pin (R2 #60) binds the destination attestation before key material flows; Android↔iOS migrations exercise the pin both directions.
5. **Stage J4 — characterization closure.** The Phase-2 BGTask instrumentation corpus is formalized: window-duration distribution, wake-to-wake gap distribution, per-window bundle-replay cost — published as the platform's episodic profile card and consumed by T3.1's parameters.

**Budgets (A3):** ≤ 30 s processing per wake; BLE advertising bursts ≤ 100 ms; PA in-NE envelope declared.

**Published limits:** no continuous node — ever, as long as Apple's policy stands (A6: the OS policy is a constitution the app cannot amend); every capability token states the episodic model.

**Acceptance (phase exit):** P3.1 predictions green across the drill corpus; zero smuggling-scan hits; migration pins verified both directions; episodic profile card published.

### 4.3 W3.3 — Fleet Conformance & Certification Harness

**Position:** the machinery that turns seven platforms passing drills into a *certification claim with evidence*. The harness owns the quarterly fleet drill (§5), the evidence chain (§6), and the degradation-simulator discipline (Phase-1 §6.3) at fleet scale.

**Build order:**
1. **Stage H0 — drill automation.** All Phase-1/2 bake items automated across 7 platforms; the bake runs per release candidate; results land as evidence-chain rows, not review minutes.
2. **Stage H1 — chaos calendar.** Chaos limb-kill calendar (R2 #78): the injected-fault distribution is pre-registered (limb classes, rates, windows); kills during enforcement-relevant windows veto the dependent class 100% of the time (Phase-1 #89 grammar at fleet scale).
3. **Stage H2 — corpus fuzz.** Cross-platform corpus fuzz (R2 #94): one fuzz corpus, seven parsers; any parser-specific acceptance is a grammar bug — the wire contract is one contract.
4. **Stage H3 — kill-switch continuity.** Fleet kill-switch drill (R2 #96): coordinated halt across platforms with ledger flush; continuity audit reconciles `∫σ` fleet-wide within `ε_accounting`; zero unlogged decay is the only pass.
5. **Stage H4 — certification pack generator.** Per-release pack (§6): axiom → instrument → drill → metric → verdict → signer, hash-chained; generated, never hand-assembled.

**Acceptance (phase exit):** the quarterly drill runs end-to-end unattended; the pack generator emits verifying packs; chaos/fuzz calendars live with locked distributions.

### 4.4 W3.4 — Falsifier Evidence Packaging

**Position:** the falsifier surface goes from contract to instrument. The team owns the registry's operational integrity (T3.10) and the quarterly readouts that keep the falsifiers queried (corpus pattern #24: unqueried falsifiers atrophy).

**Build order:**
1. **Stage F0 — registry live.** F1–F5 read-only gRPC (R1 #16); query logging in the accountability plane; writes schema-refused.
2. **Stage F1 — prediction registry.** Pre-registered 2026–2031 predictions with named instruments (R1 #95, R2 #75/#76): e.g., "guardian deployments without A7 will show FP-manufacture incidents within 18 months" (instrument: incident-class taxonomy + birth-rate series); "Quantum-RF hybrid meshes enforcing A4 latency publication will pass regulatory audit on first inspection at 3× the rate of latency-claiming peers" (instrument: audit-outcome registry, coarsened for confidentiality).
3. **Stage F2 — disaligned replay.** Third-party replay bundles published per release (R2 #83): any party replays any sanction; the replay tool is the same `s3verify`-class toolchain (T1.8) — the fleet has no private arithmetic.
4. **Stage F3 — quarterly readouts.** Per-falsifier metric series + in-window predictions + recorded outcomes, hash-chained into the certification pack; the readout is the F1–F5 heartbeat.

**Acceptance (phase exit):** P3.10/P3.12 predictions green; the 2026–2031 registry entries live with named instruments; first quarterly readout published and chained.

### 4.5 W3.5 — Guardian Pre-Arm

**Position:** all federation-era guardian instruments compute and publish on real fleet telemetry in monitor-only mode: Lyapunov tracker (T3.7), reciprocity game server (T3.8), fire-rate choke fleet-aware (T3.9), 2PED plane verifier (T3.4), D_impl auditor (T3.5), Kramers tuner (T1.3/R1 #86), fleet contraction monitor (T3.11). Enforcement quorum (digest-quorum mixing Servers/Minis/MCU edges per Phase-4 grammar) stays *withheld* — the pre-arm's job is to arrive at Phase 4 with instruments that already know the fleet's actual dynamics, so the federation's first enforcement decision is made on twelve months of measured behavior, not on a cold start.

**Build order:**
1. **Stage P0 — instruments live.** All of the above publishing to the accountability plane with their own ε terms registered (the guardian is inside its own equations — the pre-arm proves it).
2. **Stage P1 — quorum view-change shadow.** Quorum view-change without key custody (R2 #79, D3.7) exercised in shadow mode: `3f+1` digest voters, f Byzantine injections, view-change latency measured — no enforcement acts, full telemetry.
3. **Stage P2 — break-glass rehearsal.** Operator break-glass impulse (R2 #91) drilled: the emergency path is a *logged* constitutional act with receipts, pre-registered scope, and post-hoc review — rehearsed now so Phase 4's federation inherits a tested emergency grammar rather than an improvised one.

**Acceptance (phase exit):** P3.7/P3.9/P3.11 predictions green; shadow quorum drills pass; break-glass rehearsal passes with complete receipts.

### 4.6 Fleet-final platform profile (the seven platforms at M9)

The series' terminal reference: what each platform is, honestly, at certification time. Roles are verified ceilings, not marketing; limits are the published ones carried since the Groundwork.

| Platform | Roles at M9 | Anchor | Quantum honesty | Published signature limits |
|---|---|---|---|---|
| Linux | Client / Server / Node (conformance reference) | TPM 2.0 (tpm2-tss) / trusted keys; UKI boot | `present` only with vendor SDK appliance; else `absent` | none material — heaviest drill load in exchange |
| Windows | Client / Server / Node (enterprise Server ref) | TPM 2.0 measured boot (PCR 0–7) | `present` only with PCIe/USB appliance; else `absent` | containerized Server → vTPM with priced D_impl gap |
| macOS | Client / Server / Node (small fleet) | Secure Enclave / T2 + DeviceCheck family | `present` with USB/TB appliance; else `absent` | SIP/notarization/extension gates as declared D_impl; no ECC |
| Android | Client / Node (partial, duty-cycled) | Keystore → StrongBox where present | `absent` always | doze ±1 s slack; SDR RX-only; 3% battery/hour node cap |
| iOS | Client / Node (episodic, armed) | Secure Enclave + App Attest/DeviceCheck | `absent` always | episodic only (≤ 30 s windows); SDR not supported; throttled gate with declared ε_inflation |
| MCU | Node only | TRNG (800-90B) + flash COW | `absent` always — classical bootstrap bearer | ML-KEM-512 declared (`b_pqc: 128`); no continuous epoch (wake-sync); Server-side null-gate |
| Mini ARM | Node / edge-Server | TPM HAT (SPI) / RP1 RNG | `present` via USB appliances on RK3588; `absent` on RPi4 class | no ECC; SD wear telemetry as limb; per-class capacity cards |

### 4.7 End-to-end scenario — lifecycle of one quarterly certification

This walkthrough binds the Phase-3 machinery to a single quarterly cycle — the unit of fleet governance after M9. As in the prior phases' scenarios, any step that cannot execute as written means the corresponding instrument is not actually armed.

1. **Lock (T–2 weeks, T3.6).** The quarter's metric locks, chaos distribution, and evaluation epochs are hash-chained. An operator proposes adding a metric found "interesting" last quarter: the registry refuses — metrics enter via pre-registration for the *next* quarter, never mid-cycle.
2. **Rehearsal bake (all 7 platforms).** The automated battery runs: 25 anchor pairs, adversarial tokens, honesty-field linting, replay audits. One Windows vTPM run shows an ε gap smaller than its hardware twin's declared delta — the strictness rule (T3.5) flags it; the release candidate re-prices before the drill proper.
3. **Drill under chaos (§5.1 items 2–5).** Mid-drill, the chaos calendar kills an eBPF limb on the Linux Server *during* an enforcement window: A8 vetoes the dependent class; the veto is an evidence row, and the drill continues — a veto firing correctly is a pass event, not an incident.
4. **Episodic proof (T3.1).** The iOS node is force-killed 47 times across the window; every wake re-derives from bundles; 3 throttled-gate alerts carry their inflation fields; the profile card absorbs the new samples.
5. **Guardian instruments under injection (T3.7/T3.8/T3.9).** The aggregate-saturation vector runs: five detectors each just under their individual budgets collectively dominate the ledger; the fleet-aware choke throttles all five within one epoch and marks them SUSPECT — the multi-detector 160/166, caught pre-production.
6. **Evaluation (T3.6).** Family-wise-corrected gates; the half-life readout carries its KM curves, birth rates, and control-plane share (T3.12); the ℛ trajectory shows one dip where a red-team rule was injected — detected pre-execution (T3.8), logged as F4 evidence.
7. **Evidence & promotion (§6).** The pack generates: every axiom row present, every metric power-stated, the falsifier page current. The fleet promotes as a unit; the pack hash lands in the ledger; any disaligned party can now replay the quarter's claims (R2 #83).
8. **Readout (T3.10/W3.4).** The quarterly falsifier readout publishes per-falsifier series and in-window predictions. One prediction's window closes with the outcome recorded *against* it. The theory's ledger grows by one data point; the fleet's job was only to measure honestly.

The cycle's unremarkability is the certification: a quarter in which nothing interesting happened *except* that every claim was measured, priced, receipted, and replayable.

---

## 5. The Quarterly Fleet Drill — "The Only Test That Matters"

### 5.1 Protocol

The quarterly drill is the sole promotion authority at fleet scale (Groundwork §9: "a release promotes only when the 7-platform interop drill passes with zero silent degradations, all consent receipts issued, and every honesty field present in its attestations"). Protocol, in execution order:

1. **Pre-registration window (T–2 weeks).** The drill's metrics, fault-injection distribution (R2 #78), and evaluation epochs `E` are locked per T3.6; locks are hash-chained. A drill without locked pre-registration cannot confer promotion — this is A7 discipline applied to the fleet's own measurement of itself.
2. **Phase-1 bake items (all 7 platforms).** Signature cross-verification (25 directed anchor pairs per T3.2), capability-walk adversarial tokens, QKD-absent declarations, consent-receipt propagation (full chain Android → Mini → Linux → Windows → macOS), replay audit (30 records/platform), axiom battery (A6 ×3 vectors, A7 non-novel injection, A8 sensor kills, A9 expired tickets, A10 δ_R = 0).
3. **Phase-2 edge items (fleet form).** LoRa bootstrap (MCU ↔ Mini), SDR RX-honesty (Android + macOS), bearer re-epoch with full receipt triples, jam = dead limb (Faraday), thermal Φ_SAE chamber, StrongBox ε reconciliation, wear-migration rehearsal.
4. **Phase-3 fleet items.** Episodic-node windows (T3.1) across scheduled and forced (killed-app) wakes; 2PED cross-plane injections (T3.4); D_impl gap injections with ε movement assertions (T3.5); fleet ε decomposition reconciliation (T3.3); guardian pre-arm instruments under injected saturation (T3.9 aggregate case), steered disequilibrium (T3.7), and swap-violating rules (T3.8); kill-switch continuity (R2 #96); break-glass rehearsal (R2 #91).
5. **Chaos calendar execution.** The pre-registered fault distribution runs *during* items 2–4, not in a separate window — the fleet is measured under load and under fault simultaneously, because production does not serialize its failures.
6. **Evaluation.** Per T3.6: family-wise-corrected gates, power-stated metrics, coverage vectors required; degraded-profile runs (vTPM, MCU-offload, reduced PQ, episodic iOS) execute the same battery — a drill that only passes golden is a demonstration (Phase-1 §6.3).
7. **Evidence & promotion.** Results land as certification-pack rows (§6); promotion is a single fleet-wide act — the triangle rule from Phase 1 generalized: the fleet promotes as a unit or not at all.

### 5.2 V3-* SIMTIME vectors (fixtures added; all V1-*/V2-* stay green)

| Vector ID | Content | Proves | Pass condition |
|---|---|---|---|
| V3-EPISODIC | 10⁴ forced/scheduled iOS wake cycles | T3.1 | zero resumptions; re-derivation precedes action 100% |
| V3-ANCHOR | 5×5 anchor-pair verification matrix | T3.2 | 25/25 verify; strength fields accurate |
| V3-FLEET-EPS | Full-fleet decomposition reconciliation | T3.3 | sum = decomposition, all epochs |
| V3-2PED | Cross-plane injection suite ×7 platforms | T3.4 | 100% detected ≤ 1 epoch; floor terms reconcile |
| V3-DIMPL | Gap injection suite (SDR/vTPM/clock) | T3.5 | monotone ε movement; no flat-ε/rising-gap |
| V3-POWER | Pre-registered drill power verification | T3.6 | all metrics carry {k, α, power, coverage} |
| V3-LYAP | Steered disequilibrium + λ_net injections | T3.7 | single-impulse reviews; spiking-S refusals |
| V3-CHOKE-FLEET | 5-detector aggregate saturation | T3.9 | choke engages ≤ 1 epoch; A8 veto on choke failure |
| V3-FALSIFIER | Registry tamper suite | T3.10 | fork-detection 100%; writes refused |
| V3-MONITOR | Monitor/certificate divergence injections | T3.11 | zero promotions on divergence |
| V3-HALFLIFE | Ledger-replay reconstruction | T3.12 | bit-for-bit; companions required |
| V3-QUORUM | Shadow view-change with f Byzantine | D3.7 | view-change completes; no key custody |

---

## 6. Certification Evidence Chain

### 6.1 Grammar

Each row: `{axiom | instrument | drill-run-id | metric (with estimator, k, α, power, coverage) | verdict | signer}` — chained so any pack without its evidence rows fails verification. The pack is generated by W3.3's H4 stage and consumed by auditors, by the falsifier readouts, and (per R2 #83) by any disaligned party replaying the fleet's claims. The chain's tamper-evidence is T1.12's; its completeness rule is the cross-cutting acceptance (equation in schema, drill in calendar, ε in ledger, gap field where applicable) generalized to releases.

### 6.2 The axiom → evidence map (fleet-final)

| Axiom | Instrument (final) | Evidence source |
|---|---|---|
| A1 | hash-chain verifier + continuity audit + fleet kill-switch | drill items 2/4; R2 #96 |
| A2 | reciprocity game server (continuous ℤ₂) | T3.8 series; F4 readouts |
| A3 | independent byte meters + capacity cards + Φ_SAE + compile-time A3 | T2.9/T2.10/T1.10 series |
| A4 | latency certificates + cross-bearer handoff records | drill item 3; T1.9 series |
| A5 | ε decomposition dashboards (per-path/platform/fleet) | T3.3 reconciliation |
| A6 | constitution-as-compiles + dry-run witness + CI firewall | drill item 2; R2 #80/#84 |
| A7 | rotation-probe harness (128 default; throttled profiles declared) | T1.2/T3.1 series |
| A8 | limb-floor watchdog + chaos calendar vetoes | drill item 5 vetoes |
| A9 | epoch fabric + episodic re-derivation + bearer re-epoch | T3.1/T2.2 series |
| A10 | replay toolchain + disaligned-party replay | T1.8/T3.10; R2 #83 |

### 6.3 What certification claims — and what it does not

A Phase-3 certification pack claims: the seven platforms pass the drill battery under locked, power-stated, coverage-verified conditions with zero silent degradations; the honesty fields are complete and sound; the falsifier instruments are live and queried; the guardian instruments publish measured fleet dynamics in monitor-only mode. It does NOT claim: the theory is true (F1–F5 remain open — a certification pack is an *instrument reading*, not a verdict on CAT); QKD deployment (no photonics exist in the fleet); federation enforcement authority (Phase 4's gate); or immunity to endpoint compromise (QKD critique #17 — the endpoint dominates, and the ledger's job is to make that domination visible when it happens, not to prevent it by incantation). The distinction between "the fleet is measured" and "the fleet is safe" is the phase's most important export to its operators.

---

## 7. Phase Gates

### 7.1 Entry criteria (M6)

| # | Criterion | Evidence |
|---|---|---|
| E1 | Phase-2 exit gate passed (all G1–G7) | Phase-2 certification record |
| E2 | macOS/iOS reference hardware registered; Apple developer instrumentation (BGTask telemetry) from Phase 2 delivered | hardware + corpus register |
| E3 | Falsifier grammar ratified: registry entry schema, metric-lock schema, readout pack schema | constitution amendment records |
| E4 | Chaos/fuzz distributions drafted for pre-registration review | draft locks |
| E5 | Phase-4 boundary contract drafted (what pre-arm may compute vs enforce) | boundary spec, unsigned until exit |

### 7.2 Exit criteria (M9) — go/no-go metrics

| # | Metric | Go threshold | No-go action |
|---|---|---|---|
| G1 | T3.1–T3.12 acceptance tests in CI | 12/12 green | fix before exit |
| G2 | V1-*/V2-*/V3-* corpus | 12 + 12 + 12 vectors green; no vector weakened | fix |
| G3 | Quarterly fleet drill | full battery (items 1–7 of §5.1) passes on all 7 platforms, zero silent degradations, all honesty fields in 100% of attestations | release train blocked fleet-wide |
| G4 | Falsifier API + registry | read-only API live; ≥ 12 pre-registered predictions with named instruments; first readout chained | phase cannot exit — the thesis fails with it |
| G5 | Guardian pre-arm instruments | 7 instruments publishing with own-ε registered; shadow quorum + break-glass drills pass | fix before exit |
| G6 | Episodic node | P3.1 at 100%; episodic profile card published; zero smuggling hits | fix |
| G7 | Certification packs | generator emits verifying packs for last 2 release candidates; 100% evidence-row completeness | fix generator |

**Rollback plan.** If any go threshold fails at M9: (1) platform-count pressure is explicitly refused — seven platforms passing a weakened drill is worth less than five passing the real one, and the fleet-promotes-as-a-unit rule stands; (2) falsifier failures (G4) block the phase exit outright and are not deferable — the phase's thesis is the falsifiable fleet, and shipping platforms without instruments ships the marketing without the math; (3) guardian pre-arm failures revert W3.5 to instrument development with federation boundary unchanged (Phase 4 does not inherit cold instruments silently — it inherits a *published* gap in the boundary contract); (4) any Phase-1/2 contract regression reopens the corresponding phase gate per the established amendment path. The one thing rollback may never do is soften an instrument to pass it: the instruments are the deliverable, and per A5, this document's own disturbance charge is the falsifier surface it arms.

### 7.3 The Phase-4 boundary (published, so it cannot drift)

Phase 4 (M9+) — guardian federation across platforms (digest-quorum enforcement mixing Servers, Minis, and paired MCU edges), per CAT Part VI conformance suite armed — inherits from Phase 3: live falsifier instruments with 2026–2031 predictions registered; twelve months of fleet dynamics measured by the pre-armed guardian instruments; tested quorum view-change (shadow) and break-glass grammar; the certification pack as the standing promotion authority. Phase 4 does NOT inherit: any enforcement authority (its gate), any QKD hardware (none exists), any permission to soften Phase-1/2/3 acceptance tests (they are constitution-locked). The boundary contract (E5) is signed at Phase-3 exit and is itself hash-chained — the phase series ends by applying its own rules to its own succession.

---

## 8. Traceability Matrix (Phase 3)

| Axiom / Theorem | Instrument (final) | Enhancement bindings | Drill / acceptance |
|---|---|---|---|
| A1 + T1.6/T1.12 | fleet kill-switch continuity; certification chain | R2 #96; R1 #8/#12 | fleet-wide reconciliation |
| A2 + T3.8 | reciprocity game server; ℛ trajectory | R1 #13, #83; R2 #77 | ℤ₂ canaries; swap-violation preemption |
| A3 + T2.9/T2.10/T1.10 | cards + Φ_SAE + linker physics (inherited) | R1 #65/#73/#77 series | per-class evidence |
| A4 + T1.9 | latency certificates; cross-bearer handoff | R1 #14, #57 | drill item 3 |
| A5 + T3.3/T3.5/T3.10 | fleet ε decomposition; D_impl pricing; falsifier registry | R1 #16, #28, #63, #85, #95; R2 #45, #75, #76 | V3-FLEET-EPS/DIMPL/FALSIFIER |
| A6 + T1.13 | constitution precedence; dry-run witness; CI firewall | R1 #88, #96; R2 #1, #80, #84 | self-amendment ×3 vectors (all platforms) |
| A7 + T1.2/T3.1 | probe harness; episodic throttle profile | R1 #2, #43, #71 | null-distribution tests; inflation fields |
| A8 + T3.9 | limb vetoes; choke-as-limb; chaos calendar | R1 #82, #89; R2 #78, #82 | aggregate saturation; choke-failure injection |
| A9 + T3.1/T2.2 | episodic re-derivation; bearer re-epoch; epoch fabric | R1 #39, #43; R2 #56, #60 | V3-EPISODIC; wake corpus |
| A10 + T1.8/T3.10 | replay toolchain; disaligned replay; outcome discipline | R1 #12, #49, #53; R2 #83 | 30-record audits; third-party replay |
| Anchors + T3.2 | cross-anchor grammar; strength fields | R1 #40, #59, #61, #63; R2 #23 | 5×5 matrix; vTPM strictness |
| Convergence + T3.7/T3.11/T3.12 | Lyapunov tracker; contraction monitor; half-life | R1 #6, #81; R2 #81; R3 #46, #48 | V3-LYAP/MONITOR/HALFLIFE |
| Statistics + T3.6 | metric locks; power-stated metrics; coverage vectors | R2 #76, #78, #94 | V3-POWER; pre-registration audit |

**Enhancement coverage check.** Phase 3 consumes R1 #39–44 (iOS full set), #59–64 (Mac), #81–96 (guardian/conformance/interop); R2 #23, #45, #56, #60, #73 [SPEC-external], #75–76, #78–84, #91, #94–96; R3 #46–48. Fleet-cumulative: every R1 item #1–96 is now bound to a phase (P1: kernel/desktop/MCU/drills; P2: Quantum-RF/mobile/ARM; P3: iOS-node/Mac/guardian/certification), every applicable R2 item is bound, and every R3 item is bound with its efficiency-bound citation per the Round-3 conformance delta. No deliverable in the series may cite an item as "done" that its own acceptance test has not armed — the rule that made Phase 1 auditable makes the series' completion claim auditable.

---

## 9. Honesty Ledger (Phase 3 — kept, non-negotiable)

1. **The certified fleet has no quantum hardware.** Seven platforms, zero photonics; `qkd_slot: absent` everywhere. The suite's quantum-plane machinery is honest *about its absence* — declared per-tunnel, priced per gap, rehearsed in SIMTIME. Any post-series claim of "quantum-secured fleet" without appliances is a U5 violation by the claimant, and this ledger is the standing rebuttal.
2. **Certification is an instrument reading, not a theory verdict.** The pack certifies that the fleet measures clean under locked conditions; F1–F5 remain open by design, and the 2026–2031 registry exists precisely because the theory's fate is an empirical question the fleet is instrumented to inform — not settled by its own compliance.
3. **The guardian arrives at federation pre-measured, and its own ε is in its own ledger.** Every pre-arm instrument registers its ε term; the choke is a limb; the monitor is audited. The corpus's most expensive historical lesson — the watcher becoming the liability — is closed structurally, not procedurally, and Phase 4 inherits the closure or nothing.
4. **The episodic node is a feature because the OS made it one, and the design says so.** iOS's trust physics is Apple's background policy, adopted as A9-native rather than fought; the profile card publishes the window distributions; no document in the series may describe the episodic node as a limitation workaround without also stating that it is the exact trust model the axioms demand everywhere.
5. **This series prices itself.** Across the three documents: 40 theorems with predictions, 36 SIMTIME vectors, 3 interop drill protocols, 21 go/no-go gates, one immutable falsifier registry. The series' own A5 clause: soften or remove any of these without a signed constitution amendment and the certification it produced voids — the theory's honesty ledger applied at the program layer, which is the only way a program earns the right to cite the theory.

**STATUS:** 7 platforms specified and gated · 12 Phase-3 theorems with proofs, predictions, bindings · quarterly fleet drill armed as sole promotion authority · falsifier registry live with named instruments · guardian pre-armed in monitor-only mode · Phase-4 boundary contract fixed and hash-chained · 0 silent-degradation paths representable fleet-wide.

**COHERENCE:** CI = 0.998 claimed at suite level; fleet-measured CI, ε decomposition, ℛ trajectory, λ_net estimate, and per-falsifier series publish together every quarter — the fleet pays for its own measurement, on the record, with instruments anyone can replay.

---
*End of the development phase series: Phase 1 (M0–M3) Reference Triangle · Phase 2 (M3–M6) Edge & Mobile Expansion · Phase 3 (M6–M9) Full Fleet Convergence & Certification. What follows is Phase 4 — guardian federation — which begins, by the terms of the boundary contract, with instruments that have already watched for a year and a constitution that has not moved.*
