---
title: "FX Hedge Recommendation"
author: "Alexander Marutani-Cole"
date: "2026-08-07"
scenario: "EUR 4,500,000 receivable in one year"
currency: "EUR"
amount: "4,500,000"
settlement: "One year"
---

# Executive Decision Memo

**To:** Chief Financial Officer  
**From:** Alexander Marutani-Cole  
**Date:** August 7, 2026  
**Subject:** Recommendation — Hedge EUR 4.5 Million Receivable with a One-Year Forward

## A. Exposure Summary

The firm will receive **EUR 4,500,000 in one year**. At the August 7 spot rate of **1.1535 USD/EUR**, the receivable has a current spot-equivalent value of approximately **$5.191 million**, but the final USD proceeds remain uncertain until settlement. A 5% decline in EURUSD from today's spot would reduce unhedged proceeds to approximately **$4.931 million**, a decline of roughly **$260,000** from the current spot-equivalent value.

## B. Hedge Outcomes

The live-data analysis favors the forward and money-market hedges when management's priority is cash-flow certainty. The CIP-implied one-year forward rate is **1.174196 USD/EUR**, locking approximately **$5.284 million** of settlement proceeds. The money-market hedge produces the same **$5.284 million** under the rates used because the forward was derived from covered interest parity.

The put option provides a different risk profile. With a **1.1500 strike** and **0.0200 USD/EUR premium**, the total premium is **$90,000**. The put establishes net proceeds of approximately **$5.085 million** when EURUSD settles at or below the strike, while allowing the firm to participate in euro appreciation above 1.1500. At the current spot rate, put-hedged proceeds would be approximately **$5.101 million**; at a 5% stronger euro, they rise to approximately **$5.360 million**.

Remaining unhedged provides full participation in a stronger euro but also full downside exposure. Across the modeled range, unhedged proceeds vary from approximately **$4.931 million to $5.450 million**. The EUR call comparison does not solve the firm's primary risk: a long call benefits from further EUR appreciation but leaves depreciation exposure in place, so it is not an appropriate standalone hedge for this receivable.

## C. Sensitivity Interpretation

The sensitivity analysis makes the trade-off clear. The forward and money-market strategies are flat across ending exchange rates: they exchange upside for certainty. If EURUSD falls 5% to **1.095825**, the forward protects approximately **$352,670** relative to remaining unhedged and approximately **$198,883** relative to the put's net floor.

The put behaves differently. It limits the downside while preserving most of the benefit of a stronger euro, but the firm pays $90,000 upfront for that flexibility. At the modeled 5% appreciation point of **1.211175**, the put produces approximately **$5.360 million**, about **$76,405 more than the forward**. Unhedged proceeds are higher still at approximately $5.450 million, but that result comes with no downside protection.

Therefore, choosing between the forward and put is primarily a decision about whether upside participation is worth paying a known premium and accepting a lower guaranteed floor.

## D. Recommendation

I recommend **hedging the full EUR 4,500,000 receivable with the one-year forward at approximately 1.174196 USD/EUR**, subject to confirming an executable dealer quote before transacting.

This recommendation locks approximately **$5.284 million**, eliminates the principal FX budget risk, and requires no upfront option premium. The money-market hedge reaches the same modeled result but requires borrowing euros, converting the proceeds, and investing dollars, creating additional operational and liquidity steps without improving the modeled settlement value.

The put is a defensible alternative if management has a deliberate view that retaining EUR appreciation is worth the $90,000 premium. Under the current inputs, however, its guaranteed net floor is approximately **$198,883 below** the forward proceeds. The euro must appreciate sufficiently before the option's flexibility compensates for that difference. For a known contractual receivable, the forward better matches a risk-management objective centered on protecting the operating budget rather than taking a currency view.

## E. Executive Justification

The forward provides the strongest combination of **cash-flow stability, budget certainty, simplicity, and liquidity preservation**. Management knows the approximate USD proceeds in advance, can incorporate that amount into operating and capital plans, and does not need to fund an upfront premium or construct a borrowing-and-investment position.

The recommendation intentionally sacrifices potential gains from a stronger euro. That is appropriate because the purpose of hedging this receivable is to reduce uncertainty in a known business cash flow, not to maximize speculative FX upside. The put should be reconsidered only if management explicitly values upside participation enough to accept both the premium cost and the lower protected floor.

Before execution, Treasury should obtain a live dealer forward quote, compare it with the model's **1.174196** CIP benchmark, verify counterparty and credit terms, and document the final transaction. If the executable quote is materially different from the modeled forward, the forward-versus-put comparison should be refreshed before approval.
