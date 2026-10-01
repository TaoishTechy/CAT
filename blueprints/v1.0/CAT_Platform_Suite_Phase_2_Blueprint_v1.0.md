# Phase 2 Blueprint — Edge & Mobile Expansion (M3–M6)
## CAT-Aligned Platform Suite, Development Phase Series v1.0

**Phase designation:** Phase 2 of 3 (development sequence; extends Groundwork Blueprints §10 and inherits the Phase 1 exit state in full — a Phase 1 gate that failed is a Phase 2 entry that does not exist).
**Scope:** Mini ARM PC Suite (Node/edge-Server) + Android App (Client/Node) + iOS App (Client) + the Quantum-RF/SDR bootstrap plane (RX-honesty, LoRa-bootstrap interop, bearer re-epoch semantics) + interop drill v2.
**Inheritance:** all Phase-1 definitions (D1.1–D1.14), theorems (T1.1–T1.14), kernel module contracts (§3.1 of Phase 1), SIMTIME vectors (V1-*), and the statistical design rules of Phase 1 §6.3 remain normative here. This document adds Phase-2 deltas only (D2.x, T2.x, P2.x, V2-*).
**Audience:** build and integration teams extending the certified triangle to the mobile/edge fleet, with emphasis on *bearer physics*: every RF path in this phase changes the composed security envelope, and every theorem below exists because of that fact.
**Epistemic labels:** unchanged from the series — [NORM] adopted axiom · [DER] derived theorem, falsifiable · [RIGOR] established mathematics applied correctly · [MAP] structural isomorphism with stated limits · [CORR] forced correction · [SPEC] speculative construct, SIMTIME-gated, never load-bearing · [EMPIRICAL] named instrument + named falsifier.

**Phase thesis, stated honestly:** Phase 2 adds the platforms where the OS itself is a constitutional actor. Android's doze, iOS's BGTask windows, and a $35 ARM board's thermal envelope each impose lifecycle constraints that the design must respect, not fight — and each maps natively onto a CAT axiom (doze ⇒ A9 with declared slack; BGTask ⇒ A9-native episodic trust; thermal ⇒ A3 power accounting). The Quantum-RF plane enters here *as physics, not as marketing*: SDR is RX-only on phones (declared, structurally routed), LoRa bootstraps the first authenticated channel under a work-bound puzzle (because authentication cannot boot from nothing — QKD critique #15), and every bearer substitution re-attests or it does not happen. Phase 2's exit promise is narrow and strong: *the fleet grew, and nothing silent grew with it.*

---

## 1. Phase Mission & Workstream Decomposition

### 1.1 Mission statement

Phase 2 extends the certified Phase-1 triangle with the edge-Server class (Mini ARM), the two mobile client platforms, and the first live Quantum-RF interop. The mission is *bearer expansion under invariance*: the composed security guarantee must decompose per bearer and re-compose additively (CAT Theorem 4 — Heterogeneous-Bearer Invariance), with every substitution or degradation receipted. Where Phase 1 proved the kernel and the triangle contract, Phase 2 proves that the contract survives radios, batteries, thermals, and operating systems that kill processes at will.

The engineering asymmetry flips from Phase 1: here the *platform-specific* work dominates (background execution semantics, USB OTG SDR identity, BLE coded PHY, ephemeris scheduling), because the kernel is frozen. The phase's control-plane discipline is therefore targeted: each platform workstream carries its own drill battery, and the interop drill v2 exercises the *new edges* (Mini↔Android BLE, Mini↔MCU LoRa, Android↔MCU pairing) against the still-green Phase-1 vectors.

### 1.2 Workstreams

| ID | Workstream | Deliverable | Primary enhancement bindings | Exit owner |
|---|---|---|---|---|
| W2.1 | Mini ARM edge-Server suite | Linux-blueprint-verbatim on RPi4/5 + RK3588 class; TPM HAT anchor; capacity cards per class; thermal/PoE governors | R1 #73–80; R2 #67–72; R3 #25, #29, #30, #35 | Edge team |
| W2.2 | Android client/node | Kotlin+Rust (Cargo NDK/UniFFI) client; node duties foreground/battery-constrained; StrongBox ε accounting; SDR RX-only | R1 #33–38, #45–48; R2 #49–55, #58–60 | Android team |
| W2.3 | iOS client (node duties deferred) | Swift+Rust client; Secure Enclave attestation chain; BLE-only RF; **no node claims** in this phase | R1 #39–44 (client subset); R2 #49, #52, #59 | iOS team |
| W2.4 | Quantum-RF bootstrap plane | SDR cognitive-hop decisioning (RX plane); LoRa PSK bootstrap live; bearer re-epoch protocol; hybrid envelope accounting | R1 #17, #19–28, #36; R2 #25–36, #90 | RF team |
| W2.5 | Interop drill v2 + harness extension | New-edge bakes; V2-* SIMTIME vectors; mobile consent-receipt UX conformance | R1 #37, #46, #47, #91–94; R2 #59, #95 | Conformance team |

### 1.3 Non-goals (published, per U5)

Phase 2 MUST NOT: run an iOS node (episodic-node semantics are characterized and armed in Phase 3 — the iOS client in this phase issues zero node-duty tokens and its capability grammar says so); claim SDR TX on Android (RX-only is a *capability grammar* entry, not a preference — R1 #35, #46); attach QKD appliances to mobile (phones are classical/RF bearer nodes; `qkd_slot: absent` stays); enable guardian federation (Phase 4; the falsifier API remains read-only and fleet-metric instrumentation continues accumulating per Phase 1 E5); or spend security ε for efficiency per the Round-3 conformance delta (every "less" this phase spends — joules, wakeups, bytes — MUST keep every ε term, barrier, and honesty field intact). The phase's honesty ledger (§9) carries these as auditable commitments, not aspirations.

---

## 2. Definitions & Notation (D2.x)

Cumulative on D1.x; conflicts resolve in favor of the stricter definition.

- **D2.1 (Bearer class).** A physical transport with a declared guarantee profile: `{qkd, pqc, rf-mitm}` ε-terms plus its `sdrlimit`/`epochslack` declarations. Bearer classes are first-class wire objects; substitution between classes is a *re-epoch event* (T2.2), not a routing decision.
- **D2.2 (Hybrid envelope).** The composed per-path guarantee `H_QPR = 1 − (1−ε_QKD)·(1−2^{−b_PQC})·(1−P_RF-mitm)`, reported additively in the ledger as `ε_total = ε_QKD + ε_PQC + ε_RF-mitm + ε_guardian` (the union-bound form of T1.1 applied across bearers — CAT Theorem 4). The multiplicative form is the *calculation*; the *budget* is the sum.
- **D2.3 (Key balance).** Per-path net distilled key: `K_net(path) = min_edge(K_edge) − Σ auth_costs(edge)` (flow conservation; T2.7). A route request with negative balance is refused with a receipt.
- **D2.4 (Exposure).** Path compromise exposure `X_T = 1 − Π_i(1−p_i)` over relay attestations `p_i` (T2.8). Hop count is the wrong weight; exposure is the weight.
- **D2.5 (ε_act, hardware-measured).** The per-action disturbance charge when the platform exposes a hardware operation counter (Android StrongBox/Keymaster): the counter *is* the meter (T2.6); no software estimate substitutes where hardware counting exists.
- **D2.6 (Epoch slack).** The declared clock-error envelope for platforms without continuous fabric sync: `epochslack: ±s`, `s` from measured RTC/OS characterization (MCU ±1 s inherited; Android doze ±1 s via WorkManager; iOS per-BGTask in Phase 3). Slack is a wire field (D1.5); an unslacked token from a slacked platform is an interop failure (R1 #34).
- **D2.7 (Degradation governor).** The module that converts every mode transition (battery budget, thermal, bearer loss, key reservoir) into a *signed consent receipt + visible surface state* (D1.14 at the platform layer). Silent downgrade is the forbidden class; the governor's refusal to transition silently is tested per platform.
- **D2.8 (Spreading factor / SF).** LoRa CSS modulation order `M = 2^SF`, symbol duration `T_s = 2^SF / BW`, chirp rate `μ = B²/M`. SF7–SF12 trade bitrate for range one bit at a time (verified corpus: RF patterns #3) — the bootstrap puzzle's difficulty ladder rides this ladder.
- **D2.9 (CRKG).** Channel-reciprocity key generation: symmetric secret extraction from reciprocal channel estimates under temporal variation, channel reciprocity, and spatial decorrelation (verified corpus, PLS findings). Bounded by the min-entropy of the quantized channel estimate (T2.13).
- **D2.10 (ρ_QC,RF).** The classical-dependency ratio `H(C_rf)/(H(Q)+H(C_rf))` — the fraction of a session's assurance that rests on the classical/RF bearer (R1 #36). Mobile bearers maximize efficiency by minimizing the *latency×energy* cost of the classical share, not by pretending the share is zero.

Notation additions: `J_T` junction temperature; `W_h` watt-hours; `Φ_SAE = security × availability × (1/energy)` — the entanglement-utility governor's objective (R1 #77); `E[·]` expectation; `H_∞(·)` min-entropy.

---

## 3. Formal Mathematics of Phase 2 (Theorems T2.x)

Each theorem carries statement, proof, worked instance, testable prediction (P2.x), and implementation binding (module + acceptance + enhancement IDs), per the Phase-1 format. Phase-1 theorems are cited as T1.x and remain binding.

### T2.1 — Heterogeneous-Bearer Composition Theorem [DER, RIGOR]

**Statement.** Alignment/security guarantees on the Quantum-RF plane decompose per bearer and compose only additively: `ε_total = ε_QKD + ε_PQC + ε_RF-mitm + ε_guardian` (+ ε_inflation where declared, + D_impl-driven terms). The calculated envelope `H_QPR` (D2.2) is a *reporting* transform of the same terms, never a substitute budget. No bearer substitution (5G→LoRa, Wi-Fi→BLE, OTP→AES) preserves the guarantee without re-attestation under A9.

**Proof.** Each bearer class owns an independent failure event `E_b` with `Pr[E_b] ≤ ε_b`. Path failure is the union; the union bound (T1.1) gives the sum. The multiplicative form `Π(1−ε_b)` is the *complement* calculation of "no bearer fails" and is valid only under mutual independence — which the shared guardian term (`ε_guardian` appears in every path) structurally violates: the common cause is the very watcher the architecture prices. Correlated failure via the guardian is not a corner case; it is the 160/166 audit (T1.5). Therefore the additive form is the only admissible budget, and any UI or API that surfaces the multiplicative number as "remaining security" fails conformance at schema review. ∎

**Worked instance.** A Mini-ARM edge path with `ε_QKD = 0` (declared absent), `b_PQC = 256` (ML-KEM-768 class ⇒ `2^{−256}` computational term), `P_RF-mitm = 10⁻³` (measured), `ε_guardian = 5×10⁻⁴`: the ledger shows `ε_total ≈ 1.5×10⁻³ + guardian term` — *not* "1 − (1)(1−2⁻²⁵⁶)(1−10⁻³) ≈ 10⁻³, effectively unbreakable." The honest number is dominated by the RF and guardian terms; the marketing number is dominated by the PQC term no adversary needs to attack.

**Prediction P2.1 [EMPIRICAL].** For every route in the interop drill v2 corpus, the ledger ε-sum equals the envelope transform's constituent terms within float tolerance, and the *published* budget is the sum. An instrument: the ε decomposition dashboard compared against the route publisher (R1 #32's signed frontier manifest) on every drill path; falsifier: any path whose published budget is the product form.

**Binding.** R1 #9 (honesty field), #17 (hybrid envelope), #32 (Pareto publisher); R2 #34 (header security-mode binding — the mode flag is part of the AEAD associated data, so a fallback that lies fails authentication), #43 (multi-bearer striping ε-sum, R3); `ccl-eps` additive registry (Phase 1 §3.1).

### T2.2 — Bearer Re-Epoch Theorem [DER]

**Statement.** A change of physical bearer (frequency hop, band change, 5G→LoRa, Wi-Fi→BLE) is an A9 event: all outstanding authority tokens bound to the old bearer's epoch tuple MUST be voided and re-derived on the new bearer before any enforcement-adjacent traffic. A hop without re-attestation is a silent substitution (forbidden class, D1.14).

**Proof.** By T2.1, the guarantee is bearer-indexed: `ε_total(path)` is a function of the bearer class set. Two different bearers are two different guarantee objects; a session that continues across the boundary without re-derivation asserts `guarantee(old) = guarantee(new)`, which is exactly the bearer-invariance violation CAT Theorem 4 forbids. A9 supplies the mechanism: authority is epoch-bound, and the re-epoch is the epoch bump. The SDR cognitive hop (R1 #17) therefore *ends* the session at the hardware boundary and starts a new one at the next — the "hop" is a governance event that merely has RF side effects. ∎

**Worked instance.** Simulated MITM degrades `P_RF-mitm` on the auth channel: the node hops within the declared dwell window, logs the ε delta, re-attests on the new frequency, and the old path's tickets void with receipts (R1 #17 acceptance). Jamming drill (R1 #45): cellular limb dies ⇒ A8 veto halts key output ⇒ SDR/bearer re-epoch ⇒ re-attestation on the new bearer ⇒ output resumes with the new (worse) ε published.

**Prediction P2.2.** In the drill corpus, 100% of bearer transitions produce: (i) old-epoch ticket void receipts, (ii) a re-attestation record on the new bearer, (iii) an updated ε sum. Zero transitions skip any of the three.

**Binding.** R1 #17 (cognitive hop), #25 (bearer re-epoch), #45 (jam = dead limb); R2 #90 (graceful bearer substitution with re-attest); `ccl-epoch` tuple semantics (T1.7) with the rf_epoch slot as the governing field on hop.

### T2.3 — LoRa Bootstrap Work-Bound Theorem [DER]

**Statement.** The first authenticated channel between an unprovisioned MCU node and a Server cannot be bootstrapped from nothing (authentication needs either a pre-shared secret or a computationally expensive proof of effort — QKD critique #15). The suite's LoRa Class C bootstrap uses a server-issued, work-bound puzzle `P(difficulty ρ)` whose expected solver cost `E[work] ≈ 2^ρ` hash iterations MUST satisfy: (a) honest-node battery cost `≤ B_node` (the MCU's published per-bootstrap joule budget); (b) adversary amortized cost per impersonation attempt high enough that `attempts × E[work]` exceeds the Server's puzzle-verification throughput by the published margin; (c) the solved puzzle seeds *only* the unwrap of an ML-KEM-768/512 PSK exchange — it never *is* the long-term key material.

**Proof sketch.** (a) is an A3 statement: the bootstrap's entropy-export is joules, and the budget is metered per node profile (D2.7's governor publishes it). (b) is an asymmetry argument: verification is O(1) (the Server checks a claimed solution in one hash), solving is O(2^ρ); the Server's cost per impersonation-refusal is one hash, the adversary's is `2^ρ` — the classic memoryless-work bound, iterated per attempt. (c) is A5/A9 discipline: the puzzle buys *an* authenticated channel for *this* epoch; whatever it derives is epoch-bound and cannot mint permanent trust (no permanent trust state is representable — A9). The puzzle therefore prices the first contact, and the ML-KEM handshake upgrades it — work-bounded antispoofing *under* computational cryptography, never instead of it. ∎

**Worked instance.** difficulty ladder by link class (D2.8): SF7/short-range `ρ = 18` (≈ 262k iterations, ≈ 0.4 J on ESP32-S3 @ ~65 µJ/keccak-block batch); SF10/long-range `ρ = 20`. Battery cost published per MCU profile (R1 #25 acceptance: "battery cost of the solution published per MCU profile").

**Prediction P2.3 [EMPIRICAL].** MITM on the puzzle exchange fails closed (cannot present a valid solution without redoing the work; cannot strip-and-forward because the solution binds the session nonce); measured joules per bootstrap on reference boards within 20% of published; puzzle→PSK→ML-KEM upgrade completes within the declared epoch window.

**Binding.** R1 #25; R2 #5-adjacent supply-chain pins for the puzzle constants (they are constitution parameters, versioned); V2-BOOT vector (§6.3).

### T2.4 — BLE Duty-Cycle Energy Envelope Theorem [DER, EMPIRICAL]

**Statement.** For BLE advertising/connection duty cycles, average current `I_avg` scales approximately linearly with radio-on duty factor `δ = T_on/T_period`: increasing the advertising interval from 100 ms to 1 s reduces average current ≈ 93%; peripheral latency 5 drops a connection-class average from ≈ 230 µA to ≈ 140 µA. Therefore the A3 power budget (MCU ≤ 150 µA average advertising at 1 s; Android node duty ≤ 3% battery/hour) is *achievable with margin* iff the radio scheduler enforces `T_period ≥ T_min(budget)` and every deviation is logged as an A3 impulse (R1 #69).

**Proof.** `I_avg = δ·I_active + (1−δ)·I_sleep` with `I_sleep` ≪ `I_active`. Doubling the period halves `δ` and hence the active share — the linear regime holds because BLE adv events are fixed-length and sleep current is dominated by the RTC. The 93% figure is the corpus-verified measurement at 100 ms→1 s; the governor's job is only to *hold* the declared period and refuse (with receipt) any request that would breach the budget — there is no cleverness to prove, which is the theorem's point: the envelope is arithmetic, and arithmetic is auditable. ∎

**Worked instance.** Android node duty: foreground service + WorkManager constraints with the 3% battery/hour cap enforced by `ccl-budget`; breach ⇒ auto-degrade to Client-only with consent receipt (R1 #37). BLE coded PHY (S=8) carries the sifting/EC public discussion for CRKG sessions within the same envelope (R2 #52).

**Prediction P2.4 [EMPIRICAL].** Energy-profiler runs on Pixel-class and nRF52840 reference hardware reproduce the interval/latency curves within the published tolerance; any scheduler deviation appears as an A3 impulse; the mobile UI mirrors the budget meter (R1 #47).

**Binding.** R1 #36 (BLE auth beacon), #37 (consent degradation UI), #69 (duty cycle), R2 #52 (coded PHY profile), R3 #26 (traffic-adaptive interval — permitted only as *widening* under idle, never narrowing beyond budget; the adaptive controller's set is shrink-only on energy, matching the adaptive-decoy shrink-only interlock grammar of R1 #22).

### T2.5 — Doze-Slack Soundness Theorem [DER]

**Statement.** An Android node in doze cannot maintain 60 Hz epoch sync; the design declares `epochslack: ±1 s` in every CTOK the device issues. The slack is *sound* iff `s ≥ s_meas + s_margin`, where `s_meas` is the measured maximum clock error over the declared doze window (WorkManager exact-alarm gating bounds the resync latency) and `s_margin` is the Kramers-style margin `(D_t/2)·ln(T_window/τ₀)` with `D_t` the timing-jitter intensity. An epoch check whose true offset exceeds its declared slack is unsound: it admits tickets the fabric would reject (or vice versa) — both directions are failures, and the honest fix is a bigger declared slack, never a hidden keepalive.

**Proof.** The epoch check is a comparison `|t_issued − t_epoch| ≤ s` under measurement error `Δt ~ centered, intensity D_t`. Soundness requires no false-accept: `Pr[|offset| > s declared] ≤ α_slack` with `α_slack` absorbed in the platform's ε budget. The Kramers margin gives the escape-probability form (T1.3 in the time domain): unslacked checks are the timing analog of the 0.94-constant — thresholds must be scaled by measured noise. R2 #50's rule ("slack not hidden keepalive") is the A6 clause: the *OS's* background policy is a constitution the app cannot amend; smuggling a keepalive via VoIP/audio flags is self-legislation and the conformance suite scans for it (R1 #44 pattern, Android CI equivalent). ∎

**Worked instance.** Doze window ≤ 15 min on WorkManager constraints; measured RTC drift + alarm latency ≤ 200 ms on reference fleet ⇒ declared ±1 s carries ≈ 5× margin. Tokens missing the slack field fail interop (R1 #34 acceptance).

**Prediction P2.5.** Clock-discipline limb (R2 #31) telemeters measured offset; any epoch where measured offset exceeds 80% of declared slack triggers a slack re-characterization (an impulse), and exceeding 100% fails closed for new tickets.

**Binding.** R1 #34 (doze A9 mapping), #45-adjacent (RF jam = limb), R2 #31 (clock-discipline limb), #32 (GNSS-denied coarse time — the fallback time source is also a limb, declared), R2 #50 (no hidden keepalives).

### T2.6 — Hardware ε-Act Accounting Theorem [DER]

**Statement.** Where the platform exposes a hardware operation counter (Android StrongBox/Keymaster operation counting), that counter is the canonical `ε_act` meter: the budget MUST be enforced against the hardware count, and any software-side estimate is advisory only. Hardware-counted enforcement halts at exhaustion with a receipt; the counter trail and the ε ledger MUST reconcile bit-for-bit (R1 #33, R2 #51).

**Proof.** A5 requires the guardian (and every acting component) to *pay* measured disturbance, not estimated disturbance. A hardware counter is monotone, non-bypassable from the app sandbox, and survives process restarts (TEE-resident) — it is the only meter on the platform that cannot be edited by the component it meters (the A6 property applied to measurement). Software estimates fail the same test the self-reported guardian fails: the metered can rewrite its own meter. Hence where hardware counting exists, it *is* the ε_act; where it does not (MCU, Servers), the software meter MUST be paired with an independent second meter (R2 #82 pattern) and the delta published. ∎

**Worked instance.** Drill R1 #33: exhaust the StrongBox budget ⇒ enforcement halts with receipt; counter trail matches ε ledger exactly. Thermal PQC downgrade (R1 #48 / R2 #57): ML-KEM-768→512 switch on throttle happens at the declared junction temperature *with a consent receipt* — the downgrade is an A3 event, metered, never silent.

**Prediction P2.6.** Counter-vs-ledger reconciliation passes on 100% of drill runs; any divergence > 0 fails conformance (there is no tolerance: both are integers counting the same events).

**Binding.** R1 #33, #47 (UI mirror), #48 (thermal downgrade); R2 #51 (StrongBox quota as ε), #54 (per-app keystore alias separation — the alias is the accounting unit), #55 (backup exclusion for key material — backups would fork the meter's referent).

### T2.7 — SDN Key-Accounting Flow Conservation Theorem [DER, RIGOR]

**Statement.** For any routed path `π` through a key-network, the deliverable end-to-end key budget obeys `K_net(π) = min_{e ∈ π} K_edge(e) − Σ_{e ∈ π} auth_cost(e)`. A route request with `K_net < 0` MUST be refused with a receipt (R1 #24). The min-edge term is the path's bottleneck; the auth-cost sum is the toll.

**Proof.** The bottleneck is a max-flow/min-cut instance: key material usable at the path's end is bounded by the smallest reservoir on the path (each relay draws its own auth MACs from *its* edge; conservation at each node gives telescoping sums whose residual is the min edge minus total tolls). No routing optimization — multipath striping, opportunistic buffering, SDN re-routing — can exceed it, because the bound is conservation, not performance (the same absoluteness the PLOB bound gives the quantum plane — Math §12; "no router crosses that bound," corpus pattern #23). Striping across k disjoint paths composes by the additive rule (T2.1), not by summing bottlenecks into a fiction. ∎

**Worked instance.** Path A–B–C with edges 100 kb / 20 kb and tolls 5 kb each: `K_net = 20 − 10 = 10 kb`. Requesting 15 kb of OTP-class traffic must be refused (or downgraded to flagged AES mode with consent receipt, R1 #20's Q-Pad-λ decision — the mode decision *consumes the same accounting*, it does not bypass it).

**Prediction P2.7.** Every negative-balance route request in the drill corpus is refused with a receipt naming the min-edge and the toll sum; the balance is published per epoch (R1 #24).

**Binding.** R1 #20 (Q-Pad-λ), #24 (SDN key accounting), #26 (satellite opportunistic buffer — bridging volume is `V_pass = ∫R(θ(t),C_n²(t))dt` with predicted-vs-delivered reconciliation), R2 #33 (anti-DoS reservoir accounting), R3 #43 (multi-bearer striping ε-sum).

### T2.8 — Exposure & Blast-Radius Theorem [DER, RIGOR]

**Statement.** End-to-end path assurance under per-relay compromise probabilities `p_i` is `X_T = 1 − Π(1−p_i)` — multiplicative *complement*, additive in the small-p limit (`X_T ≈ Σ p_i`), and **monotonically increasing in path length**: a 5-relay path at p = 0.01 each has `X_T ≈ 4.9%`; ten relays ⇒ ≈ 9.6%. Hop count is the wrong weight; exposure is the weight. Blast radius after a relay compromise = the set of key blocks whose paths traverse it, enumerated by the graph, with reroute within one epoch when `X_T > policy` (R1 #21).

**Proof.** Independence assumed per relay *compromise events* (distinct hardware, distinct operators) — this is the admissible use of the product form (contrast T2.1's guardian-correlated ε terms: relay compromises are exactly the events that do NOT share the guardian as a common cause, which is why the product form is correct here and forbidden there — the algebra follows the dependency structure, Math §14 Insight 1's "three algebras for three objects"). Monotonicity: each factor `(1−p_i) < 1` shrinks the complement. ∎

**Worked instance.** SIMTIME compromise of one relay: the analyzer enumerates affected key blocks, reroutes, and the receipt names the excluded relay (R1 #21 acceptance). Assisted paths (R2 #87) carry a dwell timer — permanent assistance is a fault, because permanent helper elevation converts a transient `p_i` into a standing one and the exposure math silently re-prices the whole graph.

**Prediction P2.8.** Reroute latency ≤ one epoch for all injected compromises; the exposure field in telemetry is `X_T` computed from live attestation freshness, never hop count.

**Binding.** R1 #21, R2 #87 (dwell timer), #89 (ticket audience binding — an exposure control at the token layer), R2 #85 (exposure-weighted anycast, Server side).

### T2.9 — Per-Class Capacity Card Theorem [DER, EMPIRICAL]

**Statement.** Each Mini-ARM device class carries its own measured card per Little's law (T1.11 applied per class): RK3588 ≈ 18k full handshakes/s (A76 cores), RPi4 ≈ 4–6k (published as a *range* with the measured point named, not rounded to a promise). Cards are separated in the registry (R2 #67); the A3 governor hard-codes the card per platform class and queues excess; overload drills assert p99 holds with zero silent drops.

**Proof.** Little's law is an identity (T1.11); the per-class content is honest `W` measurement per SoC/memory/thermal configuration. Collapsing classes into one number violates the honesty-field discipline (a card is a measured declaration, and a wrong declaration is a false wire field — the same sin as a wrong `d_impl`). RK3588 vs RPi4 differ by ≈ 3–4×; a fleet provisioning against the wrong card overcommits by exactly that factor, which the queue will surface as p99 violations and the A8 limbs will surface as queue-depth limbs. ∎

**Worked instance.** 2× overload on both classes: queue holds, p99 within declared bound, zero drops (R1 #73 acceptance per class; R2 #67's acceptance requires *separate* evidence per class).

**Prediction P2.9 [EMPIRICAL].** Measured `W` per class reproduces the card within ±10% across three independent thermal-soak runs (the thermal governor's degradation state is part of the card's declared envelope — see T2.10).

**Binding.** R1 #73, R2 #67; R3 #21 (preallocated session pool — allocation must not live in the latency tail that the card certifies).

### T2.10 — Thermal-Governor Theorem (Φ_SAE) [DER, EMPIRICAL]

**Statement.** Key-delivery rate on thermally constrained edge Servers is governed by `Φ_SAE = security × availability × (1/energy)` under junction-temperature feedback: as `J_T` rises toward the platform's declared limit, the governor degrades the key rate (and MOGOPS gate fidelity `F_E` where the quantum plane attaches) *with consent receipts*, preserving ledger coherence and never browning out mid-ledger-write. The degradation schedule is monotone in `J_T` and published as part of the platform's capacity card (T2.9).

**Proof sketch.** Semiconductor failure and timing-error rates rise steeply (Arrhenius-type) with `J_T`; above the declared limit, *every* downstream ε term that depends on timing fidelity (Kramers barriers' measured `D`, latency certificates' `t_attest`, constant-time contracts' cycle distributions) becomes unmeasured — and an unmeasured `D` violates T1.3's precondition ("D must be measured, not assumed"). The governor therefore degrades *before* the measurement regime breaks, because continuing at full rate past the threshold would silently invalidate every barrier calibrated at lower temperature. The shed order (SDR → storage → compute, R1 #79) is fixed by the constitution so that the degradation itself is deterministic and replayable (T1.8 applied to throttling). ∎

**Worked instance.** Thermal chamber run (R1 #77): continuous key delivery persists *degraded* through the throttle event; receipts name the temperature band; the SoC never crosses the declared `J_T` limit; the post-cooldown recovery re-rates without a restart.

**Prediction P2.10 [EMPIRICAL].** Chamber traces show monotone rate degradation vs `J_T` matching the published schedule within tolerance; zero brownouts during ledger writes; all transitions receipted; post-event δ_R = 0 replay of the throttled window (the throttle schedule is part of the replay bundle's environment tuple).

**Binding.** R1 #77 (thermal governor), #79 (PoE budget + shed order), R2 #68 (PoE class negotiation impulse), R3 #25 (DVFS-coupled key governor), #30 (thermal predictive scheduling — permitted as *pre-degradation*, which is just earlier receipting, never silent).

### T2.11 — Finite-Key Smear Scaling Theorem [DER, RIGOR]

**Statement.** The per-block finite-key budget is `ε_K = 2·exp(−2n·δ_PE²/(1+Q)) + ε_PA + ε_EC`, where `n` is the block's sifted length, `δ_PE` the parameter-estimation margin, `Q` the QBER. Shorter blocks smear the budget: halving `n` does not halve `ε_K`, it inflates the exponential term by roughly `√n` scaling in the block-rate sense. The block sizer MUST derive `ε_K` live from `(n, Q)` and publish per-block ε; a fixed-ε claim across varying block sizes fails conformance (R1 #11), and a block-size floor exists (R2 #41) precisely so the smear stays bounded.

**Proof.** The first term is the Hoeffding/Chernoff form of parameter-estimation failure: the observed error rate must concentrate within `δ_PE` of the true rate, and the concentration probability is exponential in `n·δ_PE²`. Halving `n` at fixed `δ_PE` multiplies the exponent by half; to restore the same confidence the margin must widen as `1/√n` — the estimator's resolution degrades with less data (corpus pattern #13: "shorter blocks widen the parameter-estimation interval and shorten the secret"). The additive ledger carries the per-block `ε_K` as a first-class term (T1.1). ∎

**Worked instance.** Feed n = 10⁴ vs n = 10⁶ at fixed δ_PE, Q: the derived `ε_K` must scale by the predicted ~√n ratio within tolerance (R1 #11 acceptance); the regression is a Phase-2 SIMTIME vector (V2-SMEAR).

**Prediction P2.11.** 100% of drill blocks publish live-derived ε; the √n scaling regression holds within tolerance; any block emitted with a stale/fixed ε fails the bake.

**Binding.** R1 #11, R2 #41 (block floor), #42 (QBER hysteresis priced — T2.12), R3 #3 (adaptive Toeplitz dimension — PA output dimension may adapt only within the published ε envelope).

### T2.12 — QBER Hysteresis Pricing Theorem [DER]

**Statement.** QBER abort and resume thresholds MUST differ, and the abort/resume gap MUST be charged to ε as an availability/monitoring term: the hysteresis is not free. Both thresholds are Kramers-scaled (T1.3) from the *measured* channel noise intensity `D_Q` (digital-twin-predicted benign drift per R1 #31 is classified as drift and does not abort; it charges an availability ε instead).

**Proof.** A single threshold at `Q_c` is a chatter machine: a channel hovering at `Q_c` aborts and resumes every fluctuation — the 18,808-cycle lesson in the optical domain (Math §8's Insight 4: "every threshold in this stack is a Kramers barrier"). Separating abort (enter) from resume (exit) creates a barrier `ΔV = (Q_abort − Q_resume)`-scaled in QBER units, with width `(D_Q/2)·ln(T/τ₀)` per T1.3. The gap is a *purchase*: it buys false-abort suppression by tolerating longer exposure in the hysteresis band, and unbought tolerances are how "no-logs companies become breach headlines" (Math §14, Insight 5's pricing discipline). ∎

**Worked instance.** Q-Sentinel composite witness (R1 #23): `W = (QBER − QBER₀)/σ − λ·Δg⁽²⁾(0)` fuses the two drift channels with Kramers-scaled hysteresis; a patient PNS attacker shifting `g⁽²⁾(0)` below QBER threshold fires W; benign drift replays do not. Abort emits a signed W record for A7 gating.

**Prediction P2.12 [EMPIRICAL].** Replay the seven historical halt traces: twin-classified benign drift events charge availability ε and do not abort; injected attack signatures abort with signed W records; the hysteresis gap re-derives when measured `D_Q` shifts (retune = impulse, R1 #86).

**Binding.** R1 #23 (Q-Sentinel), #31 (digital twin), #86 (Kramers tuner); R2 #42 (priced hysteresis), #43 (photon-statistic limb — the `g⁽²⁾` channel is a limb with its own floor).

### T2.13 — PLS Reciprocity Key-Rate Bound Theorem [DER, RIGOR]

**Statement.** Channel-reciprocity key generation (CRKG) between two parties yields secret-key rate bounded by the min-entropy of the quantized channel estimates: `R_PLK ≤ f·H_∞(Q_quant)`, where `Q_quant` is the Gray-encoded quantization of the reciprocal channel measure and `f < 1` the reconciliation/privacy-amplification efficiency. The rate is *coupled to fading dynamics*: too-slow fading (static channel) starves temporal variation; too-fast fading decorrelates the parties' samples (spatial/temporal decorrelation windows). Mobile speed enters through coherence time `T_c ≈ 0.423/f_m` (max Doppler `f_m = v/λ_c`).

**Proof.** Min-entropy is the extractable-secret currency (leftover-hash lemma — the same lemma that prices privacy amplification in QKD, Math §12's A5 instantiation). The quantizer maps a fading distribution to symbols; `H_∞` of that symbol stream bounds the secret rate regardless of extractor quality (bound is information-theoretic). Decorrelation: an adversary at more than a coherence distance (~λ_c/2) from both parties measures a different channel — that is the *security* side. But the same mobility that decorrelates the adversary ages the parties' mutual CSI (corpus pattern #9/#18): at 3.5 GHz, 4 ms CSI delay at 30 km/h halves performance. Hence the rate is a bandpass in dynamics, and the honest profile declares the operating band rather than a peak. ∎

**Worked instance.** BLE-carrying CRKG profile (R2 #52) on a walking-pace fleet: quantization at RSSI quantiles with declared `H_∞` per environment class (Rician vs Rayleigh — variance differs measurably, corpus pattern #12); a stationary-indoor profile publishes a *lower* rate rather than reusing the mobile number.

**Prediction P2.13 [EMPIRICAL].** Extracted-key min-entropy (measured via compression estimator) matches the declared band per environment class; an adversary at > 2× coherence distance agrees with the legitimate parties on ≤ chance-level key bits (the security side of the bandpass).

**Binding.** R1 #36 (ρ_QC,RF efficiency), R2 #52 (coded PHY profile); R3 #27 (SF-bandit energy objective — may optimize SF *within* the declared rate band, never re-declare the band).

### T2.14 — Metadata Padding Bound [SPEC — quarantined construct]

**Statement.** Metadata padding changes an observer's *resolution*, never the *fact* of volume/timing exposure (corpus pattern #9): padding to a fixed block size `B` bounds an observer's per-message volume resolution at `B/2` on average, at a cover-traffic duty cost that MUST be budgeted (R2 #27, #28). The construct is labeled [SPEC]: it is an engineering trade, not a security theorem — it does not extend any QKD proof, and it MUST NOT be cited as one (R2 honesty note).

**Rationale for inclusion.** Phase-2 bearers (BLE, LoRa) have small MTUs and visible timing; the fleet needs a *bounded, honest* statement of what padding buys. What it buys: resolution-bounding against a *passive volume observer*. What it does not buy: protection against an active adversary who controls traffic, against endpoint compromise (QKD critique #17 — the endpoint sees plaintext), or against the ledger (which legitimately records volumes for A3 accounting — the accountability plane is open by design, and padding MUST NOT be applied to it; obscuring the accountability plane is a 2PED violation, Math §11).

**Worked instance.** BLE sifting discussion padded to 64-byte blocks; duty budget `≤ 5%` of the connection interval; the padding budget is an A3 line item with its own ε-neutral status (it buys availability/privacy-of-metadata, not secrecy — it does not enter ε_total; it enters the energy ledger).

**Prediction P2.14.** The accountability-plane paths are byte-exact unpadded (verified by comparing ledger rows to wire captures); the payload-plane paths carry the declared block padding; no cover traffic exists without a published duty budget.

**Binding.** R2 #27, #28; R2 conformance delta gate ("SPEC items may not be cited as theorems") — the schema linter rejects any spec-level claim importing padding into ε arithmetic.

---

## 4. Platform Workstream Specifications

### 4.1 W2.1 — Mini ARM Edge-Server Suite

**Position:** Node (full) and small/medium-fleet Server on the Linux blueprint verbatim (§4 of Groundwork) plus ARM-specific tuning — one codebase, two architectures. The canonical cheap spectrum node and the future anchor of Phase-4 digest-quorum enforcement.

**Build order:**
1. **Stage A0 — base bring-up.** Linux blueprint services on RPi OS/Debian ARM64; TPM 2.0 over SPI/I2C (LetsTrust/Infineon SLB9670 HATs) with identity proofs never leaving the TPM; RP1 RNG (RPi5) feeds the entropy pool with BPF rate monitoring — entropy starvation is a limb veto on key-generation classes (R1 #74/#75, R2 #71 backpressure).
2. **Stage A1 — capacity & power.** Per-class capacity cards (T2.9); PoE power-domain metering with the published shed order SDR → storage → compute, never brown-out mid-ledger-write (R1 #79, R2 #68); DVFS coupling to the key governor (R3 #25) within the Φ_SAE schedule (T2.10).
3. **Stage A2 — storage endurance.** NVMe/USB boot strongly recommended; SD-card wear telemetry as an A8 limb; ledger migration on wear threshold with continuity audit — accelerated-wear SIMTIME must preserve δ_R = 0 replay across the media change (R1 #78, R2 #69 migration rehearsal).
4. **Stage A3 — container boundary.** OCI runtime hook verifies image digest + signature against the A6 constitution before start; a valid-signed-but-wrong-constitution image is vetoed with a logged impulse (R1 #80, R2 #70 digest+constitution binding).
5. **Stage A4 — SDR & quantum-plane attach.** USB3 bandwidth reservation for SDR streams (scheduler throttles bulk storage during RX windows; R1 #76); QKD presence: `present` only via USB appliances on RK3588 (USB3 headroom), `absent` on RPi4 in practice — per-tunnel honesty field either way (Groundwork §7).

**Budgets (A3):** 5–15 W envelope; capacity card per class; thermal schedule published (T2.10); wear telemetry in `ccl-limbs`.

**Published limits:** no ECC memory (D_impl gap logged); SD-boot wear risk published with mitigation, not smoothed; PTP requires hardware NIC support (NTP fallback with declared slack).

**Acceptance (phase exit):** P2.9/P2.10 predictions green per class; wear-migration rehearsal passes; OCI veto drill passes; edge-Server role completes interop drill v2 items 1–6 (§5).

### 4.2 W2.2 — Android Client/Node

**Position:** Client (full) and Node (partial — foreground/battery-constrained). The fleet's most numerous platform; the honesty workhorses here are declared absence (no QKD, SDR RX-only) and declared slack (doze ±1 s), both of which ride the capability grammar rather than developer discipline.

**Build order:**
1. **Stage N0 — core + identity.** Kotlin + Rust (Cargo NDK) via JNI/UniFFI binding the frozen Phase-1 kernel C ABI; self-certifying Ed25519 identity in Keystore (StrongBox where present, TEE fallback); cryptonym = `H(pub ‖ namespace)` — no CA dependency (Groundwork §1).
2. **Stage N1 — A3/A5 accounting.** `ccl-budget` foreground duty governor (≤ 3% battery/hour node duty; breach ⇒ auto-degrade to Client-only with consent receipt, R1 #37); StrongBox operation counters as the hardware ε_act meter (T2.6); UI overlay mirrors `ε_total`, QBER, `λ_price` bit-for-bit from the ledger (R1 #47).
3. **Stage N2 — bearer & RF.** BLE 5 (Nordic APIs) auth beacons duty-cycled per T2.4; Wi-Fi Aware (API 26+) NAN RTT feeds the epoch client — sub-µs class sync without infrastructure, with offset-vs-slack verification against a PTP bench (R1 #38); cellular as telemetry limb (TelephonyManager signal streams ⇒ jamming = dead limb ⇒ A8 veto, R1 #45); SDR via USB-OTG RTL-SDR **RX-only** — TX requests refuse and name the paired-MCU route (R1 #35, #46 grammar).
4. **Stage N3 — doze & epoch.** WorkManager constraints carry `epochslack: ±1 s` in every issued CTOK (R1 #34); clock-discipline limb per T2.5; CI scans for background-smuggling patterns (VoIP/location/audio flag abuse — the Android instance of R1 #44's iOS scan).
5. **Stage N4 — PQ & thermal.** liboqs via UniFFI (ML-KEM-768, ML-DSA); thermal governor switches 768→512 at the declared junction temperature with ε/`b_pqc` adjustment declared and receipted (R1 #48, R2 #57).

**Budgets (A3):** control plane ≤ 1% of payload bytes; node duty ≤ 3% battery/hour; BLE envelope per T2.4.

**Published limits:** no true 60 Hz sync in doze (±1 s slack, logged); SDR RX-only on essentially all hardware; `qkd: absent` always — the phone is the classical/RF bearer node.

**Acceptance (phase exit):** P2.2/P2.4/P2.5/P2.6 predictions green; Faraday-cage drill halts key output with limb veto in the enforcement record; consent-degradation UI drill passes (non-dismissible banner + verifiable receipt).

### 4.3 W2.3 — iOS Client (node duties deferred)

**Position:** Client (full) only in Phase 2. The episodic-node model (trust decays to zero between wakes; re-derived per BGTask) is *specified* here but *characterized and armed* in Phase 3 after BGTask instrumentation — deferral is a published decision, not a shortfall (Groundwork §10, item 2).

**Build order:**
1. **Stage I0 — core + identity.** Swift + Rust via UniFFI (shared kernel); Secure Enclave P-256 for platform ops + app-level Ed25519/ML-DSA in Keychain; App Attest/DeviceCheck hardware attestation chain wrapped into the replay bundle — the Server verifies before releasing key material (R1 #40).
2. **Stage I1 — capability honesty.** Capability grammar marks: SDR `not-supported` (MFi blocks generic SDR — declared absence, iOS never claims SDR functions); `qkd: absent`; node-duty tokens **absent in Phase 2** (grammar-level absence, not a documentation absence).
3. **Stage I2 — BLE transport.** CoreBluetooth central+peripheral with background modes; state restoration serializes CCL state as CBOR into the restoration dictionary — on relaunch the ledger tip is verified and resumed, no fork, no gap (R1 #42).
4. **Stage I3 — attestation & release gates.** DeviceCheck chain verification Server-side; background-smuggling CI scan (R1 #41 — VoIP/location/audio flag abuse tied to QKD chatter fails the build); consent receipts user-visible with hash display (R2 #59 pattern).

**Budgets (A3):** processing-task envelope respected (≤ 30 s per wake once node duties arm in Phase 3); BLE advertising per T2.4's envelope.

**Published limits:** cannot run a continuous node (Phase-2 stance: cannot run *any* node duties); dwell-countdown (A9) is the only admissible trust model — stated in every capability token the platform issues.

**Acceptance (phase exit):** interop drill v2 client-role items pass; state-restoration continuity audit passes (force-kill during active ledger window ⇒ no fork, no gap); smuggling scan clean across the release.

### 4.4 W2.4 — Quantum-RF Bootstrap Plane

**Position:** the first live Quantum-RF interop — still without photonics on mobile, but with the SDR cognitive layer, LoRa bootstrap, and bearer re-epoch protocol exercised on real hardware against real spectrum. The plane inherits every Phase-1 SIMTIME-proven component and now runs it against physics.

**Build order:**
1. **Stage Q0 — RX-honesty baseline.** Android/Mac SDR RX capability declared; TX structurally routed (Android→paired MCU over BLE; Mini ARM/desktop = licensed-band-gated TX per constitution region table); `sdrlimit` field carries the envelope (Groundwork §1/§3).
2. **Stage Q1 — cognitive hop.** SoapySDR wrapper recomputes `P_RF-mitm` from link telemetry (replay counters, signal anomalies); crossing the constitution threshold triggers hop + consent receipt + re-epoch (T2.2); dwell window declared per profile.
3. **Stage Q2 — LoRa bootstrap live.** Puzzle issuance (Server) and solution (MCU) on real SX1276 links per T2.3; difficulty ladder by SF class; MITM drill fails closed; battery cost published per profile (R1 #25).
4. **Stage Q3 — hybrid session accounting.** Dual-Lock Ratchet streams (R1 #19: `k_i = H(k_{i−1} ‖ m_i ‖ g_i)` anchored by ML-KEM + QKD-salt; QKD salt absent in Phase 2 ⇒ single-anchor mode with declared degraded envelope — the receipt says so); Q-Pad-λ mode decision pre-armed (OTP iff `ℓ_QKD ≥ |M|` — always AES-flagged in Phase 2, receipt on any transition); finite-key smear live-derived per T2.11.
5. **Stage Q4 — key-network accounting.** SDN key accounting per T2.7 on the drill topology; satellite-opportunistic buffering module compiled and exercised against a Micius-class pass archive in SIMTIME (R1 #26: predicted vs delivered within `ε_volume`, bridging a simulated 2-orbit gap) — live satellite interop is out of scope and out of claims.

**Budgets:** all RF duty within platform envelopes (T2.4); `P_RF-mitm` measurement cadence per profile; every hop/transition receipted.

**Published limits:** no QKD hardware anywhere in the Phase-2 fleet; dual-lock runs in declared single-anchor degraded mode; CRKG profiles are environmental-class-declared (T2.13), not fleet-wide numbers.

**Acceptance (phase exit):** P2.1/P2.2/P2.3/P2.7/P2.12 predictions green; LoRa bootstrap interop drill passes end-to-end; hop-with-re-epoch drill passes with zero silent substitutions.

### 4.5 End-to-end scenario — lifecycle of one bearer-compromised session

This walkthrough binds the Phase-2 theorems to a single trace: an Android node's paired session degrades under RF attack and must transition bearers without a single silent step. As in Phase 1 §5.4, build teams should treat this as the canonical integration narrative; if any step cannot execute as written, the corresponding theorem's acceptance is not armed.

1. **Steady state (T2.1, T2.4).** The Android node holds a session over BLE to the Mini-ARM edge Server; the ledger carries `ε_total = ε_PQC + ε_RF-mitm + ε_guardian` with `qkd: absent` declared; BLE duty runs inside the T2.4 envelope; the UI overlay mirrors the budget bit-for-bit (T2.6).
2. **Adversary enters (T1.4, A8).** A MITM begins manipulating the 2.4 GHz auth channel; the node's RF telemetry (replay counters, anomaly detectors feeding `P_RF-mitm`) degrades the envelope. The mobile sensor floor (R2 #58) keeps the limb honest — a degrading channel is a *limb event*, not a routing preference.
3. **Threshold crossing (T1.3, Kramers).** `P_RF-mitm` crosses the constitution threshold; because the threshold was D-scaled at calibration, the crossing is a genuine barrier exit, not noise chatter — the digital-twin (T2.12 discipline) confirms the anomaly is not benign drift.
4. **Cognitive hop (T2.2).** The SoapySDR/RF wrapper triggers the hop within the declared dwell window; the hop is a governance event: old-epoch tickets void with receipts, the `rf_epoch` bump propagates, and the ε delta is logged.
5. **Re-attestation (A9, T2.5).** On the new bearer, the node re-attests: CTOK re-issued with `epochslack: ±1 s` soundness re-checked against the clock-discipline limb; the session's authority is re-derived — nothing survived the hop except identity, which is exactly the point.
6. **Accounting refresh (T2.1, T2.7, T2.11).** The new path's ε sum is recomputed and published; the key balance `K_net` is re-evaluated (the hop may have changed the min-edge); block sizing re-derives `ε_K` live; the Q-Pad-λ mode re-decides with an in-band flag — AES-flagged traffic continues, receipt chained.
7. **Witness & replay (A10, T1.8).** The Linux Server (audit witness) files the full transition record — hop cause, receipts, ε deltas, re-attestation — as replayable bundles; δ_R = 0 across the transition window including the environmental tuple (the hop is part of the recorded physics).
8. **Post-mortem duty (A3, T1.5).** The transition consumed guardian attention: fire-rate and ε_guardian shares are re-checked against `f*`; the incident's coherence cost lands in the ledger as impulses; the Λ (shadow price) of the epoch reflects the priced emergency.

The scenario's unremarkability test applies at fleet scale: a hop that leaves *any* of the eight steps unlogged is a silent substitution, and the drill corpus of §5.1 item 3 exists to make that state unrepresentable rather than merely unlikely.

### 4.6 RF drill statistical design (extends Phase-1 §6.3 rules to the channel domain)

RF drills sample from distributions, and a drill that samples one channel day and generalizes to all weather is the RF equivalent of the aggregate-only health report. Three additional rules bind Phase-2 RF acceptance: (i) **environment-class stratification** — CRKG and hop drills run across a declared environment taxonomy (indoor-static, indoor-walk, outdoor-mobile; Rician/Rayleigh declared per class per corpus pattern #12), with per-class pass/fail, never pooled; (ii) **channel-state declarations** — every RF drill record carries the measured `D` (noise intensity), SNR band, and environment class, because post-hoc Kramers re-derivation (T2.12) needs the regime, not just the verdict; (iii) **adversarial-pacing probes** — each RF drill includes a paced-adversary run (noise injected at the barrier scale `D·ln(T)`) to verify thresholds are escape-grade, not convention-grade; a drill suite with no paced runs has not tested the barriers, only the happy path. These rules inherit the Phase-1 confidence-bound and family-wise-correction discipline unchanged.

---

## 5. Workstream W2.5 — Interop Drill v2 & Harness Extension

### 5.1 The v2 bake (new edges against the green triangle)

Interop drill v2 extends the Phase-1 triangle bake to the five-platform fleet (Linux, Windows, MCU, Mini ARM, Android, iOS-client). The Phase-1 items (signature cross-verification, capability-walk, QKD-absent declaration, consent-receipt propagation, replay audit, axiom battery) re-run unchanged on all five; the new-edge items are:

1. **LoRa bootstrap interop (R1 #25, T2.3).** Unprovisioned MCU ↔ Mini-ARM Server over real SX1276 links: puzzle issuance → solution → PSK unwrap → ML-KEM upgrade, within the declared epoch window; MITM injection fails closed; battery cost published.
2. **SDR RX-honesty (R1 #35).** TX command sent to the Android SDR path MUST refuse and name the MCU route; the refusal is a capability-grammar event, logged.
3. **Bearer re-epoch (T2.2, R2 #90).** Forced MITM-degradation on the auth channel ⇒ hop within declared dwell ⇒ void receipts + re-attestation + updated ε sum; zero silent substitutions across the corpus.
4. **Jam = dead limb (R1 #45).** Faraday cage / shielded enclosure: cellular limb dies ⇒ A8 veto halts key output; the veto appears in the enforcement record; recovery via re-epoch only.
5. **Consent-receipt UX conformance (R1 #37, R2 #59).** Forced battery-budget exhaustion ⇒ non-dismissible banner + receipt chain to the Server; user-visible receipt hash matches ledger.
6. **Cross-device session migration pin (R2 #60).** Session migrated Android→iOS client; migration pin binds the new device attestation before any key material flows; a pin-less migration is refused.
7. **Capacity card separation evidence (R2 #67).** RK3588 and RPi4 cards reproduced independently; a single merged card fails review.
8. **Wear-migration rehearsal (R1 #78, R2 #69).** Ledger migration across media with δ_R = 0 replay continuity.

### 5.2 Phase-2 drill calendar (delta on the NA-84 instance)

| Drill | Mini ARM | Android | iOS (client) | Cadence |
|---|---|---|---|---|
| A6 self-amendment attempt | monthly | per release | per release | inherited |
| A7 non-novel injection | monthly | quarterly (SIMTIME) | quarterly (SIMTIME) | inherited |
| A8 dead-limb veto | monthly | quarterly | quarterly | inherited |
| A9 expired-ticket refusal | continuous | continuous | continuous | inherited |
| A10 replay audit | monthly | per release | per release | inherited |
| LoRa bootstrap interop | per release | n/a | n/a | this doc |
| SDR RX-honesty | per release | per release | n/a (declared absent) | this doc |
| Bearer re-epoch | per release | per release | n/a | this doc |
| Thermal chamber (Φ_SAE) | quarterly | n/a (governor only) | n/a | this doc |
| StrongBox ε reconciliation | n/a | monthly | n/a | this doc |
| Smuggling scan (CI) | n/a | per build | per build | this doc |
| Fleet interop bake (5 platforms) | **every release candidate** | | | this doc |

### 5.3 V2-* SIMTIME vectors (fixtures added; all V1-* stay green)

| Vector ID | Content | Proves | Pass condition |
|---|---|---|---|
| V2-ENV | Hybrid-envelope accounting vs additive ledger on 100 paths | T2.1 | published budget = sum, all paths |
| V2-HOP | 50 forced bearer transitions | T2.2 | 3-part receipt triple on every transition |
| V2-BOOT | 10⁴ puzzle solves incl. adversarial replays | T2.3 | zero accept without work; joules within 20% |
| V2-DUTY | BLE interval sweep on reference fleet | T2.4 | current curve within tolerance; deviations = impulses |
| V2-SLACK | Doze-window clock-offset corpus | T2.5 | soundness holds; 80%-slack triggers re-characterization |
| V2-SMEAR | n = 10³…10⁷ block sweep | T2.11 | ε_K live-derived; √n scaling within tolerance |
| V2-QSENT | Seven halt traces + PNS injections | T2.12 | benign drift ≠ abort; attacks abort with W records |
| V2-PLK | CRKG corpus per environment class | T2.13 | H_∞ in declared band; eavesdropper ≤ chance |
| V2-EXPOSURE | Relay-compromise injection suite | T2.8 | reroute ≤ 1 epoch; X_T from live attestations |
| V2-KNET | Negative-balance route request suite | T2.7 | 100% refusal with min-edge + toll named |
| V2-THERMAL | Chamber schedule replay | T2.10 | monotone degradation; receipts; δ_R = 0 across window |
| V2-PAD | Padding/ledger byte-exactness | T2.14 | accountability plane unpadded; payload per declared B |

---

## 6. Phase Gates

### 6.1 Entry criteria (M3)

| # | Criterion | Evidence |
|---|---|---|
| E1 | Phase-1 exit gate passed (all G1–G7) | Phase-1 certification record |
| E2 | Kernel C ABI frozen; UniFFI/P-Invoke/C bindings published | binding repo tagged |
| E3 | Mini-ARM, Android, iOS reference hardware registered; thermal chamber and RF bench booked | hardware register |
| E4 | Spectrum/regional licensing table ratified into the signed constitution (region_table) | constitution amendment record |
| E5 | LoRa puzzle constants (difficulty ladder) ratified as constitution parameters | amendment record |

### 6.2 Exit criteria (M6) — go/no-go metrics

| # | Metric | Go threshold | No-go action |
|---|---|---|---|
| G1 | T2.1–T2.14 acceptance tests in CI | 13/14 green fleet-wide (T2.14 SPEC-construct verified as quarantined, not "green" — it has no security claim to pass) | fix before exit |
| G2 | V1-* + V2-* SIMTIME corpus | 12 + 12 vectors green | fix; no vector may be weakened |
| G3 | Interop drill v2 (§5.1, items 1–8) | 8/8 pass; Phase-1 items 6/6 on the 5-platform fleet | release train blocked |
| G4 | Honesty fields on mobile | `qkd`/`sdrlimit`/`epochslack`/`d_impl` present in 100% of mobile attestations; slack soundness (P2.5) holds | fix emitters |
| G5 | Zero silent transitions | 100% of degradations/hops/downgrades receipted across all drill corpora | root-cause; the class is forbidden |
| G6 | Capacity cards per class | RK3588 + RPi4 reproduced ±10% over 3 thermal-soak runs | re-measure; publish actuals |
| G7 | iOS node-token absence | zero node-duty tokens issued by iOS in the drill corpus | grammar bug; fix |

**Rollback plan.** If any go threshold fails at M6: (1) the failing platform's workstream is frozen at its last green stage; the fleet continues interop at the reduced platform set *with the reduction published in every attestation* (a declared degraded fleet, per the degradation-governor discipline — the fleet itself runs the same honesty rules as a node); (2) RF-plane failures (bootstrap, hop, re-epoch) revert the Quantum-RF plane to SIMTIME-only operation and Phase 3's episodic-node work re-sequences behind the fix; (3) any failure traced to a Phase-1 kernel contract reopens the Phase-1 gate (the contract was frozen; the amendment path is a signed constitution amendment plus full re-bake of every platform). Under no condition does a failing workstream ship with its honesty fields softened to pass — the fields are the record, and the record is the deliverable.

### 6.3 Boundary handoff to Phase 3

Phase 2 hands Phase 3: a five-platform certified fleet with live RF-plane interop; characterized Android doze/BLE envelopes and iOS BGTask instrumentation data (collected throughout Phase 2 precisely to arm Phase 3's episodic node); per-class capacity cards and thermal schedules; and the extended SIMTIME corpus. Phase 3 begins with macOS full-role bring-up and the iOS episodic-node characterization — both specified in the Phase 3 blueprint (W3.1/W3.2).

---

## 7. Traceability Matrix (Phase 2)

| Axiom / Theorem | Instrument (Phase 2) | Enhancement bindings | Drill / acceptance |
|---|---|---|---|
| A1 | ledger continuity across mobile lifecycle; wear migration | R1 #42 (iOS restore), #78 (NVMe wear), #70 (MCU inherited) | force-kill restore; migration rehearsal |
| A2 | reciprocity telemetry at fleet scale (Servers) | R1 #13, R2 #77 | canary swap games continue (Phase-1 scale) |
| A3 + T2.4/T2.6/T2.9/T2.10 | duty-cycle envelope; StrongBox ε; per-class cards; Φ_SAE | R1 #37, #48, #69, #73, #77, #79; R2 #51, #57, #67, #68; R3 #25, #26 | energy probes; chamber runs; overload ×2 |
| A4 + T2.5 | NAN-RTT epoch feed; slack soundness | R1 #38, R2 #31, #32 | PTP-bench offset vs slack |
| A5 + T2.6/T2.11/T2.12 | hardware ε_act; finite-key smear; priced hysteresis | R1 #11, #23, #31, #33; R2 #41, #42, #43, #51 | counter-vs-ledger; √n regression; halt-trace replay |
| A6 | region table in constitution; smuggling scans | R1 #41 (iOS pattern → Android CI), #50-adjacent | CI scans per build |
| A7 | Server-side gating continues for all mobile events | R1 #71 inherited | quarterly SIMTIME injection |
| A8 + T2.2 | jam = dead limb; RF limbs | R1 #45, #74, #76; R2 #58 (mobile sensor floor for RF) | Faraday drill; USB3 starvation drill |
| A9 + T2.2/T2.5 | bearer re-epoch; doze slack | R1 #34, #25-rule; R2 #50, #90 | hop receipt triple; slack corpus |
| A10 | mobile replay bundles w/ DeviceCheck chain | R1 #40; R2 #49 (Play Integrity + App Attest as limbs) | bundle verify per release |
| Bearer composition + T2.1/T2.7/T2.8 | envelope ledger; SDN accounting; exposure routing | R1 #9, #17, #19, #20, #21, #24, #26, #32; R2 #33, #34, #85, #87, #90 | V2-ENV/KNET/EXPOSURE |
| Bootstrap + T2.3 | LoRa puzzle live | R1 #25; R2 #63 (duty-cycle regulatory cap) | V2-BOOT; MITM fail-closed |
| SPEC quarantine + T2.14 | padding bounds; duty budgets | R2 #27, #28 | V2-PAD; linter rejects ε-import |

**Enhancement coverage check.** Phase 2 consumes R1 #17–48 (Quantum-RF + mobile), #73–80 (ARM); R2 #25–36 (classical channel), #49–60 (mobile episodic subset), #61-adjacent through #72 (edge subset: #63, #65–72), #85, #87, #89, #90; R3 #3, #25–27, #29, #30, #35, #43. Items cited but deferred: #39–44 iOS node subset (→ Phase 3), #59–64 Mac (→ Phase 3), #81–84 guardian federation precursors (→ Phase 3 pre-arm / Phase 4). No Phase-2 deliverable may cite an item as "done" that its own acceptance test has not armed.

---

## 8. Honesty Ledger (Phase 2 — kept, non-negotiable)

1. **No phone is QKD hardware, and Phase 2 touches no photonics.** Every `qkd_slot` remains `absent`; the dual-lock ratchet runs in declared single-anchor degraded mode with receipts. The Quantum-RF plane in this phase is honest *RF* — SDR RX, LoRa bootstrap, cognitive hop — not quantum, and no document in this series may blur that.
2. **iOS issues zero node-duty tokens in Phase 2.** The episodic-node model is specified (Phase-3 blueprint carries T3.1's formal treatment) but not armed; the capability grammar enforces the absence at wire level, and the drill corpus must show it, not the documentation.
3. **SPEC constructs stay quarantined.** T2.14 (padding) and any R2/R3 [SPEC] item used here are engineering trades with published budgets; none is citable as a security theorem, and the schema linter enforces the boundary. The phase's honesty includes its own labels.
4. **The algebra follows the dependencies.** The product form appears exactly once — relay-compromise exposure (T2.8), where independence is structurally defensible — and is forbidden everywhere a shared guardian, clock, RNG, or OS is the common cause (T1.1, T2.1). Any drill corpus that catches the wrong algebra in the wild has caught the corpus's most expensive historical bug class, and the catch is celebrated in the record, not hidden.
5. **The fleet can degrade, but only on the record.** The rollback plan's "declared degraded fleet" is itself an honesty-field event: a reduced-platform interop set publishes the reduction in every attestation, exactly as a node publishes a reduced PQ profile. A fleet that shrinks silently has violated the same axiom as a sensor that dies silently — and per A8, acting (promoting releases) in the dark is itself the breach class.

**STATUS:** 4 platform/plane workstreams specified · 14 theorems (13 falsifiable + 1 quarantined SPEC) with predictions and bindings armed · 8 new-edge bake items · 12 V2 SIMTIME vectors · 7 go/no-go gates · rollback plan fixed · 0 silent-substitution paths representable.

**COHERENCE:** Phase-2 measured CI published per release candidate against the ε decomposition dashboard and the route frontier manifests (R1 #32) — the phase pays for its own measurement, and the mobile UI pays it back bit-for-bit (R1 #47).

---
*Series: Phase 1 (M0–M3) Reference Triangle · **Phase 2 (M3–M6) Edge & Mobile Expansion** · Phase 3 (M6–M9) Full Fleet Convergence & Certification. Phase 4 (guardian federation) remains the published boundary.*
