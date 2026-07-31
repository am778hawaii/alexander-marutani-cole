---

title: "FX Hedging Workbook Technical Specification"
author: "Alexander Marutani-Cole"
date: "2026-07-31"
scenario: "EUR 4,500,000 receivable in one year"
currency: "EUR"
amount: "4,500,000"
settlement: "One year"
status: "Phase 2 Specification"
-------------------------------

# FX Hedging Workbook Technical Specification

## 1. Problem Statement

The company expects to receive **EUR 4,500,000** in one year from a foreign customer. Because the payment will ultimately be reported in U.S. dollars, fluctuations in the EUR/USD exchange rate create uncertainty in the dollar value of the receivable. A decline in the euro before settlement would reduce USD cash inflows and could negatively affect cash flow forecasts, budgeting, and profitability.

This specification defines the complete workbook structure before any Excel model is built. Placeholder market values are **indicative only** and will be replaced with live market data during **Phase 4**.

---

# 2. Named-Range Input Contract

| Named Range | Placeholder Value | Unit        | Phase 4 Data Source               |
| ----------- | ----------------: | ----------- | --------------------------------- |
| FC_AMT      |         4,500,000 | EUR         | Assignment scenario               |
| S0_in       |              1.10 | USD/EUR     | Financial market data (spot rate) |
| F0_in       |              1.11 | USD/EUR     | Bank or market forward quotes     |
| R_USD       |             4.50% | Annual rate | SOFR or comparable USD rate       |
| R_FC        |             2.25% | Annual rate | Euro money-market rate            |
| K_PUT       |              1.08 | USD/EUR     | Option market quote               |
| K_CALL      |              1.12 | USD/EUR     | Option market quote               |
| PREM_PUT    |             0.020 | USD/EUR     | Option premium                    |
| PREM_CALL   |             0.018 | USD/EUR     | Option premium                    |
| T_DAYS      |               365 | Days        | Assignment scenario               |

All calculations must reference these named ranges rather than worksheet cell addresses.

---

# 3. Workbook Architecture

## Cover

* Workbook title
* Student information
* Assignment information
* Version history

## Legend_Key

Defines:

* Currency conventions
* Named ranges
* Color formatting
* Formula conventions
* Units

## Inputs

Contains only editable assumptions and market inputs.

No calculations should appear on this worksheet.

## Forward_Hedge

Calculates USD proceeds using a forward contract.

Displays:

* Forward rate
* Locked exchange rate
* USD proceeds

## Money_Market_Hedge

Calculates the synthetic forward using borrowing and lending.

Displays:

* Present value of receivable
* Foreign borrowing
* USD investment
* Future USD proceeds
* Covered interest parity comparison

## Option_Hedge

Calculates proceeds using currency options.

Displays:

* Option payoff
* Premium cost
* Net proceeds
* Comparison against remaining unhedged

## Sensitivity

Evaluates ending spot-rate scenarios and summarizes hedge performance.

Includes:

* Scenario table
* Comparison chart

## Notes_Assumptions

Documents:

* Assumptions
* Data sources
* Interest-rate conventions
* Modeling limitations
* Revision history

---

# 4. Assumptions and Constraints

* Interest calculations use an ACT/360 convention unless live market conventions require otherwise.
* Placeholder interest rates are illustrative and replaced during Phase 4.
* Transaction costs other than stated option premiums are ignored.
* Forward pricing is expected to approximate covered interest parity.
* Option premiums are paid immediately and treated as a reduction in net proceeds.
* All exchange rates are expressed in USD per EUR.
* All monetary outputs are reported in U.S. dollars unless otherwise noted.

---

# 5. Calculation Flow

## Forward Hedge

1. Read FC_AMT.
2. Read F0_in.
3. Calculate:

Forward_Proceeds = FC_AMT × F0_in

4. Report Forward_Proceeds.

---

## Money Market Hedge

Step 1

Discount FC_AMT using R_FC over T_DAYS.

Compute present value of the euro receivable.

Step 2

Convert discounted euros into USD using S0_in.

Invest resulting USD at R_USD.

Step 3

Compute future USD proceeds at settlement.

Perform a covered interest parity comparison against the forward hedge.

Report:

* Borrowed EUR
* Initial USD investment
* Final USD proceeds
* Difference from forward hedge

---

## Option Hedge

1. Compare ending spot rate S_T with strike K_PUT.
2. Determine exercise decision.
3. Calculate option payoff.
4. Subtract PREM_PUT.
5. Compute:

Option_Net_Proceeds

Repeat calculations across every sensitivity scenario.

---

# 6. Sensitivity Analysis

Evaluate ending exchange rates:

S_T ranges from

0.95 × S0_in

to

1.05 × S0_in

using **1% increments**.

Display:

* Unhedged proceeds
* Forward proceeds
* Money-market proceeds
* Option proceeds

Produce one comparison line chart displaying all strategies across the exchange-rate range.

---

# 7. Validation Rules

The workbook must satisfy all of the following checks.

### Forward vs. Money Market

Forward proceeds should approximately equal money-market proceeds after rounding differences.

### Option Continuity

Net option proceeds must change continuously with S_T and correctly reflect option exercise decisions.

### Formula Integrity

Every output cell must contain a formula.

No manually entered calculated values.

### Error Check

No worksheet may contain:

* #DIV/0!
* #VALUE!
* #REF!
* #NAME?
* #NUM!
* #N/A

### Named Ranges

All formulas reference named ranges rather than worksheet addresses.

---

# 8. Output Summary

The workbook must produce the following summary outputs.

* Forward_Proceeds
* MoneyMarket_Proceeds
* Option_Net_Proceeds
* Unhedged_Proceeds
* Best_Hedge
* CoveredInterestParity_Difference

Summary tables:

* Input Summary
* Hedge Comparison
* Sensitivity Results
* Validation Checklist

---

# 9. Phase Progression

**Phase 3:** Build the workbook directly from this specification using AI assistance while independently auditing every worksheet and formula.

**Phase 4:** Replace all placeholder market inputs with live exchange rates, forward rates, interest rates, and option premiums from current market sources.

**Phase 5:** Validate workbook outputs, perform sensitivity analysis, compare hedge alternatives, and prepare the final recommendation supported by quantitative evidence.
