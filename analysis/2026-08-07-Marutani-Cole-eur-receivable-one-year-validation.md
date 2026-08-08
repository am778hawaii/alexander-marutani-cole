# Phase 5 Validation — EUR 4.5MM One-Year Receivable

**Student:** Alexander Marutani-Cole  
**Course:** FIN 321 · International Business Finance · Summer 2026 (Sec. 701)  
**Date:** August 7, 2026

## Part 1 — Independent LLM Execution

For the production test, the independent analysis was constrained to the Phase 2 specification and Phase 4 market-data memo. No workbook results were supplied as inputs. The exact prompt was:

> Using only the two attached documents, independently compute the complete FX hedge analysis for the stated receivable. Calculate the forward, three-step money-market hedge, put, call, and unhedged outcomes; evaluate them across the required sensitivity range; identify any ambiguities in the documents; and recommend a strategy. Show enough arithmetic that the results can be independently checked. Do not assume access to my Excel workbook.

The raw independent output is saved separately as `2026-08-07-Marutani-Cole-eur-receivable-one-year-independent-llm-output.md`.

## Part 2 — LLM vs. Workbook Comparison

Three ending EURUSD rates were selected from the required sensitivity grid: 95%, 100%, and 105% of live spot. These are 1.095825, 1.153500, and 1.211175 USD/EUR.

| Strategy | S_T | Independent LLM | Workbook | Difference | Diagnosis |
|---|---:|---:|---:|---:|---|
| Unhedged | 1.095825 | $4,931,212.50 | $4,931,212.50 | $0.00 | Reconciled |
| Unhedged | 1.153500 | $5,190,750.00 | $5,190,750.00 | $0.00 | Reconciled |
| Unhedged | 1.211175 | $5,450,287.50 | $5,450,287.50 | $0.00 | Reconciled |
| Forward | 1.095825 | $5,283,882.88 | $5,283,882.88 | $0.00 | Reconciled |
| Forward | 1.153500 | $5,283,882.88 | $5,283,882.88 | $0.00 | Reconciled |
| Forward | 1.211175 | $5,283,882.88 | $5,283,882.88 | $0.00 | Reconciled |
| Money market | 1.095825 | $5,283,882.88 | $5,283,882.88 | $0.00 | Reconciled |
| Money market | 1.153500 | $5,283,882.88 | $5,283,882.88 | $0.00 | Reconciled |
| Money market | 1.211175 | $5,283,882.88 | $5,283,882.88 | $0.00 | Reconciled |
| Put | 1.095825 | $5,085,000.00 | $5,085,000.00 | $0.00 | Reconciled |
| Put | 1.153500 | $5,100,750.00 | $5,100,750.00 | $0.00 | Reconciled |
| Put | 1.211175 | $5,360,287.50 | $5,360,287.50 | $0.00 | Reconciled |
| Call overlay | 1.095825 | $4,850,212.50 | $4,850,212.50 | $0.00 | Reconciled, but economic role is ambiguous |
| Call overlay | 1.153500 | $5,109,750.00 | $5,109,750.00 | $0.00 | Reconciled, but not a downside hedge |
| Call overlay | 1.211175 | $5,599,575.00 | $5,599,575.00 | $0.00 | Reconciled, but increases EUR-upside exposure |

### Discrepancy diagnosis

The numerical outputs reconcile. The important discrepancy is conceptual rather than arithmetic: the Phase 2 specification describes a “call overlay” but does not clearly explain that a long EUR call is not a conventional hedge for an EUR receivable. The course lab clarifies that the put is the relevant option hedge for a receivable and that the call is primarily a comparison case for the reversed exposure. This is a **spec ambiguity**, not a workbook arithmetic error.

## Hand Verification

### 1. Forward hedge

Named-range equation:

`Forward_Proceeds = FC_AMT × F0_in`

Arithmetic:

`= 4,500,000 × 1.1741961951`

`= $5,283,882.88`

**Workbook:** $5,283,882.88  
**Hand calculation:** $5,283,882.88  
**Result:** Reconciled.

### 2. Money-market hedge — all three steps

**Step 1 — Borrow the present value of the EUR receivable**

`EUR_PV = FC_AMT / (1 + R_FC × T_DAYS/360)`

`= 4,500,000 / (1 + 0.0225 × 365/360)`

`= EUR 4,399,633.36`

**Step 2 — Convert borrowed EUR at spot**

`USD_Today = EUR_PV × S0_in`

`= 4,399,633.36 × 1.1535`

`= $5,074,977.09`

**Step 3 — Invest USD until settlement**

`MM_Proceeds = USD_Today × (1 + R_USD × T_DAYS/360)`

`= 5,074,977.09 × (1 + 0.0406 × 365/360)`

`= $5,283,882.88`

**Workbook:** $5,283,882.88  
**Hand calculation:** $5,283,882.88  
**Result:** Reconciled. The equality to the forward is expected because `F0_in` was computed using covered interest parity.

### 3. Put hedge at a 5% EUR depreciation

Use `S_T = 0.95 × S0_in = 1.095825`.

Gross unhedged proceeds:

`FC_AMT × S_T = 4,500,000 × 1.095825 = $4,931,212.50`

Put intrinsic payoff:

`FC_AMT × MAX(K_PUT − S_T,0)`

`= 4,500,000 × (1.150000 − 1.095825)`

`= $243,787.50`

Premium:

`FC_AMT × PREM_PUT = 4,500,000 × 0.0200 = $90,000.00`

Net put proceeds:

`$4,931,212.50 + $243,787.50 − $90,000.00 = $5,085,000.00`

**Workbook:** $5,085,000.00  
**Hand calculation:** $5,085,000.00  
**Result:** Reconciled.

## Spec Retrospective

The production test showed that the specification was buildable, but it was not fully precise about the economic meaning of the call. A context-free reader could follow the literal call-payoff formula and reproduce the workbook, yet still reasonably wonder why a long EUR call belongs in a hedge analysis for an EUR receivable. The specification should have explicitly separated **“receivable hedge”** from **“comparison instrument.”** Version 2 would state that the EUR put is the relevant option hedge because it establishes a downside floor while preserving appreciation upside, whereas a long EUR call does not protect against EUR depreciation and is included only to demonstrate how option direction changes with the underlying exposure.

A second improvement concerns the EUR interest-rate input. The Phase 2 specification said to use a euro money-market rate but did not define a preferred one-year benchmark or a proxy hierarchy. In Phase 4, that required judgment and resulted in the ECB deposit facility rate being used as a transparent proxy. Version 2 would specify a hierarchy: first use a maturity-matched one-year EUR government or money-market yield; if unavailable, use a clearly identified ECB reference-rate proxy and disclose the maturity mismatch.

Finally, the specification should distinguish more clearly between a **live dealer forward quote** and a **CIP-implied forward**. Both are valid inputs for the project, but they serve different validation purposes. Version 2 would require the model to label `F0_in` as either “quoted” or “CIP-implied” and explain that exact forward/MM equality is expected only when the forward itself is generated from the same rate inputs.

## Repo Polish Checklist

- [ ] Top-level README includes current professional bio.
- [ ] README contains an FX Hedging Project section linking Stages 1–5.
- [ ] Repository description is set to a concise finance-portfolio description.
- [ ] All artifacts are stored in canonical `docs/`, `models/`, `data/`, and `analysis/` locations.
- [ ] Stub READMEs accurately describe each folder.
- [ ] `prompt-log.md` includes Phase 5.
- [ ] Raw independent LLM output is committed with or linked from this validation document.
- [ ] Commit history uses descriptive incremental messages.
- [ ] Repository is public and instructor collaborator access remains enabled.
