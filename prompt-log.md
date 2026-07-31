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
