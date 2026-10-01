# Phase 4 Blueprint — Guardian Federation (M9–M15)
## CAT-Aligned Platform Suite, Development Phase Series v1.5

**Supersedes:** Phase 4 v1.0. Inherits Phase 1–3 v1.5. Entry requires the Phase-3 v1.5 exit gate and the corrected boundary.
**Amendment basis:** audit items E119–E130; solution S-48 and the epoch, receipt, and term-sheet repairs from earlier phases.
**Phase designation:** Phase 4 of 4. Flag default remains `monitor`.

**Phase thesis, restated:** Federation may enforce only as a digest quorum that cannot be demoted by one node, only on metrics fresher than a published bound, and only after the fleet has actually been measured for the interval the boundary names.

---

## 0. v1.5 delta register

| ID | v1.0 defect | v1.5 rule |
|---|---|---|
| P4-D1 | macOS voter in the table, excluded in D4.1 | Opt-in, class-carded, same section |
| P4-D2 | Unilateral demotion | f+1 votes and a cooldown |
| P4-D3 | Partial synchrony unnamed | GST and timeout are constitution parameters |
| P4-D4 | Embedded λ_net and ℛ may be arbitrarily old | Freshness ≤ 60 ticks |
| P4-D5 | WATCH and coercion undefined | Both defined; undetectable rules withdrawn |
| P4-D6 | A6 lookup only in CI | Sanction sink checks at runtime |
| P4-D7 | No production owner for the registry | Conformance function owns it after M9 |
| P4-D8 | Constitution epoch for old bundles unnamed | Bundles address a constitution id |
| P4-D9 | Arming on two quarters | Not before M15 and four drills |
| P4-D10 | ℛ* still a conjunct | Conjunct only after Phase 3 locks the estimator |

---

## 1. Electorate (one table)

| Platform | Vote | Condition |
|---|---|---|
| Linux Server | Yes | Independent fate class |
| Windows Server | Yes | vTPM voters price `d_impl` and do not count as hardware-TPM fate |
| Mini ARM | Yes | Own capacity card; RK3588 and RPi4 are different classes |
| macOS | Opt-in | Small-fleet voter only if the capacity card and SIP gap are published |
| Android, iOS | No | Must not receive a voter key |
| MCU | No | Feature digest only |

A fate class is one OS image digest plus one firmware digest. Four voters in one fate class are one vote for quorum arithmetic, not four. Minimum independent voters for `enforce` is `3f+1` with `f ≥ 1`, hence at least four independent fate classes, on at least two sites.

---

## 2. Protocol parameters

- Epoch id is Phase 1 v1.5 `(era, tick)`. View-change timeout and GST are constitution integers.
- Leader is `argmin H(era ‖ tick ‖ voter_id ‖ tip)` among the current independent set.
- A commit faster than the A4 bound computed from those parameters is refused.
- Demotion to `monitor` requires `f+1` independent voters and a cooldown of one certificate lifetime. A single node may request demotion; it may not complete it. The local choke limb may still refuse to sign, which withholds that voter and can prevent commit without flipping the flag.
- Certificates embed `lambda_price`, `conf_omega` if locked, and `ℛ` only if the estimator is locked. Each embedded metric carries the tick it was computed. Age greater than 60 ticks voids the certificate.
- Bundles address `constitution_id`. A third party loads that id. Issuer-offline replay uses the addressed package, not “current.”
- Sanction sinks call the constitution lookup at runtime. CI is additional, not sufficient.
- WATCH is the published state when independent site-paths < 2 or independent fate classes < `3f+1`. Entry and exit are impulses. While WATCH, new enforcement certificates are not issued.
- A `qkd: present` claim without a matching appliance attestation chain is a parse failure. No separate coercion detector is required for that case.

---

## 3. Theorems adjusted

T4.1 stands as safety by quorum intersection, with liveness only inside the published GST. Cascading view changes across an era bump are a recovery epoch, not a commit.

T4.2 conjunct (iii) uses `ℛ` only after the estimator lock. Until then that conjunct is absent, not silently true. Demotion is the f+1 rule above.

T4.3 and T4.4 stand, with the game budget evaluated once per certificate lifetime, not once per 60 Hz tick.

T4.5 requires constitution id inside the inputs hash.

T4.6 requires two distinct attestation identities on the impulse. Reason codes are an enum.

T4.7 remains SPEC. The runtime lookup is the control; CI is the regression net.

---

## 4. Ownership and gates

After M9 the falsifier registry is operated by the conformance function. A development workstream is not a production owner.

G1–G6 stand, with these additions. G7: electorate table and D4.1 match. G8: demotion drill shows a single node cannot clear the flag. G9: certificate older than 60 ticks is refused. G10: arming review cannot start before M15, four quarterly drills, and six months of seven-platform telemetry. Staging evidence transfers only if the staging note lists fate-class diversity and site count.

Rollback remains: any failed gate leaves the flag at `monitor`.

---

## 5. Honesty ledger (v1.5)

1. Federation does not create quantum security.
2. The watcher is inside its own equations, once.
3. SPEC exports do not gate enforcement.
4. F1–F5 stay open.
5. One node cannot demote the federation. v1.0 said otherwise; that was a standing denial-of-service lever.
6. Twelve months of fleet data are not inherited at M9. Arming waits until the calendar can support the claim.
7. Physical and platform-policy limits from Phases 1–3 still apply. This ledger does not shrink them.

**STATUS:** v1.5 issued · electorate consistent · demotion requires f+1 · freshness bounded · arming not before M15 · 0 key-custody paths · 0 unilateral flag clears.
