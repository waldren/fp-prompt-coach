# Safety Triage

Run this screen on every faculty request *before* drafting the prompt. It takes 5 seconds and prevents the most common harms.

The skill audience is faculty — sophisticated users who already know HIPAA exists. The triage is informational and educational, not gatekeeping. When PHI shows up, surface it briefly, walk through de-identification, and proceed.

## Step 1: PHI screen

Look at the user's request and any examples or context they pasted. Flag if you see any of the 18 HIPAA identifiers:

1. Names
2. Geographic subdivisions smaller than state (street, city, county, ZIP, precinct — except first 3 digits of ZIP if the population in that ZIP-3 region is >20,000)
3. All elements of dates (except year) directly related to an individual — DOB, admission date, discharge date, death date — and all ages over 89
4. Phone numbers
5. Fax numbers
6. Email addresses
7. Social Security numbers
8. Medical record numbers
9. Health plan beneficiary numbers
10. Account numbers
11. Certificate/license numbers
12. Vehicle identifiers and serial numbers, including license plates
13. Device identifiers and serial numbers
14. Web URLs
15. IP addresses
16. Biometric identifiers (fingerprints, voiceprints)
17. Full-face photos and comparable images
18. Any other unique identifying number, characteristic, or code

### If PHI is present

Briefly note it, then walk through de-identification and continue. Don't moralize. Something like:

> Quick note before I draft this: the example you shared has [specific identifier — e.g., "the patient's name and DOB"]. Public LLMs like ChatGPT/Claude/Gemini aren't covered under your institution's BAA, so let me strip those before we build the prompt. Here's the de-identified version:
>
> [Show the cleaned version using Safe Harbor: replace names with role descriptors, dates with relative terms, MRN/address/phone removed, ages over 89 rounded to "90+", specific institutions genericized.]
>
> Want me to proceed with this version? If you have an institutional AI tool with a signed BAA, you could use the original.

Then proceed with the prompt as soon as the faculty member confirms.

### Safe Harbor reminders for de-identification

- Replace names with role descriptors: "a 67-year-old woman" not "Mrs. Smith"
- Replace specific dates with relative terms: "two weeks ago" not "October 14"
- Strip MRNs, addresses, phone numbers entirely — they aren't needed for educational use
- Round ages over 89 to "90+"
- Replace specific institutions with generic terms: "the inpatient service" not "Memorial Hospital"
- Remove rare-disease-plus-rare-demographic combinations that could re-identify even without explicit identifiers

### When to recommend an institutional tool instead

If the de-identified version loses too much clinical specificity to be useful, suggest the institutional AI tool with BAA coverage as the better venue. Common examples:

- Microsoft Copilot for M365 with EDU/enterprise tenancy and BAA
- Institutional ChatGPT Enterprise / Edu with BAA
- Epic-integrated AI features (separate compliance framework)
- On-device or institutionally-hosted local models

The skill doesn't need to know each institution's specific approved-tool list — just remind the faculty member to check.

## Step 2: Scope-of-practice screen

Faculty legitimately use AI to support clinical decision-making — that's a supported use case. The question isn't "is this clinical?" but "is this asking AI to make the decision, or to support the clinician making it?"

**Decision support (fine — proceed):**
- "Help me organize the workup options for treatment-resistant hypertension"
- "What are the AAFP-recommended next steps when first-line therapy fails for X?"
- "Walk through the differential for fatigue in an adult primary care patient"
- "Summarize the evidence on Y for shared decision-making with patients"

**Decision replacement (reframe before drafting):**
- "Tell me what to prescribe for this patient"
- "What's the diagnosis?"
- "What dose should I write?"
- "Should I admit this patient?"

The distinction is whether the clinician's judgment sits between the AI output and the action on the patient. Decision support prompts should still include a refusal clause to keep the model from drifting:

> If a question would require individualized clinical judgment beyond educational generalities, respond: "This requires the clinician's individualized judgment — I can outline general principles only."

For genuinely educational uses ("a hypothetical case of a 55-year-old with chest pain"), this rail isn't strictly necessary but doesn't hurt and is good modeling.

For clinical decision support specifically, also build in:
- A "show your reasoning" rule so the output is transparent and verifiable, not just a recommendation
- A "list what would change your answer" clause — what missing information would shift the recommendation
- A reminder that the response is not a substitute for clinical judgment, ideally framed as the model's stated limitation rather than a disclaimer footer

## Step 3: Evidence-grounding screen

Decide which evidence tier the prompt needs and lock the model to it. Not every prompt needs the same rigor.

| Use case | Evidence tier required |
|---|---|
| Patient education handout | USPSTF, AAFP, AHRQ, plain-language NIH/CDC summaries |
| Clinical decision support | USPSTF, specialty society guidelines, UpToDate-equivalent — must include grade of recommendation |
| Resident didactic / board prep | USPSTF, specialty society guidelines (AAFP, ACOG, AAP, ACC/AHA, ADA, etc.), UpToDate equivalent |
| EBM journal club | Original peer-reviewed literature; specify study design tier |
| QI project | IHI, AHRQ, institutional QI literature |
| Administrative / curriculum | ACGME milestones, AAFP curriculum, RC-FM program requirements |
| Synthetic case writing | No external evidence needed; flag that cases are synthetic |

The selected tier becomes the **Evidence Rail** in the final prompt. The rail should always include three rules:

1. **Source priority** — name the acceptable sources explicitly
2. **Citation discipline** — require source + recommendation grade (USPSTF letter grade, SORT A/B/C, GRADE quality), and forbid fabricated citation numbers/DOIs
3. **Uncertainty disclosure** — require the model to say "I don't have a confirmed source" rather than guessing

## Step 4: Currency check

LLMs have knowledge cutoffs. Guidelines change. For any prompt where guideline currency matters (screening intervals, vaccination schedules, hypertension targets, lipid management, contraception, prenatal care), include this line:

> Note your knowledge cutoff date in your response. Recommend that the user verify current recommendations against [USPSTF / AAFP / CDC ACIP / specialty society] before clinical or teaching use.

## Step 5: Academic integrity screen

If the user's request looks like it could be ghostwriting (drafting a manuscript, abstract, or scholarly paper they intend to submit), gently surface the AI-disclosure norms:

- Most journals now require disclosure of AI assistance
- AI cannot be a listed author
- Verbatim AI-generated text in a peer-reviewed submission without disclosure violates most publisher policies

This is not a refusal — drafting and editing assistance is fine. But the prompt should encourage the user to treat output as a starting point, not finished copy, and the strategy note should remind them of disclosure if relevant.

## Quick triage summary

For every request, in your head:

1. Any of the 18 HIPAA identifiers? → De-identify first.
2. Does this ask AI to make a real clinical decision? → Add refusal clause.
3. What evidence tier? → Lock the rail.
4. Are guidelines time-sensitive? → Add currency caveat.
5. Is this ghostwriting? → Flag disclosure norms.

If all five are clear, proceed to drafting.