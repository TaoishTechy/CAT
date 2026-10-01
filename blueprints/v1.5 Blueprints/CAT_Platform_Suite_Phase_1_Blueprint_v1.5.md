# Phase 1 Blueprint — Reference Triangle (M0–M3)
## CAT-Aligned Platform Suite, Development Phase Series v1.5

**Supersedes:** Phase 1 v1.0. v1.0 remains the historical baseline. Where v1.5 is silent, v1.0 stands. Where they conflict, v1.5 governs.
**Amendment basis:** 144-point audit v1.0, items A1–A24 and B25–B60; solutions S-01–S-05, S-07–S-24.
**Phase designation:** Phase 1 of 4. Guardian federation remains out of scope for this phase and is specified in Phase 4 v1.5.
**Normative verbs and epistemic labels:** unchanged.

**Phase thesis, restated:** Phase 1 builds the arbiter, and v1.5 adds the authority the arbiter was missing: the epoch fabric, numeric ticket lifetimes, an ε term sheet, and a wire encoding that can actually be replayed bit-for-bit.

---

## 0. v1.5 delta register

| ID | v1.0 defect | v1.5 rule |
|---|---|---|
| P1-D1 | No epoch-authority workstream | W0 Epoch Fabric Service is entry-blocking |
| P1-D2 | No ticket lifetimes | Constitution table TLT-1 is normative |
| P1-D3 | No ε term sheet | `eps-terms.v1.json` is a release artifact |
| P1-D4 | ℛ undefined | Defined in D1.15; floor is provisional until measured |
| P1-D5 | CBOR not canonical | RFC 8949 §4.2 deterministic encoding is mandatory |
| P1-D6 | ML-KEM-768 vs 512 contradiction | Suite negotiation; floor per platform |
| P1-D7 | Dual signature infeasible on LoRa | Batch-anchor profile on constrained bearers |
| P1-D8 | RNG tuple vs live nonce | DRBG commitment, not raw entropy |
| P1-D9 | ±1 s slack swallows A4 | RTT/2 plus declared asymmetry term |
| P1-D10 | Epoch id has no cross-fabric semantics | `(era, tick)` |
| P1-D11 | T1.2 conflates continuous and discrete p | Restated as a counting rule |
| P1-D12 | 18k card is a target | Card is unpublished until W is measured |
| P1-D13 | ε_accounting inequality inverted | Residual must be ≤ bound |
| P1-D14 | ℓ bits vs \|H\| bytes | All burn arithmetic in bits |
| P1-D15 | λ sign / name collision | Multiplier renamed ν; unpublished price is the violation |
| P1-D16 | Class C vs 150 µA | Scheduled listen windows |
| P1-D17 | LittleFS called COW | Append-only segment journal specified here |
| P1-D18 | Phase-1 bake has no provisioning path | Factory-wrapped PSK, wired only |

---

## 1. Workstreams (delta)

W1.1–W1.5 stand, with the contracts below amended. v1.5 adds:

| ID | Workstream | Deliverable | Exit owner |
|---|---|---|---|
| W0 | Epoch Fabric Service | Three site-separated PTP grandmasters; signed tick ledger; era on membership change | Kernel team |
| W1.6 | Wire schema pack | CDDL for attestation, receipt, suite-negotiate, bundle | Conformance team |
| W1.7 | ε term sheet | Machine-readable registry; CI recomputes the sum | Kernel team |

W0 is on the Phase-1 critical path. A triangle bake without a running grandmaster is not an A9 drill.

---

## 2. Definitions amended or added

- **D1.1a (Epoch id).** `e = (era, tick)`, `era, tick ∈ ℕ`. `tick` is a monotone function of the grandmaster’s PTP timescale. Rollback of `tick` is unrepresentable. `era` increments when grandmaster membership changes. A ticket names both fields.
- **D1.2a (Lifetimes).** Validity is an integer tick count from TLT-1, not an implicit session. Session validity remains `min` over the tuple, and each element has a number.
- **D1.3a (Term sheet).** Every ε term has `{id, owner, value, unit, derivation, calibrated_at}`. Composition remains sum-only. A term without a sheet row cannot register.
- **D1.7a (Replay bundle).** `{policy hash, inputs hash, clock, drbg_commit, counter_range, chained tip, measurement_epoch}` plus PCR quote where a TPM exists. The bundle does not contain raw keying entropy.
- **D1.15 (ℛ).** `ℛ = (r_f + r_r) / (2 · min(r_f, r_r))` over paired forward and reverse probe scores in a named window. `ℛ* = 1.15` is **provisional** and MUST NOT gate promotion until the measurement protocol in Phase 3 v1.5 has a locked estimator. Until then the symbol is telemetry, not a conjunct.
- **D1.16 (Control-plane budget).** `control_bytes ≤ max(0.01 · payload_bytes, floor_class)`. LoRa floor equals one canonical envelope. A percentage alone is not a budget on short frames.
- **D1.17 (Shadow price).** The published field is `lambda_price`. The KKT multiplier is `ν`. `ν = 0` means the constraint is inactive. The A3 violation is a missing `lambda_price` publication, not a zero multiplier.
- **D1.18 (D_impl admission).** Trace distance is published beside ε. It enters `ε_total` only through a named scaling `κ` with a unit and a calibration. An unscaled addition is a schema failure.

### TLT-1 — token lifetimes (normative starting point)

| Class | Validity | Slack rule |
|---|---|---|
| CTOK | 3 600 ticks (60 s at 60 Hz) | Validity ≥ declared slack + margin |
| Session ticket | 21 600 ticks (6 min) | Dies on any sub-epoch bump |
| Enforcement certificate | 120 ticks (2 s) on Servers; next wake on MCU | Stale if embedded metrics older than 60 ticks |
| Alert token | 1 wake or 30 s, whichever is shorter | Must carry ε_inflation if probes < 99 |

These numbers are constitution parameters. Changing them is an A6 amendment, not a config edit.

### ε term sheet v1 (minimum rows)

| Term | Owner | Starting value | Note |
|---|---|---|---|
| ε_PA | privacy amplification | calibration required | no silent zero |
| ε_PE | parameter estimation | calibration required | |
| ε_cor | error correction leakage | calibration required | |
| ε_auth | authenticator failure | `2^{-b}` labeled computational, not statistical | not summed with statistical terms without a tag |
| ε_guardian | watcher | from D1.4 once f and p_fp are measured | |
| ε_inflation | coarse null gate | required if n < 99 | formula: `α_declared − 1/(n+1)` floored at 0, tagged |
| ε_asym | one-way latency asymmetry | declared per bearer | A4 method term |
| κ·D_impl | implementation gap | κ published | dimensionless κ times distance |

Statistical terms and computational advantages are tagged. The dashboard shows both. The security budget that gates release is the sum of statistical terms plus tagged computational terms displayed separately, never silently added as if they were one probability.

---

## 3. Wire contract amendments

1. **Encoding.** Every hashed or signed CBOR structure uses RFC 8949 §4.2 deterministic encoding. Verify paths canonicalize before compare. Indefinite lengths and non-minimal integers are rejected.
2. **Chain.** `tip' = H(domain ‖ u32le(len(prev)) ‖ prev ‖ u32le(len(row)) ‖ row)`.
3. **Dual signature means AND.** Ed25519 and ML-DSA-65 must both verify on Server-class packets. A one-signature packet is a refusal.
4. **Constrained-bearer profile.** On LoRa and BLE, one ML-DSA-65 signature covers a batch of at most 32 packets or 30 s, whichever is first. Intervening packets carry Ed25519 only, each binding the batch anchor. The batch anchor is inside the replay bundle. This is a declared profile, not a silent downgrade.
5. **Suite negotiation.** `SUITE_NEGOTIATE` carries supported KEM, signature, and hash sets. Intersection is chosen. A result below the platform floor is refused with a typed receipt. The chosen suite id is in the KDF info string. MCU floor is ML-KEM-512; Server floor is ML-KEM-768. A 768 offer to an MCU is not a fault; a silent 512 session labeled 768 is.
6. **Receipts.** `{type, issuer, subject_hash, reason, epoch, prev_receipt}`. Voiding another party’s ticket requires a void receipt from the voiding party and a witness signature. Identical format across A6 vectors.
7. **Latency method.** Slacked platforms certify `meas` as RTT/2 and attach `ε_asym`. A certificate without a method id is void.
8. **Attestation CDDL** is a Phase-1 deliverable. Honesty fields, suite ids, epoch tuple, anchor class, and slack are required keys.

---

## 4. Theorems corrected

### T1.2a — Null-gate counting rule [DER, RIGOR] (replaces T1.2’s resolution claim)

**Statement.** For a discrete probe count, let `p̂ = k / n` where `k` is the number of rotation probes at least as extreme as the event. The rank resolution of an order statistic on `n` i.i.d. continuous uniforms is `1/(n+1)`, and that quantity is an expected minimum, not a p-value. Gating at `α` with a discrete count requires `n ≥ 1/α − 1` only under that counting model, and the reported interval is Clopper–Pearson at the declared α. A continuous p-value computed by a different test does not inherit this n rule. Default remains n = 128. MCU and iOS throttled profiles use n = 12 only with the ε_inflation row filled from the term sheet.

**Acceptance.** V1-NULL-128 accepts at α and rejects at α/2 under the counting model. A verdict that prints `1/(n+1)` as a p-value fails schema.

### T1.6a — Accounting residual [DER]

Reconstruction residual MUST be `≤ ε_accounting`. The v1.0 parenthetical “≥ 10⁻⁶” is withdrawn. The bound is the sum of quantization and batching terms on the term sheet, per platform.

### T1.8a — Replay domain [DER]

`F` is fixed-point only. Floating point inside `F` fails lint. The bundle records a DRBG state commitment and a counter range. The live path never resumes a counter range that appears in a bundle. δ_R ≠ 0 quarantines; a second version-pinned verifier must confirm before sanctions void.

### T1.11a — Capacity card [EMPIRICAL]

The 18 000 handshake/s figure is a **planning target**, not a card. A card exists only after per-primitive cycle counts (ML-KEM decap, ML-DSA verify, Ed25519, CBOR parse, ledger append) are measured on named hardware with a confidence interval. Until then deployments MUST NOT advertise the target.

### T1.14a — Burn units [DER]

`|H| = 256` bits. `ℓ_net = ℓ − 2|H|` with all quantities in bits. Halt if `ℓ_net ≤ 0`.

### T1.13 label

The governance self-reference argument is **[MAP]**, not [RIGOR]. The fail-closed drill stands. The Gödel citation does not.

---

## 5. Platform corrections

**Linux / Windows.** W0 client binds to the grandmaster set. PCR quotes address a `measurement_epoch`. Historical measurements are retained for the bundle retention window. vTPM ε must exceed hardware-TPM ε by at least the term-sheet margin, not by an unspecified gap.

**MCU.** No LittleFS “COW pages.” The ledger is an append-only segment journal: generation counter, parent segment id, per-record MAC from a SoC-held key. Power-cut fuzz targets this layer. Listen is scheduled Class A/B windows, or Class C at a published duty ≤ 1%. Continuous Class C listen is outside the 150 µA budget and is refused by the radio scheduler. NIST 800-90B claims are per SoC, with conditioning named; ESP32-S3, nRF52840, and STM32H7 do not share one RNG profile. Stack cap includes worst-case interrupt nesting. Provisioning for the Phase-1 bake is a factory-wrapped PSK installed over a wired jig. The LoRa puzzle remains SIMTIME-only until Phase 2.

**Caller fail-closed.** `ccl-epoch` returns receipts only to A6-enumerated TCB processes. Other callers receive a typed refusal.

---

## 6. Gates amended

G3 reports estimator, N, and an upper confidence bound. “30/30” is a sample, not a certainty claim. A sequential test (SPRT) may replace the fixed sample if α and β are on the gate. G1 lists the MCU exemptions explicitly: T1.5, T1.11, T1.13 are Server-side; the other eleven are on-device or round-trip. G4 overflow builds still fail. New G8: W0 grandmaster quorum live and era/tick present in 100% of tokens. New G9: term sheet reconciles with `ccl-eps` publish. New G10: deterministic CBOR corpus green.

---

## 7. Honesty ledger (v1.5)

1. The triangle still has no photonics. `qkd` is `absent`.
2. The 18k figure is not a measurement.
3. `ℛ* = 1.15` is not yet an instrument.
4. Class C continuous listen is not in the power budget.
5. Dual signatures on LoRa are batch-anchored. Saying otherwise would be false.
6. This revision prices itself: ten deltas that close audit CRIT items in this phase. Softening any of them voids the v1.5 certification.

**STATUS:** v1.5 issued · W0 added · lifetimes numeric · term sheet required · encoding deterministic · 0 raw RNG exports · Class C continuous listen unrepresentable.
