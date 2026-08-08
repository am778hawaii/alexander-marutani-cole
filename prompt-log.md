# AI Prompt Log

This file records meaningful uses of AI in the development of this portfolio. Entries should document what I asked, what the tool produced, what needed correction, and what I changed before using the result.

## 2026-07-24 — Stage 0 Portfolio Setup

**Tool:** ChatGPT by OpenAI

**Purpose:** Create the initial structure and draft content for my public finance portfolio repository.

**Prompt summary:**  
I provided the Stage 0 and onboarding instructions and asked ChatGPT to help me complete Phase 0. I asked it to use my background as a Finance BBA student at the University of Hawaiʻi at Mānoa, my expected Fall 2027 graduation, my food-service management experience, and my career interest in financial advising.

**AI output:**  
ChatGPT drafted:
- A professional repository homepage bio
- A longer `BIO.md`
- A one-page Markdown résumé
- The canonical folder structure and stub README files
- A prompt-log format
- Suggested descriptive commit messages

**My review and edits:**  
Before submission, I will:
- Replace all bracketed contact-information placeholders.
- Confirm that job dates and descriptions are accurate.
- Remove or revise any wording that does not sound like me.
- Confirm that the résumé remains approximately one page when rendered.
- Test the public repository link in an incognito window.

**What the AI got wrong or could not know:**  
The AI did not know my preferred email address, phone number, LinkedIn URL, or final GitHub username. Those fields require my manual input and verification.

**Final responsibility:**  
I reviewed the files and remain responsible for their accuracy, wording, and submission.

---
## 2026-07-31 — Phase 2 Workbook Specification

**Tool:** ChatGPT (OpenAI)

**Purpose:** Draft a technical specification for the FX hedging workbook before any Excel model was built.

**Prompt summary:**
"Create a complete Phase 2 specification for an Excel workbook that evaluates forward, money-market, and option hedges for a EUR 4,500,000 receivable due in one year. Use the required named ranges, workbook architecture, calculation flow, validation rules, and sensitivity analysis."

**Initial AI output:**
The first draft provided the overall workbook structure, input contract, and hedge descriptions.

**Issue I identified:**
The initial draft did not explicitly specify the required sensitivity range (0.95 × S0_in to 1.05 × S0_in in 1% increments) or clearly list the required output names and validation checks.

**Revision made:**
I edited the specification to add the required sensitivity design, named output summaries, covered interest parity validation, formula integrity checks, error checks, and explicit use of named ranges throughout the calculation flow.

**Final responsibility:**
I reviewed the specification, verified it matched the Stage 2 requirements, and remain responsible for the accuracy of the final document.

## 2026-08-07 --- Phase 3 Workbook Generation and Audit
Tool: ChatGPT by OpenAI
**Prompt used:**   
"Generate a working workbook from my Phase 2 specification for the EUR
4,500,000 one-year receivable. The workbook must contain all ten
required named ranges, formulas instead of hard-coded calculated values,
Cover and Legend/Key tabs, forward and three-step money-market hedges,
put and call option analysis including USD premiums, a formula-driven
±5% sensitivity table in 1% steps with a comparison chart, and visible
validation checks. Also audit the workbook against the specification and
document at least three findings."
**AI output:**  
The AI generated the Phase 3 Excel workbook, including the required
tabs, named ranges, formulas, formatting conventions, sensitivity
analysis, chart, and validation block.
Iteration / gap found:  
During the audit, the forward-versus-money-market parity check returned
`REVIEW`. The original indicative `F0_in` placeholder of 1.1242 was
slightly inconsistent with the placeholder spot and interest rates and
created an approximately $1,503 difference.
**Correction made:**  
I directed the model to make the indicative forward placeholder
consistent with covered interest parity. `F0_in` was updated to
1.1245340666, and the validation threshold was tightened. The parity
difference then fell to approximately $0.00 and the workbook returned
`PASS`.
**Additional checks:**  
I verified the put payoff is continuous at the strike and includes the
premium; the sensitivity table contains 11 formula-driven scenarios from
95% to 105% of spot; all ten named ranges are present; and the workbook
contains no obvious Excel error values.
**Final responsibility:**  
I reviewed the generated workbook and audit results. The Phase 3 inputs
are still indicative placeholders and will be replaced with sourced live
market data in Phase 4.

## Reusable Entry Template

### YYYY-MM-DD — Stage or Task Name

**Tool:**  
[Tool and company]

**Purpose:**  
[Why I used AI]

**Prompt summary:**  
[What I asked]

**AI output:**  
[What it produced]

**My review and edits:**  
[What I checked or changed]

**Errors, limitations, or judgment calls:**  
[What the AI missed, misunderstood, or could not verify]

**Final responsibility:**  
[How I verified and took ownership of the final work]
