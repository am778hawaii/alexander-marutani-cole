# Phase 4 Market-Data Memo

**Student:** Alexander Marutani-Cole  
**Course:** FIN 321 · International Business Finance · Summer 2026 (Sec. 701)  
**Scenario:** EUR 4,500,000 receivable due in one year  
**Retrieval timestamp:** 2026-08-07 14:10 HST

## Input Provenance

| Named range | Phase 4 value | Unit | Source | Retrieval / observation | Proxy or computation |
|---|---:|---|---|---|---|
| `FC_AMT` | 4,500,000 | EUR | Assigned course scenario | Scenario input | Contractual receivable; unchanged |
| `S0_in` | 1.1535 | USD/EUR | ECB euro reference exchange rate: https://www.ecb.europa.eu/stats/policy_and_exchange_rates/euro_reference_exchange_rates/html/eurofxref-graph-usd.en.html | Retrieved 2026-08-07 14:10 HST; ECB value for 2026-08-07 | None |
| `R_USD` | 4.06% | annual rate | FRED DGS1 / Federal Reserve: https://fred.stlouisfed.org/series/DGS1 | Retrieved 2026-08-07 14:10 HST; latest published observation was 2026-08-06 | 1-year Treasury constant-maturity yield used to match the one-year exposure |
| `R_FC` | 2.25% | annual rate | ECB deposit facility: https://www.ecb.europa.eu/stats/policy_and_exchange_rates/key_ecb_interest_rates/html/index.en.html | Retrieved 2026-08-07 14:10 HST; rate effective 2026-06-17 | Used as an explicitly documented EUR reference-rate proxy because a same-day 1-year euro yield was not reliably retrievable |
| `F0_in` | 1.174196 | USD/EUR | Covered interest parity | Computed 2026-08-07 14:10 HST | `S0_in × (1 + R_USD×T_DAYS/360) / (1 + R_FC×T_DAYS/360)` |
| `K_PUT` | 1.1500 | USD/EUR | Phase 4 strike convention | Selected 2026-08-07 14:10 HST | Near-spot put strike, slightly below 1.1535 spot |
| `K_CALL` | 1.1600 | USD/EUR | Phase 4 strike convention | Selected 2026-08-07 14:10 HST | Near-spot call strike, slightly above 1.1535 spot |
| `PREM_PUT` | 0.0200 | USD/EUR | Scenario assumption | Retained in Phase 4 | Scenario-given premium retained per instructions |
| `PREM_CALL` | 0.0180 | USD/EUR | Scenario assumption | Retained in Phase 4 | Scenario-given premium retained per instructions |
| `T_DAYS` | 365 | days | Assigned course scenario | Scenario input | One-year settlement horizon |

## Rate Choices and Forward Computation

For the USD rate, I used the **1-year U.S. Treasury constant-maturity yield** because its maturity directly matches the one-year receivable. At the time of retrieval on August 7, the latest published observation was **4.06% for August 6, 2026**; I documented that one-day publication lag rather than substituting a different maturity.

For the euro side, I used the **ECB deposit facility rate of 2.25%** as a transparent reference-rate proxy. This is not a one-year market yield, so the limitation is explicit. It is preferable to inventing or silently using a stale 1-year quote and can be replaced if a reliable same-day 1-year euro rate is available.

No live one-year forward quote was used. Under the course ACT/360 simple-interest convention, covered interest parity gives:

`F0_in = 1.1535 × (1 + 0.0406×365/360) / (1 + 0.0225×365/360) = 1.174196 USD/EUR`.

This is **0.064196 (5.8%) above** the Phase 2 indicative forward placeholder of 1.1100 and **0.049662 (4.4%) above** the parity-consistent Phase 3 placeholder of 1.124534. The gap reflects the new live spot and rate environment rather than a structural model change.

## Workbook Re-Population and Checks

Only the named-range input cells were repopulated. The hedge formulas and workbook structure did not require repair. With live inputs, the forward and money-market proceeds both equal approximately **$5,283,882.88**, so the workbook parity check returns `PASS`. The sensitivity table automatically rebuilt its eleven scenarios from **0.95×S0_in through 1.05×S0_in**, and the comparison chart moved with the updated spot rate. The option-continuity, sensitivity-row-count, and endpoint checks also return `PASS`.

## FX Hedging Lab Cross-Check

I cross-checked the workbook using the equations published in the course FX Hedging Lab, which specifies the same named ranges, simple-interest ACT/360 convention, three-step money-market hedge, parity formula, and ±5% sensitivity grid.

| Output | Workbook | FX Hedging Lab benchmark | Difference |
|---|---:|---:|---:|
| Forward proceeds | $5,283,882.88 | $5,283,882.88 | $0.00 |
| Money-market proceeds | $5,283,882.88 | $5,283,882.88 | $0.00 |
| Put net proceeds at `S_T = S0_in` | $5,100,750.00 | $5,100,750.00 | $0.00 |
| CIP-implied forward | 1.174196 | 1.174196 | 0.000000 |

There were no unresolved discrepancies. The model therefore survives the Phase 4 live-data population without a structural formula change.
