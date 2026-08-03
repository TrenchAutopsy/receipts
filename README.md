# Trench Autopsy — Receipts

We run the morgue for Solana memecoins: an always-on collector that tracks fresh
pump.fun-class launches at population scale, measures the **exit liquidity** actually
available in the pool (not the price chart), and issues death certificates when a
token's real sell-side depth is gone. We publish our results — including our nulls.

This repository is the **public receipt vault**. Every research claim we publish is
backed by files whose SHA-256 hashes were committed publicly **before or alongside**
the claim. The files here are byte-exact: hash them yourself and compare against the
hashes we posted. If they don't match, we cheated. That's the whole deal.

```
sha256sum -c SHA256SUMS
```

---

## n600-look-1 — the pre-registered single look (verdict: NULL)

**Question:** does a bigger opening flow into a fresh memecoin pool predict a longer
life before the exit liquidity dies? **Answer (primary): no** — a null, published as
such. One secondary cut (S3) survived multiple-comparison correction; it is treated as
a hypothesis for Round 2, not a claim.

The analysis was a **one-shot, pre-registered look**: the rules (cohort definition,
death rule, statistics, multiple-comparison procedure, and the mapping from numbers to
verdict words) were frozen and hash-locked *before* the data was examined. The look
consumed a single-use lock — fingerprint `197054c20180ef5f0d3efd15`, recorded inside
both the report and the machine-readable result — so it cannot be quietly re-run until
something comes out prettier.

**Why "n600" but n=249?** The pre-registration set a trigger of 600 tokens reaching a
sealed 72-hour verdict; that gate was met (604) and the look fired. The *analyzable*
cohort is smaller — 249 — because the same frozen rules exclude any token whose
outcome or exposure could not be established honestly: 186 with an indeterminate
outcome, 281 with an ambiguous event time, 220 excluded by the SOL-reference validity
band, 73 that died before the exposure window opened. Every exclusion rule was written
down before the data was seen, and the full attrition table is the first block of
`N600_LOOK_REPORT.md`. Trigger counts tokens that *finished*; the estimand counts
tokens we can *honestly analyze* — and we would rather publish 249 defensible rows
than 600 convenient ones.

| File | SHA-256 | What it anchors |
|---|---|---|
| `n600-look-1/BUILD4_SIGNAL_PREREG_v13.md` | `f1b15799…e2ea` * | The frozen pre-registration (v13): every rule of the experiment, ratified and signed before the look fired. |
| `n600-look-1/N600_LOOK_REPORT.md` | `82adc71a…e6ea` * | The verbatim run report produced by the harness at look time (2026-07-31 15:43–15:44 UTC). |
| `n600-look-1/N600_LOOK_RESULT.json` | `aee11011…aafa` * | The canonical machine-readable result. Where prose and JSON disagree, this file wins. |
| `n600-look-1/LOOKDAY_N600_PACKAGE.md` | `5d2084bd…4ce7` | The ex-ante publication package: announcement drafts written for **every possible outcome** (positive, null, adverse, not-identified), committed ~40 hours *before* the look ran. We could not have known which one we'd need. |

\* Full hashes in `SHA256SUMS`. The first three are the hashes published in the
analysis post; the fourth is the pre-verdict receipt.

**Known erratum** (disclosed, not edited away): the report header says 12/238 censored
while the analyzed set is 12/237; the JSON is canonical. The files stay byte-exact —
fixing a typo in a hash-committed file would be indistinguishable from tampering.

## Verifying for yourself

1. Hash any file here (`sha256sum <file>`, or `certutil -hashfile <file> SHA256` on
   Windows) and compare against the hashes in the published analysis.
2. Read the pre-registration first, then the report — the point of the exercise is
   that the rules predate the numbers.
3. Every number in the published analysis should be findable in the report or the
   result JSON. If you find one that isn't, call it out.

## What's next here

- Round 2 (temporal replication on a fresh cohort, plus a paper-traded S3) is
  pre-registered the same way; its artifacts land in `n600-look-2/` when the round
  completes.
- Parts of the pipeline are scheduled to open as an open-data/open-source milestone;
  until then, methodology questions and document requests: DM us.

## License

Documents and data in this repository: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
— share and adapt with attribution to **Trench Autopsy**. See `LICENSE`.

Nothing here is financial advice. A `CLEAR` from us is a coroner's note that the
patient is currently alive — it is not a buy signal.
