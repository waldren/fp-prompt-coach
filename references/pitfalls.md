# 15 FM-Specific Prompt Pitfalls

These are the failure modes that show up in Family Medicine education prompts and don't appear in general-purpose prompt engineering guides. Audit every drafted prompt against this list before delivering.

The first 12 are clinical-content failure modes (citations, dosing, currency). The last 3 are pedagogical failure modes (milestone misalignment, framework misuse, content-delivery trap) — these come from the medical education literature on how FM faculty actually teach.

Each pitfall includes a before/after example so faculty using this skill can see the reasoning.

---

## 1. Hallucinated citations and fake DOIs

**The failure:** LLMs generate plausible-looking citations (author names, journal titles, year, DOI, page numbers) that don't exist. This is the highest-frequency, highest-stakes failure mode in EBM prompts.

**Before:**
> "Summarize the latest evidence on SGLT2 inhibitors for heart failure with citations."

**After:**
> "Summarize SGLT2 inhibitor recommendations from the 2022 AHA/ACC/HFSA Heart Failure Guideline. Cite only the guideline document itself by name and section. Do not generate journal article citations, DOIs, or page numbers — I will look those up myself. If you are uncertain whether a recommendation appears in this guideline, write 'I don't have a confirmed source for this' rather than guessing."

**Why this works:** Restricts citations to a single named document the user can verify, and explicitly forbids the most-fabricated fields (DOI, page numbers).

---

## 2. Outdated guidelines presented as current

**The failure:** LLMs trained on data from 2023 may confidently recommend the 2017 ACC/AHA HTN target without flagging that newer guidance exists. Faculty teach the wrong target. Residents fail boards.

**Before:**
> "What is the blood pressure target for a 60-year-old with diabetes?"

**After:**
> "Summarize blood pressure target recommendations for adults with diabetes from the most recent ADA Standards of Care and ACC/AHA hypertension guideline. State your knowledge cutoff date in your response. List the specific guideline edition you are citing. Remind me to verify against the current ADA Standards of Care (published annually each January) before clinical or teaching use."

**Why this works:** Forces the model to disclose its knowledge cutoff and the specific guideline edition, and primes the user to verify currency.

---

## 3. Dosing without weight-based logic

**The failure:** Pediatric, geriatric, and renal-adjusted dosing are common LLM error sources. Models give an adult dose for a 25-kg child or skip renal dose adjustment entirely.

**Before:**
> "What's the amoxicillin dose for otitis media?"

**After:**
> "Describe the AAP-recommended approach to amoxicillin dosing for acute otitis media in children, including the specific weight-based formula (mg/kg/day), maximum daily dose, and the high-dose vs. standard-dose distinction. Do not give a single number for 'the dose' — show the formula. Note that I am responsible for actual patient-specific dosing, and add: 'Confirm against current AAP guidance and a pediatric reference before prescribing.'"

**Why this works:** Refuses to let the model collapse the formula into a number, which is where transposition errors happen.

---

## 4. Off-label use presented as standard

**The failure:** LLMs often discuss off-label uses without flagging them, leading residents to believe an indication is FDA-approved when it isn't.

**Before:**
> "Tell me about gabapentin for hot flashes."

**After:**
> "Describe the evidence for gabapentin in vasomotor symptoms in menopause. Explicitly label this as off-label use in the United States — gabapentin is not FDA-approved for this indication. Cite the strongest available evidence (NAMS position statement, AAFP), include the typical dose range used in trials, and note the limitations of that evidence. Discuss risk-benefit framing for residents."

**Why this works:** Forces the off-label label, which is often the most clinically important fact for a teaching point.

---

## 5. Vague clinical setting

**The failure:** "What's the workup for chest pain?" gets a hospital-based, troponin-and-stress-test answer when the user is teaching outpatient FM. The opposite also happens.

**Before:**
> "Walk through the differential and workup for chest pain."

**After:**
> "Walk through the outpatient Family Medicine approach to a 55-year-old with subacute (2-week) intermittent chest pain in a continuity clinic visit. Frame the differential by likelihood given outpatient FM prevalence (musculoskeletal, GERD, anxiety, stable angina, atypical presentations) — not the inpatient ED differential. Discuss what can be done in clinic, what merits same-day ED transfer, and what merits scheduled cardiology referral."

**Why this works:** Anchors the model to outpatient FM prevalence, which is genuinely different from ED prevalence and where most LLM training data lives.

---

## 6. Ignoring health literacy in patient material

**The failure:** "Plain language" handouts come back at a 12th-grade reading level full of words like "monitor," "indicated," "comorbid," and "as tolerated."

**Before:**
> "Write a handout about managing high blood pressure."

**After:**
> "Write a one-page patient handout about high blood pressure for an adult patient at 6th-grade reading level (Flesch-Kincaid). Replace every clinical term with plain language: not 'hypertension' → 'high blood pressure'; not 'monitor' → 'check'; not 'manage' → 'take care of'; not 'lifestyle modification' → 'changes you can make'; not 'comorbid' → 'other health problems'. Use 'you' and 'your.' Each paragraph ≤3 sentences. Include a clear 'when to call us' section with red flags in plain language."

**Why this works:** Tells the model exactly which jargon to swap, which is more reliable than a generic "use plain language" instruction.

---

## 7. Bias in narrative evaluations

**The failure:** Models pad evaluations with plausible-sounding but unobserved behaviors, and use gendered or racialized descriptors ("articulate," "aggressive," "soft-spoken," "mature for her age").

**Before:**
> "Write a milestone evaluation for an intern."

**After:**
> "Draft a narrative milestone evaluation. I will provide specific behaviors I observed — wait for me to give them before drafting. Do not invent observations. Use only behavioral, observable language ('documents allergies in the medication reconciliation section before signing notes'), never personality language ('she's bright,' 'he's lazy'). Avoid descriptors that have been documented to introduce bias in resident evaluation: 'articulate,' 'aggressive,' 'soft-spoken,' 'mature,' 'pleasant,' 'difficult,' 'hard-working.' Map observations to specific ACGME FM Milestone subcompetencies."

**Why this works:** Names the specific biased descriptors known from the medical education literature, and refuses to draft from nothing.

---

## 8. Sycophancy on clinical reasoning

**The failure:** A resident pastes their differential diagnosis and asks if it's right. The model says "Excellent reasoning!" and adds an enthusiastic agreement, even when the differential is missing the dangerous diagnosis.

**Before:**
> "Here's my differential for this case — does this look right?"

**After:**
> "Critique the following differential diagnosis for [case]. Specifically: (1) Identify any 'can't-miss' diagnoses missing from the list. (2) Identify any items that are unlikely given the clinical features. (3) Identify any items that should be higher or lower in probability. Do not validate the differential as 'good' or 'comprehensive' — your job is to find what's missing or wrong. Be direct. If the differential is genuinely complete and well-ordered, say so, but only after specifically checking for can't-miss diagnoses."

**Why this works:** Structurally forbids generic praise and forces the model to do the most clinically valuable thing — find the gap.

---

## 9. Treating the AI as the clinical decider

**The failure:** Faculty (or worse, residents) ask the AI for *the* answer, then act on it. The AI is supposed to support reasoning, not replace it.

**Before:**
> "What should I do for this patient with HTN that won't come down on three meds?"

**After:**
> "Outline the AAFP and ACC/AHA approach to apparent treatment-resistant hypertension. Include: (1) confirming true resistance (medication adherence, white-coat effect, technique), (2) secondary causes to consider, (3) workup recommended before adding more medication, (4) typical fourth-line agents and when to refer. This is for my own reasoning — I will make the patient-specific decision in clinic. End with: 'This requires individualized clinical judgment beyond what I can provide.'"

**Why this works:** Reframes from "tell me what to do" to "outline the framework I will reason through," and bakes in the refusal clause.

---

## 10. Synthetic case contamination

**The failure:** Asking the AI to write a "realistic" case sometimes produces a case that closely resembles a published case report, including unusual findings that pattern-match to a specific real patient.

**Before:**
> "Write me a realistic case of a patient with Cushing's syndrome for a teaching conference."

**After:**
> "Write a fully synthetic teaching case of an adult patient presenting with features of Cushing's syndrome to an outpatient FM clinic. The case must be: (1) explicitly fictional, not based on any real or published patient, (2) demographically generic (no unusual occupational or geographic details that could pattern-match), (3) clinically representative of the typical Cushing's presentation (not zebra features), (4) include three plausible differential considerations besides Cushing's. At the top, include: 'This is a synthetic teaching case. Any resemblance to real patients is coincidental.'"

**Why this works:** Forces the model toward "typical" rather than "interesting" presentations, which keeps cases pedagogical and reduces accidental real-case echoes.

---

## 11. Missing currency caveat for vaccination, screening, and contraception

**The failure:** Three areas of FM change frequently — vaccine schedules (CDC ACIP updates twice a year), screening intervals (USPSTF updates regularly), and contraception (especially after Dobbs-related state-law changes). LLMs confidently give old answers.

**Before:**
> "What's the recommended cervical cancer screening schedule?"

**After:**
> "Summarize the current USPSTF cervical cancer screening recommendations, including the 2018 update and any subsequent guidance you're aware of. Note: USPSTF, ACS, and ACOG recommendations differ. Compare them. State your knowledge cutoff date. Remind me to verify against the current USPSTF recommendation (https://www.uspreventiveservicestaskforce.org) before teaching, because cervical cancer screening recommendations are an active area of guideline evolution."

**Why this works:** Names the three major guideline-issuing bodies (which differ on this topic) and primes the user to verify.

---

## 12. Pediatric and prenatal blind spots

**The failure:** Generic LLM training data is heavily adult-medicine. Pediatric and prenatal questions are answered with adult logic, with adult dosing, or with outdated pregnancy-category language.

**Before:**
> "Is fluoxetine safe in pregnancy?"

**After:**
> "Describe the evidence on fluoxetine use in pregnancy from the perspective of a Family Medicine clinician managing depression in a pregnant patient. Note that the FDA pregnancy letter categories were retired in 2015 — use current PLLR (Pregnancy and Lactation Labeling Rule) framing, MotherToBaby, and ACOG guidance instead. Address: (1) untreated maternal depression risks, (2) what's known about fluoxetine specifically in pregnancy, (3) the framing of risk-benefit shared decision-making. Do not say 'Category C' — that framework no longer exists."

**Why this works:** Names the specific outdated framework the model is likely to default to (FDA pregnancy categories) and redirects to the current one.

---

## 13. Milestone-level mismatch (year-of-training instead of subcompetency)

**The failure:** Prompts that anchor to "PGY-2 level" alone produce content pitched at an imaginary average — but ACGME FM Milestones 2.0 are explicitly developmental. A real PGY-2 might be Level 2 in PC1 (Acute) and Level 3 in PC2 (Chronic), and content pitched at "PGY-2 generic" misses both.

**Before:**
> "Make me a teaching case for a PGY-2 on heart failure."

**After:**
> "Make me a teaching case targeting ACGME FM Milestones 2.0 PC2 (Chronic Illness) Level 3 — 'Manages chronic illness in patients with multiple comorbidities.' Resident is a PGY-2, but pitch the case to PC2 Level 3 specifically: the patient should have multiple comorbidities (HFrEF + diabetes + CKD stage 3), and the case should require the resident to integrate guideline-directed therapy across all three rather than treat HF in isolation. Include 2 expected behaviors at Level 3 ('reconciles competing recommendations,' 'incorporates patient priorities') and 2 at Level 4 ('leads multidisciplinary planning') so I can use this case to identify whether the resident is performing above or below the target level."

**Why this works:** Anchors to a specific subcompetency and level rather than a year of training, which lets the case actually test what it's meant to test. Also embeds Level 3 vs. Level 4 anchor behaviors so the assessment is calibrated.

---

## 14. Pedagogical framework misuse (SNAPPS as monologue, OMP as lecture)

**The failure:** Faculty ask for "a SNAPPS exercise" and the AI produces a fully pre-filled case where every step is written out — defeating the entire purpose of SNAPPS, which is learner-driven analysis. Similarly, OMP outputs sometimes balloon into mini-lectures rather than the 1-5 minute microskills format.

**Before:**
> "Give me a SNAPPS case for my resident on shortness of breath."

**After:**
> "Generate a SNAPPS-structured case for my PGY-2 on shortness of breath in an outpatient continuity clinic. The framework is learner-led — produce ONLY the patient information the resident would receive (chief complaint, history, exam findings, basic labs). DO NOT pre-fill steps 2-6 of SNAPPS — the resident does the Summarize, Narrow, Analyze, Probe, Plan, and Select steps. Provide a separate facilitator's guide for me with: (1) what a Level 3 PC4 (Undifferentiated) resident's analysis should include, (2) 3 probing questions I can use if the resident's analysis is incomplete, (3) what self-study question the resident should land on. The case is the prompt; the analysis is the resident's work."

**Why this works:** Forces the AI to respect the learner-led nature of SNAPPS by producing only the case stimulus and a separate facilitator key, not a pre-completed walk-through that turns the exercise into a recitation.

---

## 15. Faculty content-delivery trap

**The failure:** Faculty using AI to "prepare for teaching" sometimes get back a fully written script — a monologue they would deliver to passive residents. This contradicts everything the literature shows about how adult learners and residents actually learn, and it loads the faculty member with content rather than tools.

**Before:**
> "Help me prepare to teach my residents about resistant hypertension."

**After:**
> "Help me prepare a 20-minute interactive teaching session on resistant hypertension for my PGY-2 and PGY-3 residents. Do NOT write me a script or lecture text. Instead, produce: (1) Three Socratic questions, ordered from comprehension → application → analysis, that I will pose to drive discussion. (2) A 'brief teaching point' (≤90 seconds when spoken) I can deliver after each question if the discussion needs it. (3) One brief case vignette the residents will work through using SNAPPS structure. (4) A 'pearl card' summary I can hand them at the end. The output is a tool for me to facilitate with, not content for me to deliver."

**Why this works:** Explicitly forbids the lecture/script default and forces the AI to produce facilitation tools rather than monologue text. Aligns with how FM teaching actually works: high-velocity environments where faculty facilitate brief active-learning moments rather than deliver lectures.

---

## Audit checklist before delivering any prompt

Run through these in your head:

1. ✅ Citations limited to verifiable named sources?
2. ✅ Currency caveat if topic is time-sensitive?
3. ✅ Dosing kept as formulas, not numbers, where appropriate?
4. ✅ Off-label uses flagged?
5. ✅ Clinical setting specified?
6. ✅ Reading level locked if patient-facing?
7. ✅ Anti-bias rules in place if assessment?
8. ✅ Sycophancy blocker if asking for critique?
9. ✅ Refusal clause if there's any clinical-decision risk?
10. ✅ Synthetic flag if writing cases?
11. ✅ Currency reminder if vaccination/screening/contraception?
12. ✅ Pregnancy/peds/geriatric specifics if relevant?
13. ✅ Milestones 2.0 subcompetency + level (not just year of training)?
14. ✅ Pedagogical framework respected (SNAPPS learner-led, OMP brief, R2C2 conversational)?
15. ✅ Faculty receives facilitation tools, not lecture scripts?
16. ✅ Productivity-teaching test passed (usable in 3-5 min if for clinic use)?

If any are missing and applicable, add them before delivering the prompt.