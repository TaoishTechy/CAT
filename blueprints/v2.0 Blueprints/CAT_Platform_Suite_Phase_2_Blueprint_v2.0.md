# Phase 2 Blueprint — Edge & Mobile Expansion (M3–M6)
## CAT-Aligned Platform Suite v2.0

**Status:** normative. Supersedes Phase 2 v1.0 and v1.5. Inherits Phase 1 v2.0 only.
**Phase:** 2 of 4.

The fleet grows by Mini ARM, Android, and an iOS client, plus a live RF plane. Nothing silent grows with it. iOS issues no node-duty tokens in this phase.

---

## 1. Workstreams

W2.1 Mini ARM. W2.2 Android. W2.3 iOS client. W2.4 RF plane. W2.5 drill v2. OCI start-time re-verification and periodic runtime digest (former S-32) sit in W2.1: verify at exec, re-measure in service, mismatch kills and writes an impulse. The TOCTOU window is closed here, not deferred.

---

## 2. Bootstrap, reclassified

The v1.0 puzzle energy was wrong: `2^{18} · 65 µJ ≈ 17 J`, about a minute on an ESP32-S3. A GPU finishes the same work in milliseconds. Hash-work at a battery-feasible ρ is not impersonation resistance.

**Normative identity.** The LoRa puzzle is a Sybil and issuance-rate tax under A3. It is not an authenticator. Impersonation resistance is the factory-wrapped per-device PSK from Phase 1 v2.0, or a later PHY distance bound if one is specified and measured. Unprovisioned nodes do not join.

`ρ = floor(log2(B_node / E_hash))`, adjustable by at most 1 per epoch inside a constitution range. Issuance requires a lighter client work token and a pool. The nonce is `H(server_random ‖ node_trng ‖ rf_fingerprint ‖ rtc)`.

Exit drill G3 is solver-correctness, joule publication within 20% of the bench, and issuance accounting. It is not “unprovisioned node authenticates over LoRa.”

---

## 3. Bearers, power, sessions

A hop enters `REATTEST`. Tickets void. BLE must finish within 2 s; LoRa within the next RX window plus slack. Otherwise the session stays down and a receipt says so.

Reroute bounds are per class. Server-local may use one epoch. LoRa may not.

Reporting envelope includes the guardian factor and is labeled reporting-only. The budget is the statistical sum. `ε_QKD` is omitted when the slot is `absent`, not set to 0.

Operating node duty ≤ 1% battery per hour. 3% per hour is an emergency ceiling that forces client-only. Power sheets are line-item. A HackRF-class radio that breaks the envelope constrains the radio or raises the envelope.

`P_RF-mitm` is a likelihood ratio against a benign library, isotonic-calibrated, with an interval per epoch. Uncalibrated scores do not enter ε. Miscalibration is a drill. Environment classes are not pooled.

The ratchet spec is hashed into the constitution before any theorem cites it. It states the out-of-order window, chain epoch, and state-loss recovery, and uses the Phase 1 KDF. Thermal suite switch keeps both suites for a bounded tick overlap; the receipt precedes the first 512-class ciphertext.

Cached App Attest or Play Integrity may be used until expiry. After that, node duty refuses or runs degraded-ε with a receipt. Airplane mode is a declared state. `SCHEDULE_EXACT_ALARM` is not assumed; slack is measured against the alarms the platform actually grants.

Migration pin: `H(old_session ‖ new_attestation ‖ notary_epoch)` under a Server notary. Consent receipts buffer with sequence numbers. Overflow vetoes further degradation.

Region is taken from the radio regulatory domain where the OS exposes it, else from a signed operator assertion. Self-declared region alone does not unlock TX.

---

## 4. Honesty

No phone is QKD hardware. iOS issues zero node tokens. The puzzle is a rate tax. The 17 J figure stands. A hop has a dead window.
