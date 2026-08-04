# Stage 2 review — EUR receivable, one year · Treasury sign-off

Alexander — I read your specification the way Treasury actually reads one: the spec is the contract the build must honor, so the test is whether an analyst could hand this to a modeler (or an AI) and get back the workbook you intended, with the exposure pointed the right way. It passes that test — the architecture is complete and the hedges point correctly for a receivable. What needs attention before you build is the *placeholder set*, because your own headline validation check fails against it.

| Criterion | Score |
|---|---|
| Named-range contract & tab architecture | 30 / 30 |
| Calculation flow | 30 / 30 |
| Validation & sensitivity plan | 20 / 20 |
| Reproducibility & prompt log | 20 / 20 |
| **Total** | **100 / 100** |

**What you did well — and why it matters**

- **Your forward is pointed the right way, which is rarer than it sounds.** With `R_USD` at 4.50% above `R_FC` at 2.25%, the euro must trade at a forward *premium* — and you shipped `F0_in = 1.11` against a spot of 1.10. Getting that direction right means you understand the higher-rate currency trades at a forward discount, rather than reaching for a number that "looks like" a hedge rate.
- **The money-market hedge is built in the correct direction for a receivable.** Discount `FC_AMT` at `R_FC`, convert at spot, invest USD to settlement — and your `Money_Market_Hedge` tab reports the borrowed EUR as a visible output. Inverting the borrow-side currency is the most common error on this stage; you avoided it.
- **The sensitivity plan includes the unhedged baseline.** Plotting `Unhedged_Proceeds` alongside forward, money-market and option is what makes the chart answer the CFO's actual question — *what did hedging buy me?* Several specs omit the diagonal every other line is judged against. Yours doesn't.
- **`Best_Hedge` as a named output is a nice touch.** Forcing the workbook to *declare a winner* rather than leaving four numbers on a page is the difference between a model and an analysis.
- **Your validation section names the right five checks** — parity, option continuity, formula integrity, error scan, named-range discipline. That is the correct shape for a controls list.

**Fix these before you build — they will become defects in the workbook**

- **Your parity check fails against your own placeholders.** §7 says forward and money-market proceeds "should approximately equal after rounding differences." Run your numbers: covered interest parity implies `1.10 × 1.045625 / 1.0228125 = 1.1245`, but you shipped `F0_in = 1.11`. That puts `Forward_Proceeds` at $4,995,000 against `MoneyMarket_Proceeds` of $5,060,403 — a gap of **$65,403, or 1.31%**. That is not a rounding difference; it is a third of a percent past anything you could call one. Set `F0_in` to 1.1245 and the gap goes to exactly $0. You had the direction right and undershot the magnitude — the fix is to *derive* the placeholder from parity rather than estimate it.
- **"Approximately equal" is not a check — put a number on it.** A control with no threshold can't fail, and a reviewer can't tell whether $65,403 is a bug or a basis. Add a `CHECK_TOLERANCE` cell and test the *rate*, not the proceeds: `ABS(F_implied − F0_in) < CHECK_TOLERANCE` with a tolerance around $0.0001–0.001 per EUR. Testing the rate tells you *which* input is wrong; a dollar difference only tells you something is.
- **`F_implied` needs to exist as a named output.** You describe a "covered interest parity comparison" and report a `CoveredInterestParity_Difference`, but the implied forward itself never appears in your named-range contract or output list. It's the number the comparison is *made of* — name it and surface it.
- **`K_CALL` and `PREM_CALL` are orphaned.** Both are in the named-range contract, and neither appears anywhere in §5, §6, or §8. Either use them — a collar, or a payable-reference schedule shown separately and excluded from the receivable recommendation — or drop them from the contract. An input nobody consumes is an invitation for the build to invent a use for it.
- **Your option premiums are inconsistent with your own forward.** `K_CALL = 1.12` sits closer to the money than `K_PUT = 1.08` under *either* forward — 0.0100 vs 0.0300 out-of-the-money against your 1.11, and actually 0.0045 *in* the money against the parity forward of 1.1245. A call nearer the money must cost *more* than a further-out put, yet you priced the call at 0.018 and the put at 0.020. Swap the relationship or re-derive both.

**To push it further (real-desk nuance)**

- **Premium timing: you pay at t=0 and subtract at t=1.** §4 says premiums are "paid immediately" and §5 says to subtract `PREM_PUT` from settlement proceeds. Those are different dates. The premium is $4,500,000 × 0.020 = **$90,000 today**, which is **$94,106** by settlement at `R_USD` — so subtracting the raw $90,000 understates the option's true cost by $4,106. Small here, but the principle is what matters: put every cash flow on the same time footing before you compare strategies, or the option looks cheaper than it is.
- **Continuity is the right check; the kink is the sharper one.** A protective put's payoff is continuous but *not smooth* — it bends exactly at `K_PUT`. Verify that the bend lands at 1.08 and that `Option_Net_Proceeds` at `S_T = K_PUT` equals `K_PUT × FC_AMT − premium` exactly. That's the test that catches an off-by-one in the `MAX` logic, which pure continuity will not.
- **Name your rate basis per leg, not globally.** §4 commits to ACT/360 "unless live market conventions require otherwise" — that escape clause will bite you at Phase 4, when the USD leg comes off SOFR (ACT/360) and the EUR leg off a quote that may not. Decide now what happens when the two legs disagree, rather than discovering it with live data.

**Housekeeping — this nearly cost you the grade**

Your spec is committed at `docs/specs/docs/specs/2026-07-31-Marutani-Cole-eur-receivable-one-year-spec.md` — the canonical path duplicated, so the file sits one wrapper folder deeper than it should. Your Stage 1 memo has the same shape (`docs/decisions/docs/decisions/…`). It's the wrapper-folder issue flagged at Stage 0, still unresolved. Graded on content so there's no penalty, but **this spec went undetected in the first grading pass because of it.** Please fix it now: move the contents of the inner `docs/` up so `docs/specs/` and `docs/decisions/` sit at the repo root, delete the empty nested folders, and commit. It gets more expensive every stage you carry it — by Phase 4 a reviewer looking for your workbook may simply not find it.

**Next — Stage 3**

Hand this spec to the AI to build, then audit what comes back against it. Fix the placeholder set first — a spec that ships an off-parity forward and premiums that contradict it will produce a workbook with exactly those defects, and your audit will be spent rediscovering your own known issues instead of finding new ones. Audit like you expect to find a defect: every calculated cell a formula referencing your named ranges, all three families present, the sensitivity table and chart, and a build-audit note with at least three real findings.

— Treasury

---

### How to work this review — professional workflow

Treat this PR the way an analyst treats feedback from Treasury — a review is a proposal to engage with, not a checklist to rubber-stamp:

1. **Read it yourself first.** Understand each point and form your own view before changing anything. Disagreeing *with a documented reason* is a legitimate, senior response.
2. **Stress-test it with an LLM (pushback pass).** Paste this review and your spec into your AI assistant and ask it to (a) explain anything you're unsure of more deeply, and (b) argue the *other side* — where might the reviewer be wrong, and what would you give up by making each change. You're building judgment, not just executing edits.
3. **Decide, then draft the changes with the LLM.** For the points you accept, have the AI help implement them — you specify exactly what and why. Your spec is the prompt; precise in, correct out.
4. **Verify — non-negotiable.** Re-run your own checks (the parity tie-out, the kink at `K_PUT`, sensitivity continuity, no error cells) and confirm the numbers before you commit. An AI will hand you a confident wrong edit; verification is what makes the result *yours*.
5. **Close the loop on the PR.** Reply in the thread with what you changed, what you pushed back on and why, then commit and push. Writing down the reasoning is exactly how this works on a real team.

*This is the same human-in-the-loop discipline the whole project is built on: the LLM drafts, you edit and verify, and you own the result.*
