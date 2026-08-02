# LOOK-DAY PACKAGE — the N=600 verdict drop

Pre-written 2026-07-29, while still blind (trigger ~511/600). **This document contains NO
result numbers — every `{{SLOT}}` is filled on look day from the single look's own report,
and nothing here ships before the look completes** (honesty guardrail 1: nothing blinded
ships pre-look). Copy variants exist for all four verdict classes; on look day exactly one
variant is kept per surface and the other three are deleted, unedited.

Public-naming rule: the independent reviewer is referred to publicly as "an independent
out-of-loop adversarial reviewer" — internal codenames stay internal. No claim of "audit"
or "peer review" — the true claim is strong enough.

---

## 1. Look-day runbook (operational order — each step stops for the architect where marked)

1. `--status` shows `trigger met`. Confirm disk/health green.
2. Re-run the event audit: `python scripts/b4_signal_look.py --audit-events`
   (preflight requires a CURRENT-version row for every manifest death).
3. Re-run `scripts/diag_death_ts_readset.py` (the R2-12 pin: re-measure at the look).
4. **STOP — architect's explicit go.** The invocation freezes the manifest and consumes
   the single look forever.
5. Run in background (long): `python scripts/b4_signal_look.py --run-primary
   --i-understand-this-is-the-single-look`
6. Save the full report output to `review/N600_LOOK_REPORT.md` verbatim; commit.
7. Fill every `{{SLOT}}` below from that report ONLY. Delete the three unused verdict
   variants. **STOP — architect reviews the filled copy before anything posts.**
8. Post order: TG long post → X thread (Typefully, URL-free) → pin both.

## 2. Fill slots (single source: the look report)

| Slot | Source line in the report |
|---|---|
| `{{DATE}}` | run date (UTC) |
| `{{N}}` / `{{N_EVENTS}}` / `{{N_CENSORED}}` | analyzable cohort line |
| `{{TRIGGER_N}}` | trigger line (≥600) |
| `{{D}}` `{{C_INDEX}}` `{{P}}` | PRIMARY line |
| `{{VERDICT_CLASS}}` | VERDICT CLASS line |
| `{{D_LO}}..{{D_HI}}` | G3 runway Manski bounds |
| `{{N_MISSING}}` | bounds line ("over N unanalyzed") |
| `{{ATTRITION_*}}` | attrition dict |
| `{{SECONDARIES}}` | S1/S2/S3 + Holm lines |

## 3. The X thread (URL-free so Typefully can direct-publish; ≤280/tweet)

> **T1 (hook):**
> We spent 7 weeks building a kill-proof experiment to answer one question:
> can the first 15 minutes of money flow predict which Solana memecoins die?
> Today we ran it. Once. That's all the protocol allows.
> Here's what happened. 🧵

> **T2 (the setup):**
> The rules were frozen BEFORE the data matured:
> — death = real quote-side liquidity below $2,500, held to a 72-hour verdict
> — signal = net flow in the first 15 minutes, nothing else
> — N=600 tokens, committed in advance
> — one statistical look, ever. No reruns, no cherry-picks.

> **T3 (the discipline):**
> The analysis code refuses to run twice — it writes a lock the moment it starts.
> Every input row is frozen and fingerprinted.
> An independent out-of-loop adversarial reviewer forced two full redesigns before
> we were allowed to call it preregistered.
> We published the death rates all along. Never the signal.

> **T4 (THE RESULT — keep ONE variant):**
> *(see §5 — verdict-class variants, one tweet each)*

> **T5 (the honest bounds):**
> Full transparency: {{N_MISSING}} tokens couldn't be analyzed (infra outages in July —
> documented, timestamped, disclosed). The math treats every one of them as
> worst-case. Bounds on the effect: {{D_LO}} to {{D_HI}}.
> We report that instead of hiding it. That's the whole brand.

> **T6 (close / CTA — no URL):**
> Every number in this thread is reproducible from a query.
> The full report, the preregistration history, and the receipts live in our
> Telegram — TrenchAutopsy. The free bot checks any token's death record before
> you ape: TrenchAutopsyBot on Telegram.

## 4. The Telegram long post

> 🔬 **THE N=600 LOOK — {{DATE}}**
>
> Seven weeks ago we committed, in writing, before the data existed, to one question:
> **does the first 15 minutes of net money flow predict which tokens die?**
>
> Death was defined in advance: real quote-side depth below **$2,500** — the level at which
> you cannot actually exit — sustained to a 72-hour verdict, measured by our own poller,
> not an aggregator's number.
>
> The test: **{{N}} tokens** ({{N_EVENTS}} deaths, {{N_CENSORED}} survivors) from a
> preregistered N=600 trigger cohort. One look. The code consumes a permanent lock the
> moment it runs — there is no "run it again until it looks good."
>
> **Result: {{VERDICT_HEADLINE}}**
> Somers' D = {{D}} (C-index {{C_INDEX}}), p = {{P}} against {{N_PERM}} within-block
> permutations. Verdict class under the preregistered mapping: **{{VERDICT_CLASS}}**.
> Worst-case bounds over everything we could not analyze: {{D_LO}} to {{D_HI}}.
>
> *(insert the ONE matching §5 paragraph here)*
>
> What we did NOT do: peek early, move the goalposts, drop awkward tokens, rerun.
> The preregistration went through an independent out-of-loop adversarial review that
> rejected two full drafts before this one; the final version's remediation map is
> public in the repo, including the two bugs the review process caught in our own code.
>
> 🤖 Check any token's death record: @TrenchAutopsyBot · defense only, never advice.

## 5. Verdict-class variants (keep exactly ONE)

**ROBUST** —
> The signal is real. Early net flow ranked deaths better than chance, survived the
> permutation test, and the worst-case bounds don't erase it. What happens next: it goes
> into our own defensive tooling first — we trade edges, we don't sell hopium. The
> defensive product stays free-first and honest.
> *(X variant, ≤280):* Money CAN smell death 15 minutes in. D={{D}}, p={{P}},
> preregistered, one look, bounds hold. We built the receipts so you don't have to
> trust us — you can check us.

**NOT_POINT_IDENTIFIED** —
> The point estimate says {{DIRECTION_PLAIN}}, but July's infrastructure outages cost us
> {{N_MISSING}} tokens, and under worst-case accounting the bounds are too wide to call
> it identified. We're publishing that verdict — "we can't claim it yet" — because
> that's what preregistration means. Accrual continues; a future test gets its own
> preregistration.
> *(X variant):* Verdict: NOT PROVEN. Point estimate {{D}}, but wide worst-case bounds
> — we lost tokens to July outages and we count every one against ourselves.
> Publishing anyway. That's the deal.

**NULL** —
> Money could NOT smell death 15 minutes in — no detectable association. We're publishing
> a null. Nobody publishes nulls in crypto; that's exactly why you should trust our
> death data. The defensive product never depended on this signal: scam filters,
> depth verdicts, and deployer rap sheets are chain facts, not predictions.
> *(X variant):* Result: the crowd's first 15 minutes know NOTHING about which tokens
> die (D={{D}}, p={{P}}). We preregistered, ran it once, and we're publishing the null.
> Show us another team in the trenches that does that.

**ADVERSE** —
> The association ran BACKWARD from the naive story: {{ADVERSE_PLAIN_SENTENCE}}.
> That's a finding, not an embarrassment — it kills a popular heuristic people trade on.
> Full numbers below, bounds included.
> *(X variant):* Plot twist: the signal exists and points the OTHER way
> (D={{D}}, p={{P}}). The heuristic you're trading on may be exactly wrong.
> Preregistered, one look, receipts in the repo.

## 6. Receipts appendix (fill + screenshot on look day)

- Lock row: `b4_signal_look_lock` (status COMPLETE, fingerprint, timestamp).
- Ratification commit `14f93f1` (SPEC flip + signed attestation, 2026-07-29).
- Dress-rehearsal commit `2743dc6` (the path ran end-to-end before it ran for real).
- Attestation log `review/ATTESTATION_LOG_b4_runway.md` (every pre-look read, dated).
- The v13 conformance map (13-item remediation path, including what was NOT initially true).

## 7. Honesty pins (copy may not violate these)

1. Never "rug prediction" — the endpoint is depth-death at 72h, which includes abandonment.
2. PRE/STRADDLE or pooled numbers, if ever cited, carry the G6 banner verbatim.
3. No financial advice; CLEAR ≠ buy; the edge (if any) is traded, not sold (guardrail 4).
4. "Independent adversarial review" — never "audit", never a vendor name.
5. Every published number must be pasteable from `review/N600_LOOK_REPORT.md`.
