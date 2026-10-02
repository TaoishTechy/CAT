# Phase 1 Blueprint — Reference Triangle (M0–M3)
## CAT-Aligned Platform Suite v2.0

**Status:** normative. Supersedes Phase 1 v1.0 and v1.5 in full. Do not read those documents for current rules.
**Phase:** 1 of 4. Federation is out of scope.
**Labels:** [NORM] adopted · [DER] derived, falsifiable · [RIGOR] applied mathematics · [MAP] analogy · [EMPIRICAL] named instrument and falsifier · [SPEC] quarantined.

Phase 1 builds the arbiter: Linux in all three roles, Windows as the enterprise server anchor, MCU as the constrained node, and one kernel. Exit is interop drill v1 with no silent degradation, every honesty field present, and every acceptance test armed. v2.0 adds the authority, the lifetimes, the term sheet, and the encoding those claims require.

---

## 1. Workstreams

| ID | Deliverable | Exit owner |
|---|---|---|
| W0 | Epoch Fabric Service, specified in §2 | Kernel |
| W1.1 | CCL crates and C ABI | Kernel |
| W1.2 | Linux suite | Linux |
| W1.3 | Windows Server | Windows |
| W1.4 | MCU profile | MCU |
| W1.5 | Interop drill v1 | Conformance |
| W1.6 | CDDL pack | Conformance |
| W1.7 | ε term sheet | Kernel |

Entry requires grandmaster hardware, site contracts, and a PTP path on the register (E0). W0 is not an exit-only surprise.

---

## 2. Epoch Fabric Service

Three site-separated grandmasters. Tick is `tick = floor((t_PTP − t0) · 60)`, `t0` a constitution instant. Agreement is a 2-of-3 threshold signature over `(era, tick, membership)`. WAN distribution is an application-layer tick protocol over authenticated UDP, not bare PTP across the internet. Hardware timestamping is required on the grandmaster LAN only.

Cross-site skew bound is 4 ms. A client that sees two signed ticks differing by more than that refuses both and holds. Membership loss of one grandmaster is degraded-continue at n−1, published, and does not bump `era`. `era` bumps only after sustained loss of a member for 60 s, or an explicit membership amendment. Old and new era overlap for one certificate lifetime (120 ticks). Tickets name `(era, tick)`.

Clients verify the threshold signature. Grandmaster key rotation is an A6 amendment. A grandmaster reboot is not an era bump.

---

## 3. Lifetimes (TLT-1)

At 60 Hz, 3 600 ticks = 60 s and 21 600 ticks = 6 min.

| Class | Validity |
|---|---|
| CTOK | 3 600 ticks, and ≥ slack + margin |
| Session ticket | 21 600 ticks; dies on any sub-epoch bump |
| Session key | same as the session ticket |
| Enforcement certificate | 120 ticks on Servers; next wake on MCU |
| Alert token | one wake or 30 s |
| Batch anchor | 1 800 ticks (30 s) |
| Receipt and void receipt | 3 600 ticks |
| Voter-set certificate | 120 ticks |
| Tick-ledger entry | 120 ticks |

Void receipts need two witnesses from distinct fate classes, neither the issuer nor the subject. Fate class is defined in Phase 4; in this phase a witness is a distinct Server anchor.

---

## 4. Wire

Deterministic CBOR, RFC 8949 §4.2. Indefinite lengths and non-minimal integers are rejected. Chain tip is `H(domain ‖ u32le(len(prev)) ‖ prev ‖ u32le(len(row)) ‖ row)`.

Dual signature means Ed25519 and ML-DSA-65 both verify on Server-class packets. On LoRa and BLE, one ML-DSA-65 signature covers a batch of at most 32 packets or 30 s. Interim packets are Ed25519-only, bind the batch anchor, and MUST NOT carry enforcement. The interim window is a tagged term `ε_interim = 2^{−128}` on the sheet, computational, not summed into the statistical budget. Verifiers keep a bounded anchor queue (64 entries, oldest evicted, eviction is an impulse).

**Suites are per peer class.** Server–Server floor is ML-KEM-768. Server–MCU floor is ML-KEM-512, receipted. A Server offers 512 only in a Server–MCU negotiation. Intersection empty is a refusal. Chosen suite id is in the KDF info string. There is no product envelope.

**Replay.** Bundles contain DRBG outputs for the recorded counter range. Bundle publication is bound to epoch death of the keys those outputs fed. A third party replays from the published bundle alone. Live counters never resume a published range; the publication schedule is the registry. `F` is fixed-point only. δ_R ≠ 0 quarantines until a second pinned verifier confirms.

**Latency.** Slacked clocks certify RTT/2 plus `ε_asym`. No method id, no certificate.

**PSK for the bake.** Factory-wrapped, per-device, installed on a wired jig. Rotation and compromise are constitution events. The wrapping key’s custody and `d_impl` are on the term sheet. This is not the LoRa puzzle.

---

## 5. ε sheet

Statistical terms and computational tags are separate columns. The release gate uses the statistical column. Computational tags are displayed and do not enter that sum.

`ε_inflation = max(0, 1/(n+1) − α_declared)` under the resolution heuristic, or the Clopper–Pearson excess under the binomial model. The sheet names which model the row uses. A row that subtracts resolution from α is a schema failure.

`ε_accounting` is the sum of named quantization and batching rows. Residual must be ≤ that bound.

`κ` is dimensionless. `D_impl` is published beside ε. It does not enter the statistical sum. A unit-bearing κ is rejected.

`ν` is the KKT multiplier. `ν = 0` means the constraint is inactive. The A3 violation is a missing `lambda_price` publication.

`ℛ = (r_f + r_r) / (2 · min(r_f, r_r))`. `ℛ* = 1.15` is provisional and MUST NOT gate promotion until Phase 3 locks an estimator.

Control bytes ≤ `max(0.01 · payload, floor_class)`. LoRa floor equals one canonical envelope.

---

## 6. Null gate (one model)

The binomial model is normative. For zero observed extremes, the rule-of-three applies: a claim `p < 0.01` at 95% confidence needs `n ≥ 300`. Default probe count is 300. The old n = 128 figure is withdrawn. `1/(n+1)` may be printed as a rank heuristic and MUST NOT be the gate. Clopper–Pearson is the interval. MCU and iOS throttled profiles use n = 12 only with a nonzero inflation row when `1/13 > α_declared`.

---

## 7. Platforms

Linux and Windows bind to W0. PCR quotes address `measurement_epoch`. Historical measurements are retained for the bundle window. The 18 000 handshake/s figure is a planning target. A card exists only after per-primitive timings on named hardware, with an interval. No acceptance test may require reproducing the target.

MCU ledger is an append-only segment journal: generation, parent segment id, public hash chain. A per-record MAC is additional and never replaces the hash chain. Checkpoint anchors every N segments, signed. Compaction preserves verify-from-anchor. Retention is a constitution window. Listen is Class A/B with a constitution poll period, or Class C at duty ≤ 1%. Continuous Class C is refused. Poll drift beyond slack is an impulse. RNG claims are per SoC. Stack cap includes interrupt nesting. Null-gate is Server-side.

---

## 8. Gates

G1: theorems in §6 and the kernel contracts green on Linux and Windows; MCU exemptions are T1.5, T1.11, T1.13, named. G2: drill v1, six items, no silent degradation. G3: replay sample states N, estimator, and an upper bound. G4: MCU overflow build fails. G5: saturation choke on the 160/166 trace. G6: A6/A8/A9 fail closed, with N. G7: honesty fields complete. G8: W0 live, era and tick on every token. G9: term sheet reconciles. G10: deterministic CBOR corpus green.

Drill item 3 quarantines a misconfigured node. It does not fail the fleet for one bad peer.

---

## 9. Honesty

No photonics. `qkd` is `absent`. The 18k figure is not a measurement. `ℛ*` does not gate. Class C continuous listen is unrepresentable. LoRa enforcement packets wait for the batch anchor. This document is the Phase 1 rule set; v1.0 and v1.5 are history.
