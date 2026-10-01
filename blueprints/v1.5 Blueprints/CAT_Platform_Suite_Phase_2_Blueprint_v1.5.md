# Phase 2 Blueprint — Edge & Mobile Expansion (M3–M6)
## CAT-Aligned Platform Suite, Development Phase Series v1.5

**Supersedes:** Phase 2 v1.0. Inherits Phase 1 v1.5, not merely Phase 1 v1.0. A Phase-1 v1.5 gate that failed is not a Phase-2 entry.
**Amendment basis:** audit items C61–C90 and the Phase-1 deltas those items depend on; solutions S-14, S-25–S-39.
**Phase designation:** Phase 2 of 4.

**Phase thesis, restated:** Bearer expansion under invariance remains the mission. v1.5 corrects the joule arithmetic, gives probabilities instruments, and stops treating a hop as instantaneous.

---

## 0. v1.5 delta register

| ID | v1.0 defect | v1.5 rule |
|---|---|---|
| P2-D1 | 2^18 · 65 µJ published as 0.4 J | Cost is ≈ 17 J; ladder re-derived from the joule budget |
| P2-D2 | Puzzle difficulty static | ρ from measured joules; ±1 per epoch inside a constitution range |
| P2-D3 | Puzzle issuance unbounded | Client work token plus pool |
| P2-D4 | Hop dead-window unnamed | Bearer-classed re-handshake bound |
| P2-D5 | Reroute “one epoch” on LoRa | Per-class bound |
| P2-D6 | 3%/h node cap | Operating cap 1%/h; 3%/h emergency ceiling only |
| P2-D7 | KEM thermal switch has no overlap | Dual-suite window |
| P2-D8 | App Attest required while offline | Cached attestation, degraded-ε receipt |
| P2-D9 | P_RF-mitm has no estimator | Named model, calibration, miscalibration drill |
| P2-D10 | Ratchet cited without a protocol | Ratchet spec is a Phase-2 deliverable |
| P2-D11 | 5–15 W vs SDR draw | Itemized power sheet |
| P2-D12 | H_QPR omits guardian, then adds it | One formula |
| P2-D13 | Migration pin has no common root | Server notary |
| P2-D14 | Offline receipts unspecified | Local sequenced queue |

---

## 1. Corrected bootstrap (T2.3a)

**Statement.** Unchanged in structure: the puzzle is not the long-term key; it unwraps an ML-KEM exchange; expected work is `2^ρ` hashes; verification is one hash.

**Worked instance, corrected.** `E_hash` on the ESP32-S3 reference is ≈ 65 µJ per keccak-block batch as previously estimated. `ρ = 18` is `2^18` blocks, so energy is `2^18 · 65 µJ ≈ 17 J`, and wall time at ≈ 240 MHz is on the order of a minute, not a sub-joule event. v1.0’s “≈ 0.4 J” is withdrawn.

**Difficulty rule.** `ρ(class) = floor(log2(B_node / E_hash_measured))`, recomputed from the bench at each firmware release, and adjustable by at most 1 per epoch inside a constitution-published range. If the resulting ρ is too small to meet the adversary margin, the link class does not bootstrap; it uses the wired factory PSK. A false published joule cost fails the release scan.

**Issuance.** A puzzle request carries a lighter client work token. Pool size is per node class. Issuance and refusal are impulses.

**Nonce.** `nonce = H(server_random ‖ node_trng ‖ rf_fingerprint ‖ rtc)` with server_random 128 bits, single use, lifetime shorter than the honest grind time.

**Acceptance.** V2-BOOT: zero accepts without work; measured joules within 20% of the corrected publication; a laptop-class grind inside the epoch does not authenticate.

---

## 2. Bearer and power corrections

**T2.2a.** A hop voids tickets and starts re-attestation. The dead window is a declared state `REATTEST`, not traffic. Completion bound is per class: BLE ≤ 2 s; LoRa ≤ next RX window plus slack. If re-attestation exceeds the bound, the session stays down and a receipt says so. “Within the dwell window” is not assumed for SF12.

**T2.8a.** Reroute bounds are bearer-classed. Server-local may use one epoch. LoRa uses the next RX window plus slack. The receipt names the bound.

**T2.1a.** The reporting envelope is `H_QPR = 1 − (1−ε_QKD)(1−2^{−b_PQC})(1−P_RF-mitm)(1−ε_guardian)` and is labeled reporting-only. The budget remains the sum. `ε_QKD` is omitted, not set to 0, when the slot is `absent`. Computational terms are tagged.

**Battery.** Operating node duty ≤ 1% battery per hour. The 3% figure is an emergency ceiling that forces client-only with a receipt. MCU advertising remains ≤ 150 µA at a 1 s interval under the Phase 1 v1.5 listen rule.

**Power sheet.** Each edge class publishes line items for SoC, radio, TPM HAT, and storage. RK3588 plus a HackRF-class SDR that exceeds the 15 W envelope MUST constrain the radio or raise the envelope. An unitemized envelope fails review.

**Physical attacker.** SPI TPM HATs admit bus probe and clock glitch as a named class. Countermeasure is published `d_impl`, not a claim that extraction “logs an impulse” unless the silicon actually attests that event.

---

## 3. Session, attestation, and receipts

**Ratchet spec (deliverable).** Dual-lock uses the Phase 1 v1.5 KDF, not XOR. The spec states out-of-order window, per-chain epoch, state-loss recovery, and what is not claimed (no KCI proof beyond the combiner). Theorems MUST NOT cite the ratchet until this spec is hashed into the constitution.

**Thermal suite switch.** Both suites stay valid for a bounded tick overlap. The receipt precedes the first 512-class ciphertext. No overlap, no switch.

**Offline attestation.** A cached App Attest or Play Integrity result may be used until its stated expiry. Past expiry, re-derivation runs at degraded ε with a receipt, or refuses node duty. Airplane mode is a declared state, not a surprise.

**Migration pin.** A Server notary maps Play Integrity and App Attest into one identity grammar. `pin = H(old_session ‖ new_attestation ‖ notary_epoch)`. Restore still does not carry CTOK.

**Consent queue.** Receipts are sequence-numbered and buffered if the Server is unreachable. Gaps reconcile on reconnect. The queue is a limb; an overflow vetoes further degradation.

**StrongBox.** Reconciliation uses weighted op classes (attestation, unwrap, enforcement) and a declared drift tolerance. A raw counter delta of 1 is not required.

---

## 4. P_RF-mitm instrument

The score is a likelihood ratio against a benign trace library, isotonic-calibrated, with a confidence interval published per epoch. An uncalibrated counter MUST NOT enter ε_total. A miscalibration drill is on the Phase-2 calendar. Environment classes stay stratified; pooled passes fail schema.

---

## 5. Gates amended

G1 still quarantines T2.14. New G8: corrected joule publication matches the bench within 20%. New G9: P_RF-mitm estimator spec hashed and miscalibration drill green. New G10: ratchet spec present before any interop item cites it. “100% receipted” is reported with N and an upper bound; offline gaps are queue reconciliations, not silent success.

---

## 6. Honesty ledger (v1.5)

1. No phone is QKD hardware.
2. iOS still issues zero node-duty tokens in this phase.
3. The v1.0 puzzle energy was wrong by about forty times. v1.5 publishes the corrected arithmetic and will not bootstrap a class that cannot pay it.
4. `P_RF-mitm` without a calibration record is not a probability.
5. A hop has a dead window. The spec says so.

**STATUS:** v1.5 issued · joule ladder corrected · issuance bounded · hop window classed · estimator required · 0 product-form budgets.
