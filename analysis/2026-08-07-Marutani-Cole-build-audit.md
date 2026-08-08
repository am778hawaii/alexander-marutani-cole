# Phase 3 Build Audit

**Student:** Alexander Marutani-Cole\
**Course:** FIN 321 · International Business Finance · Summer 2026 (Sec.
701)\
**Model:**
`models/builds/2026-08-07-Marutani-Cole-eur-receivable-one-year-model.xlsx`\
**Scenario:** EUR 4,500,000 receivable due in one year

## Audit Finding 1 --- Forward / Money-Market Parity

**What I checked:** I compared `Forward_Proceeds = FC_AMT × F0_in` with
the three-step money-market hedge using the workbook's live parity
check.

**What I found:** The initial indicative forward placeholder of 1.1242
produced a roughly \$1,503 difference from the money-market hedge,
causing the parity validation to return `REVIEW`. The issue was not the
money-market formulas; the rounded placeholder forward rate was
inconsistent with the interest-rate placeholders.

**What I did:** I recalculated the indicative parity-consistent forward
from `S0_in`, `R_USD`, `R_FC`, and `T_DAYS` and changed the placeholder
`F0_in` to 1.1245340666. The validation now shows a difference of
approximately \$0.00 and returns `PASS`. This is still an indicative
placeholder and will be replaced by live market data in Phase 4.

## Audit Finding 2 --- Option Payoff and Premium Treatment

**What I checked:** I tested the EUR put at, below, and above `K_PUT`
and reviewed whether the premium is included in net proceeds.

**What I found:** The put payoff is formula-driven as
`FC_AMT × MAX(K_PUT − S_T, 0)`, and the USD premium is separately
calculated as `FC_AMT × PREM_PUT`. At the strike, the payoff is zero and
net proceeds connect continuously across the exercise boundary. The
workbook's continuity check returns `PASS`.

**What I did:** I retained the formula and added a visible validation
check. I also kept the call as a separate comparison overlay and
explicitly noted that an EUR call does not protect the downside of an
EUR receivable, preventing it from being misinterpreted as the
recommended hedge.

## Audit Finding 3 --- Sensitivity Table Construction

**What I checked:** I inspected the sensitivity table for the required
±5% range, 1% increments, formula-driven rows, and comparison chart.

**What I found:** The table contains 11 scenarios from 95% through 105%
of `S0_in`. The scenario percentages, `S_T`, and all proceeds are
generated with formulas rather than pasted values. The endpoint and
row-count checks both return `PASS`.

**What I did:** I retained the formula-driven table and verified that
the chart uses the sensitivity outputs for unhedged, forward,
money-market, and put-hedged proceeds.

## Audit Finding 4 --- Named-Range Contract and Formula Integrity

**What I checked:** I inspected the workbook-defined names and scanned
for formula errors.

**What I found:** All ten required workbook-level named ranges are
attached to the intended cells: `FC_AMT`, `S0_in`, `F0_in`, `R_USD`,
`R_FC`, `K_PUT`, `K_CALL`, `PREM_PUT`, `PREM_CALL`, and `T_DAYS`. A
workbook scan found no `#REF!`, `#DIV/0!`, `#VALUE!`, `#NAME?`, or
`#N/A` errors.

**What I did:** No formula repair was required for this finding. I kept
the named-range structure and confirmed the calculated hedge outputs
remain formulas.

## Audit Conclusion

After the audit fix to the indicative forward placeholder, the model
satisfies the Phase 3 build contract: ten named ranges, formula-driven
hedge calculations, three explicit money-market steps, put and call
option analysis with premiums, a formula-driven sensitivity table and
chart, visible validation checks, and the required color-coded
structure. All market values remain placeholders until Phase 4.
