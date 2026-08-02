# BUILD4 Signal Prereg — v13 (runway landmark, Fugu v12 minimum path executed) — RATIFIED 2026-07-29; LOOK CONSUMED 2026-07-31

**Status:** RATIFIED 2026-07-29 (`14f93f1`: SPEC_STATUS flipped, §2 signed — this header was
updated 2026-07-31 after an independent review of the PUBLIC writeup caught it still carrying
the pre-ratification wording; the code constant, not this file, was always the enforcement
point). **THE SINGLE LOOK EXECUTED 2026-07-31 15:44:20Z** — lock COMPLETE, fingerprint
`197054c20180ef5f0d3efd15`, verdict NULL (D=-0.0208, p=0.5874, n=249); S3 significant after
Holm (raw p=0.00010). Report: `review/N600_LOOK_REPORT.md` (ERRATUM: its cohort header prints
the pre-manifest split 12/238; the analyzed counts are G1's and result_json's 12/237 —
canonical values in `review/N600_LOOK_RESULT.json`). **Governance note, stated openly:** after
two Fugu rounds (v11 NO-GO → v12 NO-GO, both attached to the record), the architect elected to
execute Fugu's v12 13-item minimum path WITHOUT a third confirming round. v13 therefore ships
with this conformance map — each item → what was built → where → which test — as the
self-validation substitute. The v12 verdict remains the authoritative external spec; nothing
here relaxes it.

**v13 = v12 text + the deltas below.** Everything v12 pinned and Fugu passed (landmark concept,
censored concordance, fixed-m Holm, reservation architecture, outer-envelope framing,
supersession language) carries over unchanged.

## Conformance map — Fugu v12 "Minimum path to resubmission", item by item

| # | Fugu requirement | Implementation | Verification |
|---|---|---|---|
| 1 | Pin the endpoint; correct claims | **Recorded-event time** (poll-granular): T = the stored `death_ts` (the labeler's first below-F read at/after the peak) minus pca. All claims worded "recorded death time"; the latent crossing may precede it by up to the poll gap (hence item 3's ceiling) | v13 §1 pin; render text |
| 2 | Pre-landmark events to scope BEFORE X-missingness | assemble(): Y-validity + landmark routed before any X branch | `test_nonfinite_and_boundary_death_times` (899s→scope, 900s→included) |
| 3 | Exact versioned labeler replay | `runway_event_audit` v2: re-marshals reads exactly as `run_labeling` (no usd filter), replays sealed `label_one`, requires label/death_ts/peak equality; materiality-bounded refgap rule (band-edge native arithmetic, Fugu-sanctioned); poll-gap ceiling **10800s** (recorded-time comparability; excluded-gap distribution = mandatory report); transactional replace; `audit_version` + per-token source fingerprint; coverage fail-closed. **Live: 351 events — replay equality 100%; 145 CLEAN / 206 AMBIGUOUS (all poll-gap, the pre-C3 pause era; post-C3 accrual is clean-cadence)** | `test_audit_*` (7 tests via the labeler seam) + the live 351-event replay |
| 4 | Safety fail-closed; HARD_FAIL out of trigger | unreadable safety DB → `safety_unavailable` → run-primary refuses; HARD_FAIL excluded before trigger counting | `test_safety_db_failure_fail_closed`, `test_hard_fail_excluded_before_trigger` |
| 5 | Freeze manifest AND bounds cohort with accrual cutoff | cutoff = (pca, mint) of manifest #600; bounds cohort = every frame token ≤ cutoff with its class, frozen at reservation; envelope `n_missing` counts ONLY frozen missing classes | `test_bounds_cohort_respects_accrual_cutoff` |
| 6 | Transactional 15m; no stale rows; full-hour equivalence | derive(): compute-all → BEGIN IMMEDIATE DELETE+INSERT; `--limit` never publishes; `--verify` rebuilds every frozen 1h row's counts+volumes from raw swaps, recorded in `w15m_equivalence_checks` | 15m selftest [2]/[4]; live: 895/895 derived, 0 conformance failures; equivalence record cited in the look report |
| 7 | Validate timestamps + finite inputs | depth: finite ∧ >0; T: parseable, finite, 0 ≤ T ≤ 72.0; NULL death_ts → missing-Y | `test_nonfinite_depth_is_missing_x`, boundary test |
| 8 | Immutable snapshot + constrained recovery | reserve_look freezes lock+manifest+row-snapshot+bounds cohort in ONE txn; `--resume-look` recomputes from the SNAPSHOT only, refusing on spec/seed/n_perm/ratification mismatch or fingerprint drift | `test_crash_recovery_resume_and_tamper`, `test_resume_refused_while_draft` |
| 9 | Envelope honesty + scope routing | outer-envelope labeled as such (v12 §5); frozen-cohort accounting (item 5); scope classes excluded from bounds | v12 §5 text + item-5 test |
| 10 | Escape hatch + inherited outputs | **escape hatch IMPLEMENTED** (`--recompute-bounds`: refuses before COMPLETE, once-only via its own singleton, narrows the envelope from the frozen snapshot, never re-runs the test). **Descriptive panels IMPLEMENTED — all six** (2026-07-29 closes R13-3): restrictive-eligibility D, 6h/24h re-censor, demoted binary, per-stratum runway D (annex G6 banner attached to the panel DATA, verbatim), initial-pool contrast (single-NULL-bucket honesty pre-Phase-D, stated in the output), NC_F-as-T=0 (T=0 tie-group ⇒ mutually non-comparable, comparable vs all later — pinned at 5-not-6 pairs). Panels compute ONLY inside the look; PRE/STRADDLE + NC_F material is frozen as an aux snapshot in the reservation transaction and replayed at resume (a pre-panel reservation renders them "unavailable", never recomputed live) | `recompute_bounds` + 2 tests; `descriptives`; 8 panel tests, 4 mutations killed |
| 11 | Rendering/counters | verdict mapping ROBUST/NPI/ADVERSE/NULL; runway G1 (events/censored/comparable/HR-heuristic both directions); trigger counter from post-eligibility members | selftest [5]/[10] |
| 12 | Accurate attestation | rewritten below — distinguishes automated assembly from human-visible joins | §2 |
| 13 | Integration tests | `tests/test_b4_signal_look_v13.py` — 15 tests over all named scenarios | pytest 197 green |

## 1. Endpoint pin (item 1, final wording)

The outcome is the **recorded death time**: the first poll at/after the trajectory peak whose
genuine in-band USD depth is below F=$2,500, as derived by the sealed labeler and certified
per-event by the replay audit. It is poll-granular; the latent on-chain crossing may precede
it by up to the local poll gap. Tokens whose pre-death gap exceeds 10,800s are excluded as
event-time-noncomparable (missing-Y, bounds-included, distribution reported). No claim of
"true death moment" is made anywhere; concordance is over recorded times, ties excluded.

## 2. Blind-integrity attestation (rewritten per Fugu v12 §6 — signed at ratification)

> As of 2026-07-29 (ratification): (1) the confirmatory statistic has never executed against
> the live database — no lock row exists; `SPEC_STATUS` had never been RATIFIED before this
> commit. (2) **Automated, in-memory assembly** of token-level rows joining scores and
> outcomes occurs inside `assemble()` (dry-run/status/fingerprinting); its outputs surfaced
> to humans are COUNTS AND MARGINALS ONLY. (3) **No human has viewed a token-level
> score↔outcome join, any association statistic, or any candidate-endpoint comparison** on
> experiment data; no such statistic has been computed against the live database. The only
> code path that would (`compute_v12_statistics`, reachable solely via run-primary/resume)
> has executed ONLY on synthetic test fixtures — the v13 test suite and the 2026-07-29
> 600-token dress rehearsal — never on a live row.
> (4) Outcome-only reads on record are logged with dates in `review/ATTESTATION_LOG_b4_runway.md`.
> (5) Public product surfaces expose outcome-only quantities; no flow feature has ever been
> surfaced. (6) The runway endpoint was chosen from outcome marginals and product economics.
>
> Signed: **Denys** (architect)   Date: **2026-07-29**

## 3. Ratification procedure

1. Architect reads this document + the conformance map, signs §2.
2. The ratification commit — and nothing else — flips `SPEC_STATUS` to
   `"RATIFIED <date> — v13 runway landmark"` and records the attestation signature.
3. The look then still requires: trigger ≥600, current-version audit, 1:1 15m preflight,
   0-mismatch equivalence record, readable safety DB — all structurally enforced.
4. Invocation-time membership is the pinned rule (v12 §3.3 option 3): the manifest freezes at
   the moment of the architect's single authorized invocation; discretionary delay/re-runs are
   prohibited by the reservation itself.


## 10. Corrections applied after the c3 code review (2026-07-25)

The review of this instrument (§R13 of the cumulative doc) found that **this document's own
conformance map contained a false claim** — item 10 asserted a bounds-only escape hatch that did
not exist (`ESCAPE_TABLE` was a dead constant). That is the worst defect class for a document
serving as the substitute for a skipped Fugu round, so it is recorded here rather than quietly
patched:

- **R13-1 (P1) — FIXED by implementing, not by amending.** `--recompute-bounds` now exists with
  the narrow licence v6 §9 grants: refuses before the look is COMPLETE, refuses a second use
  (own singleton row), recomputes ONLY the envelope from the frozen snapshot, and copies the
  stored D/C/p through untouched. Two tests pin once-only and never-re-runs-the-test.
- **R13-3 (P2) — CLOSED 2026-07-29 by implementing.** All three panels now exist in
  `compute_v12_statistics` (look-only; aux material frozen at reservation, replayed at
  resume); item 10 carries the evidence. The interim honest-gap wording served its purpose.
- **R13-4 (P2) — FIXED.** `PHANTOM` is now a scope class (`scope_phantom`), not missing-Y, so it
  no longer silently widens the envelope; an *unrecognised* label now raises rather than being
  folded into missing-Y.
- **R13-5 (P2) — FIXED.** The permutation p depended on input ROW ORDER (manifest order at
  run-primary vs snapshot order at resume) and the order-independent fingerprint could not
  detect it. Rows are now canonically sorted before any RNG use; a test asserts identical
  p/D/fingerprint under shuffling.
- **R13-6 (P2) — FIXED.** Audit coverage was "the table is non-empty", so an event maturing
  after the last audit run was silently pseudo-AMBIGUOUS — a time-correlated exclusion of the
  newest deaths. `--run-primary` now refuses unless every manifest event has a current-version
  audit row.
- **R13-7 (P2) — FIXED.** `SPEC_VERSION` said v12; the module docstring still described the
  v11 design including the literal depth rule the A-3 seal forbids. Both corrected.
- **R2-12 — MEASURED, not fixed.** See §R2-12 in the cumulative doc: the read-set divergence is
  real (50.4% of POST DEAD tokens carry a class-(b) read) but **changes 0 of 351** peak anchors
  and 0 death timestamps, because a class-(b) read is below F by construction. §2's endpoint is
  therefore PINNED to the raw-usd onset and cites the measurement. **Re-run the diagnostic at
  the look.**

**Still open before ratification: NOTHING code-side (as of 2026-07-29).** R13-2 closed
(`069308d`, plus the 07-29 trigger-counter/resume-render fix `aba0232`); the three descriptive
panels closed (this commit); R8-11's b4_flow.db writer contention closed end-to-end 2026-07-27
(labels-lane compute-then-write — the last long-transaction holder). What remains is the
architect's act alone: the §2 attestation signature (name + date) and the ratification commit
that flips `SPEC_STATUS` — nothing else may flip it.
