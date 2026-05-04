# FM Use Cases

The six buckets that cover ~95% of Family Medicine educator prompt requests. Match the user's request to a bucket, then use the framework, safety considerations, and example prompts for that bucket.

## Table of contents

1. Clinical Teaching (didactics, case conferences, morning report)
2. Clinical Decision Support (diagnostic reasoning, workup organization, treatment options)
3. Patient-Facing Material (handouts, scripts, discharge instructions)
4. Assessment & Feedback (milestone evaluations, narrative comments, OSCE)
5. EBM & Research (PICO, journal club, literature appraisal)
6. Administrative & QI (curriculum, schedules, PDSA cycles)

---

## 1. Clinical Teaching

**What this looks like:** Faculty preparing didactic content, board-style review, case conferences, "topic of the week" teaching, or in-clinic precepting for residents and students.

**Recommended framework:** EBM-Anchor, RTF-Med, or BRAIN (see frameworks.md). For in-clinic teaching specifically, use BRAIN paired with an OMP or SNAPPS overlay (see pedagogical-frameworks.md).

**Safety priorities:**
- Evidence rail locked to USPSTF + specialty society guidelines
- Currency caveat for any rapidly-changing topic (HTN, lipids, diabetes, contraception, vaccines)
- Refusal clause not strictly required since this is hypothetical/educational, but include it if the prompt could drift toward "what would you do for a patient with..."

**Required prompt elements:**
- ACGME FM Milestones 2.0 subcompetency + level the topic addresses (e.g., PC2 Level 3, MK1 Level 2). See `references/pedagogical-frameworks.md` for the subcompetency table. This is more useful than "PGY-2" alone.
- Bloom's level (recall, application, analysis, evaluation)
- Setting (outpatient is the FM default, but specify if OB, inpatient, ED, nursing home, telehealth)
- Time available for the teaching session — drives length of output AND framework choice (1-5 min → OMP overlay; 10-15 min → SNAPPS overlay; full didactic → no overlay needed)
- Pedagogical framework if this is interactive teaching (OMP for busy clinic moments, SNAPPS for learner-led reasoning, MAL for reflection)

**Worked example — input:**
> "I need to prep a 30-minute didactic on diabetic foot exam for my PGY-2s next week."

**Worked example — output prompt:**

```
ROLE: You are an experienced Family Medicine educator preparing material for PGY-2 residents.

LEARNER: PGY-2 Family Medicine residents in an outpatient continuity clinic context. Target ACGME FM Milestones 2.0 subcompetencies: PC2 (Chronic Illness, Level 3 — manages chronic illness with multiple comorbidities) and PC5 (Procedural Care, Level 2-3 — performs basic exam components independently). Target Bloom's level: application (residents will perform exams the next day).

TASK: Produce a 30-minute didactic outline on the diabetic foot exam.

EVIDENCE RAIL:
- Cite only ADA Standards of Care, AAFP, USPSTF, and IWGDF guidelines.
- For each recommendation, include source and recommendation grade (e.g., ADA Grade B, SORT A).
- Note your knowledge cutoff date and remind me to verify against current ADA Standards of Care before teaching.
- If you are uncertain a guideline says what I'm asking, write "I don't have a confirmed source" rather than guessing. Do not fabricate citation numbers or DOIs.

OUTPUT FORMAT:
1. Three learning objectives (action verbs, measurable, mapped to PC2 and PC5)
2. Pre-test: 3 multiple-choice questions with rationales
3. Outline (20 minutes of content): epidemiology, key history elements, exam components (inspection, neuropathy testing including monofilament and tuning fork, vascular exam, footwear), risk stratification, follow-up frequency by risk
4. Skill demo plan: what to demonstrate live (5 minutes)
5. Post-test: 2 case-based questions with rationales
6. Three "pearls" residents should leave with

CONSTRAINTS:
- Total reading time for outline ≤ 8 minutes
- US-based practice context
- Adult patients only

SUCCESS CRITERIA:
- Every clinical recommendation traceable to a cited guideline
- Pre/post questions are board-style with one best answer
- Outline timing fits in 30 minutes including discussion

If a question would require individualized clinical judgment beyond educational generalities, respond: "This requires individualized clinical judgment — refer to the supervising physician."
```

**Strategy note:** Locked evidence to ADA + AAFP + IWGDF, forced grade-of-recommendation on every claim, anchored to specific Milestones 2.0 subcompetencies (PC2/PC5) at named levels — more developmentally precise than "PGY-2." Pre/post structure ensures the didactic is teachable, not just readable.

### Variant: in-clinic precepting (OMP overlay)

When a faculty member asks for help teaching a case during clinic — not a scheduled didactic — overlay One-Minute Preceptor structure. The prompt should produce output that fits in a 3-5 minute teaching moment between patients. Example faculty request: "Help me teach this case of new-onset afib to my resident in clinic." See `references/pedagogical-frameworks.md` § OMP for the prompt shell.

### Variant: learner-led reasoning (SNAPPS overlay)

When the goal is to develop the resident's diagnostic reasoning rather than transmit content, overlay SNAPPS structure. The prompt should produce a worksheet or scenario that the resident drives, not a pre-filled analysis. Example faculty request: "Set up a SNAPPS exercise on chronic fatigue for my second-year." See `references/pedagogical-frameworks.md` § SNAPPS.

---

## 2. Clinical Decision Support

**What this looks like:** Faculty using AI to help organize their own clinical thinking — differential diagnosis brainstorming, workup options for a complex case, summarizing the evidence behind a treatment choice, structuring shared decision-making conversations. The AI supports the clinician's reasoning; the clinician makes the call.

**Recommended framework:** EBM-Anchor with reasoning-transparency additions

**Safety priorities:**
- Show-your-reasoning rule — output should expose the logic, not just the conclusion, so the clinician can verify each step
- "What would change my answer" clause — force the model to name the missing information that would shift its recommendation
- Refusal clause for individualized prescription decisions, dosing, or specific patient management
- De-identification confirmed before drafting (faculty often arrive with semi-PHI in their head)

**Required prompt elements:**
- Clinical question, narrowly framed (a tightly-scoped question gets a useful answer; a broad one gets a textbook)
- Patient phenotype in synthetic terms (de-identified)
- What the clinician has already considered or tried (prevents the model from re-stating the obvious)
- What kind of output is most useful — list of options, decision tree, summary of evidence, or framework

**Worked example — input:**
> "I have a patient — 64yo with persistent A1c 9.2 on metformin and glipizide, BMI 36, eGFR 58, no CV disease. I'm thinking GLP-1 vs SGLT2. Help me think through it."

**Worked example — output prompt:**

```
ROLE: You are a Family Medicine clinical educator helping me think through a treatment intensification decision. You support my reasoning; you do not replace my clinical judgment.

CASE (synthetic, de-identified): Adult in their mid-60s with type 2 diabetes inadequately controlled (A1c 9.2%) on metformin + sulfonylurea. BMI 36. eGFR 58. No known CV disease. No prior GLP-1 or SGLT2 use.

TASK: Walk me through the considerations for adding a GLP-1 receptor agonist vs. an SGLT2 inhibitor as the next step. I will make the patient-specific decision.

EVIDENCE RAIL:
- Anchor recommendations to current ADA Standards of Care and AAFP guidance.
- For each recommendation, name the source and the recommendation grade.
- State your knowledge cutoff date and remind me to verify against the current ADA Standards of Care (updated each January).
- Do not fabricate citations, page numbers, or DOIs.

OUTPUT FORMAT:
1. The 3-4 patient factors most relevant to this choice, and what each suggests
2. GLP-1 considerations: efficacy on A1c and weight, renal/cardiovascular benefits/limits in this profile, contraindications, practical issues (cost, formulary, injection)
3. SGLT2 considerations: same structure
4. The case for each in this specific phenotype
5. What would change your answer — list 3-5 patient or contextual facts that would clearly tip the decision one way
6. Open questions I should ask the patient before deciding

REASONING TRANSPARENCY: Show your reasoning for each recommendation. Don't just state a conclusion — name which guideline supports it and what aspect of the patient's phenotype invokes it.

SAFETY: If a question would require individualized clinical judgment beyond educational generalities, respond: "This requires the clinician's individualized judgment — I can outline general principles only." Do not recommend specific dosing — name the drug class and refer to standard dosing references.
```

**Strategy note:** Decision support prompts must show reasoning, not just conclusions — that's how the clinician can audit the AI's logic. The "what would change your answer" clause forces the model to surface its uncertainty. Dosing is deliberately offloaded so the model doesn't transpose a number that could end up acted on.

---

## 3. Patient-Facing Material

**What this looks like:** Handouts, after-visit summaries, discharge instructions, telephone scripts, motivational interviewing scripts, vaccine-hesitancy responses.

**Recommended framework:** Patient-Ed-Frame (see frameworks.md)

**Safety priorities:**
- Reading level must be specified — default to **6th-grade Flesch-Kincaid** for general handouts, **4th-grade** for discharge instructions, **8th-grade** for prenatal/chronic disease education
- Health-literacy-conscious language: no "monitor," no "manage," no "as tolerated"
- Plain-language alternatives mandated for medical jargon
- Cultural humility: no assumptions about family structure, food, religion, or language
- Action items must be concrete: "Take this pill in the morning with food" not "take as directed"
- Always include a "call your doctor if..." section with red flags

**Required prompt elements:**
- Reading grade level (specify the number)
- Language (English only, or also include Spanish translation, etc.)
- Patient population (general adult, pediatric, geriatric, prenatal, low-literacy)
- Use case (in-clinic handout, take-home, post-visit summary, before-visit prep)
- Length cap

**Worked example — input:**
> "Make me a handout about starting metformin."

**Worked example — output prompt:**

```
ROLE: You are a Family Medicine clinician writing patient education for a general adult outpatient clinic.

TASK: Write a one-page patient handout titled "Starting Metformin" for an adult patient newly prescribed metformin for type 2 diabetes.

CLINICAL CONTEXT: General adult outpatient population, mixed health literacy, US clinic setting.

EVIDENCE RAIL:
- Base content on ADA Standards of Care and FDA labeling.
- Where you are uncertain about specific dosing or side-effect frequency, write "talk to your doctor or pharmacist" rather than guessing a number.
- Do not include drug interactions beyond the most common 3 — refer to the pharmacist for the full list.

OUTPUT FORMAT:
- Flesch-Kincaid grade level: 6th grade or below
- Plain-text headers, short paragraphs (≤3 sentences each)
- Use "you" and "your" throughout
- Sections in this order:
  1. What metformin does (2 sentences)
  2. How to take it (specific: with food, time of day, what to do if missed)
  3. What side effects to expect in the first 2 weeks
  4. What side effects mean call the clinic
  5. Red flags — call 911 or go to ER (very rare)
  6. What to know about labs (A1C, kidney function)
  7. Lifestyle reminders (1-2 sentences, non-judgmental)

CONSTRAINTS:
- ≤500 words total
- No medical jargon. Replace every clinical term with plain language. If you must use a clinical term, define it in the same sentence.
- No assumptions about diet, exercise capacity, or family structure
- Do not include specific dose numbers — say "the dose your doctor prescribed"

SUCCESS CRITERIA:
- A 6th-grader could read this aloud and understand it
- Every action the patient should take is concrete and specific
- Red-flag section is unambiguous

SAFETY: This is general patient education, not personalized medical advice. Include this exact line at the bottom: "This handout is general information. Always follow what your doctor and pharmacist tell you specifically about your medicine."
```

**Strategy note:** 6th-grade reading level locked, jargon-replacement rule explicit, dose numbers deliberately offloaded to the clinician (prevents AI from inventing or transposing dosing). Red-flag section structured so it can't be missed.

---

## 4. Assessment & Feedback

**What this looks like:** Milestone evaluations, narrative comments, formative feedback conversations, OSCE case writing, board-style MCQ writing, ABFM ITE prep.

**Recommended framework:** Assessment-Frame for evaluations and MCQs; OSCE-Builder for OSCE; Assessment-Frame with R2C2 overlay for feedback conversations (see frameworks.md and pedagogical-frameworks.md).

**Safety priorities:**
- Specific, behavioral, criterion-referenced language (not personality)
- Bias check: avoid gendered, racialized, or culturally-loaded descriptors
- ACGME FM Milestones 2.0 alignment with specific subcompetencies (PC1-5, MK1, SBP1, PROF1, PBLI, ICS) — see `references/pedagogical-frameworks.md` for the subcompetency reference
- For board questions: must be original (not copied), single-best-answer, with rationales for both correct and distractors
- For feedback conversations: use R2C2 structure (Relationship → Reactions → Content → Coaching)

**Required prompt elements:**
- For evaluations: specific observed behaviors, ACGME FM Milestones 2.0 subcompetency + level
- For MCQs: topic, learner level, exam being prepped (USMLE Step 1/2/3, COMLEX, ITE, ABFM), Bloom's level
- For OSCE: clinical scenario, expected behaviors, scoring rubric
- For feedback conversations: time available (full R2C2 vs. R2C2 in-the-moment), specific behaviors observed, whether the feedback is confirming or disconfirming relative to the resident's likely self-assessment

**Worked example — input:**
> "Help me write a stem for a board question about COPD exacerbation."

**Worked example — output prompt:**

```
ROLE: You are an experienced Family Medicine question-writer preparing original ABFM ITE-style items.

TASK: Write one original single-best-answer multiple-choice question on outpatient COPD exacerbation management.

LEARNER: PGY-2 Family Medicine resident preparing for the ABFM In-Training Exam.

EVIDENCE RAIL:
- Anchor the correct answer to current GOLD Report and AAFP recommendations.
- Note your knowledge cutoff and remind me to verify the answer against the current GOLD Report before using.
- Do not fabricate references or page numbers.

OUTPUT FORMAT:
1. Clinical vignette (≤120 words, third person, includes pertinent positives and negatives)
2. Lead-in question (single sentence, "What is the most appropriate next step?")
3. Five answer options labeled A-E
4. Correct answer
5. Rationale for the correct answer (3-4 sentences, cite source + grade)
6. Rationale for each distractor — why it's wrong
7. One-line teaching point

CONSTRAINTS:
- Original — do not reproduce existing question banks
- Bloom's level: application or analysis (not simple recall)
- One best answer; distractors must be plausible but clearly distinguishable on careful reading
- Vignette is gender-balanced across questions; vary patient demographics across a question set if writing multiple
- No trick questions, no negative lead-ins ("which is NOT...")

SUCCESS CRITERIA:
- Question tests clinical reasoning, not memorization
- Each distractor has a clear teaching reason for being wrong
- A resident who knew the GOLD criteria would get this right
```

**Worked example — narrative evaluation input:**
> "I need to write a milestone comment for my intern who's struggling with documentation."

**Worked example — output prompt:**

```
ROLE: You are an FM core faculty member writing a narrative milestone comment.

TASK: Draft narrative feedback for an intern in their outpatient continuity clinic regarding documentation.

CONTEXT I WILL FILL IN:
- Specific observed behaviors (you will ask me for these)
- Frequency (rare / sometimes / consistent)
- Trajectory (improving / plateaued / declining)
- ACGME FM Milestones 2.0 subcompetency + level most relevant (e.g., PBLI Level 1-2, ICS1 Level 2)

EVIDENCE RAIL: Use ACGME FM Milestones 2.0 language. Map observations to specific subcompetencies and levels (e.g., "PBLI3 Level 2: integrates evidence into patient care"). Reference the subcompetency anchor descriptors when describing growth areas.

OUTPUT FORMAT:
- 3 paragraphs:
  1. Strengths observed (specific, behavioral, mapped to subcompetency)
  2. Area for growth (specific, behavioral, framed as next-level milestone behavior — e.g., "next-level Level 3 behavior would include...")
  3. Concrete next steps (≤3 actions, time-bound)
- Use the SBI model (Situation-Behavior-Impact)
- 200-300 words

CONSTRAINTS:
- No personality language ("she's lazy," "he's bright"). Behavior only.
- No gendered, cultural, or appearance-based descriptors
- Avoid documented bias-prone terms: "articulate," "aggressive," "soft-spoken," "mature," "pleasant," "difficult," "hard-working"
- Avoid "good job" / "needs improvement" — use specific milestone-anchored language
- Do not write anything I haven't told you I observed. If I don't give you a specific behavior, ask before drafting.

SUCCESS CRITERIA:
- Every claim is behavioral and observable
- Milestone language is correctly mapped to the specific subcompetency and level
- Resident reading this would know exactly what to do differently to demonstrate next-level behaviors
```

**Worked example — feedback conversation input:**
> "I observed my PGY-1 miss the diagnosis of PE in a young woman with pleuritic chest pain in clinic — she anchored on costochondritis. I need to give her feedback before her next clinic. Help me prepare the conversation."

**Worked example — output prompt:**

```
ROLE: You are a Family Medicine educator helping me prepare an R2C2 in-the-moment feedback conversation (5-8 minutes).

LEARNER CONTEXT: PGY-1 Family Medicine resident, outpatient continuity clinic, 6 months into intern year. Specific observation: anchored on a musculoskeletal diagnosis (costochondritis) in a young woman with pleuritic chest pain and missed considering pulmonary embolism. Patient was ultimately diagnosed with PE in the ED later that day. Trajectory: this is the first time I've observed an anchoring error from this resident; she is generally diligent and self-aware. Likely a disconfirming-feedback situation — she will not have realized the magnitude of the miss.

TASK: Generate the R2C2 in-the-moment feedback prompts I will use, structured across all four phases.

R2C2 STRUCTURE:
1. RELATIONSHIP — 1-2 opening phrases that set the stage and signal that this is a learning conversation, not a punitive one. Acknowledge that we're going to debrief a specific case.
2. REACTIONS — 2-3 questions inviting her self-assessment. Include language for handling disconfirming feedback (when her self-assessment is likely off). Phrase questions so they don't immediately reveal that I think she missed something — let her surface it if she can.
3. CONTENT — 2 phrases for confirming shared understanding of what happened, and for sharing my observations if she doesn't surface them. Frame around clinical reasoning, not character.
4. COACHING — a Learning Change Plan template:
   - 1-2 specific goals (e.g., "Use Wells score or PERC for any chest pain in clinic for the next month before settling on a non-PE diagnosis")
   - Measurable outcomes
   - Named resources (e.g., specific UpToDate topic, AAFP article)
   - Check-in date
   - Map to ACGME FM Milestones 2.0 PC4 (Undifferentiated Signs) — what does Level 3 behavior look like for this competency?

EVIDENCE RAIL: Reference current PE workup standards (Wells score, PERC, age-adjusted D-dimer). Note knowledge cutoff. Don't fabricate citations.

ANTI-BIAS RULES:
- Behavior only — no personality language
- Avoid bias-prone descriptors: "articulate," "aggressive," "soft-spoken," "mature"
- Frame the anchoring error as a cognitive process common to all clinicians, not a character flaw

NO FABRICATION: Do not invent details about the encounter beyond what I've described. If you need more context to write a phase, ask.

CONSTRAINT: This must fit in 5-8 minutes. Each phase gets 1-2 minutes maximum. No long monologues — these are conversation prompts I'll use, not a script I'll read.
```

**Strategy note:** Anti-bias rule is structural ("behavior only," "no gendered descriptors"), not just hopeful. The prompt forbids the AI from inventing behaviors — a known failure mode where models pad evaluations with plausible-sounding but unobserved details. The R2C2 overlay is what converts a static "feedback comment" into a structured conversation faculty can actually run in 5-8 minutes; without it, AI tends to produce monologues that don't work in real preceptor time. Mapping the growth area to a specific Milestones 2.0 subcompetency (PC4) makes the feedback portable to the next CCC review.

---

## 5. EBM & Research

**What this looks like:** PICO question development, literature search strategy, journal club facilitation, critical appraisal worksheets, scholarly project design.

**Recommended framework:** EBM-Anchor (see frameworks.md)

**Safety priorities:**
- LLMs hallucinate citations. The prompt must explicitly forbid invented references.
- Citation verification clause: model must caveat that user verify each citation in PubMed
- Study design literacy: prompt asks the model to identify study type and limitations, not just summarize results

**Required prompt elements:**
- Specific clinical question (PICO components)
- Study design tier preferred (systematic review > RCT > cohort > case-control > case series)
- Whether the model is helping develop the search strategy, appraise an article the user pasted, or facilitate journal club discussion

**Worked example — input:**
> "Help me prep journal club for the SPRINT trial follow-up paper."

**Worked example — output prompt:**

```
ROLE: You are a Family Medicine educator facilitating an EBM journal club for residents.

TASK: Generate a journal club facilitation guide for the SPRINT trial extended follow-up publication.

EVIDENCE RAIL:
- Do not summarize the paper from memory. I will paste the abstract and methods, and you will work only from what I paste.
- If I have not pasted a section you need to comment on, ask me for it rather than guessing.
- Do not generate citation numbers, DOIs, or page numbers — I will verify the source.

OUTPUT FORMAT:
1. PICO restatement of the trial's research question
2. Five Socratic discussion questions for residents, ordered from comprehension → application → critical appraisal
3. Critical appraisal checklist using AAFP's POEM framework: Patient-Oriented? Outcomes that matter? Methods sound?
4. Three external-validity considerations specific to a Family Medicine outpatient population
5. One question about how (or whether) this changes practice for an FM clinic
6. Suggested takeaway for residents (1-2 sentences, hedged appropriately)

CONSTRAINTS:
- Do not invent statistics that are not in what I paste
- Distinguish between what the paper shows and what would be a reasonable inference
- Do not recommend specific BP targets without sourcing them to the paper or current guidelines

SUCCESS CRITERIA:
- A resident reading the paper alongside this guide can run a 45-minute journal club
- The appraisal forces residents to evaluate the paper, not just recap it
- All claims about the paper are grounded in pasted text, not inferred
```

**Strategy note:** "Work only from what I paste" is the single most important EBM rule — it shuts down hallucinated citations and statistics, which are the most common LLM failure mode in research contexts.

---

## 6. Administrative & QI

**What this looks like:** Curriculum mapping, schedule drafts, QI project planning, root-cause analysis, PDSA cycles, policy drafts, recruitment material.

**Recommended framework:** QI-Frame or RTF-Med (see frameworks.md)

**Safety priorities:**
- Less PHI risk, but still applicable for QI projects involving clinic data
- ACGME RC-FM program requirements as evidence anchor for curriculum work
- AAFP / Society of Teachers of Family Medicine (STFM) resources for educational design
- IHI / AHRQ for QI methodology

**Required prompt elements:**
- Specific deliverable (curriculum, schedule, AAR, PDSA worksheet, policy)
- Stakeholder audience
- Constraints (time, ACGME requirements, institutional norms)
- Whether the user wants a draft to edit or a brainstorm to react to

**Worked example — input:**
> "Help me design a PDSA cycle for improving HPV vaccination rates in our clinic."

**Worked example — output prompt:**

```
ROLE: You are a Family Medicine QI educator helping residents design a PDSA cycle.

TASK: Generate a PDSA cycle worksheet for improving adolescent HPV vaccination completion rates in an outpatient FM clinic.

EVIDENCE RAIL:
- Use IHI Model for Improvement methodology.
- Anchor recommended interventions to AAFP, CDC ACIP, and the Community Preventive Services Task Force.
- For each suggested intervention, indicate evidence strength (strong / moderate / limited / unknown). Do not fabricate evidence grades.

OUTPUT FORMAT:
- Aim statement (SMART format) — provide a template with placeholders for clinic-specific numbers
- Measures: 1 outcome, 2 process, 1 balancing — with operational definitions
- Two candidate change ideas, each with:
  - PLAN: hypothesis, who does what, when, where, predicted result
  - DO: data collection plan
  - STUDY: how to analyze, what success looks like
  - ACT: criteria for adopt / adapt / abandon
- Identify likely barriers (clinician workflow, patient concerns, system-level)
- One question to ask before starting: who is the project champion?

CONSTRAINTS:
- Designed for a 6-month resident QI project
- Realistic for a clinic without a dedicated QI analyst
- Patient-centered — acknowledge HPV vaccine hesitancy explicitly without being dismissive

SUCCESS CRITERIA:
- A resident could take this to clinic leadership and start the project
- The aim statement and measures are operationalizable
- Interventions are evidence-anchored, not just brainstormed
```

**Strategy note:** Evidence anchoring extends beyond clinical guidelines to QI-specific sources (IHI, Community Preventive Services Task Force). Forcing operational definitions on measures is the biggest difference between a real PDSA and "a brainstorm with PDSA labels."

---

## Cross-cutting: procedural training

Procedural training (IUDs, biopsies, joint injections, POCUS, etc.) doesn't fit neatly into one bucket — it spans Clinical Teaching (skill demos), Assessment (checklists, mastery thresholds), and Admin/QI (curriculum structure). When a faculty member asks for procedural-training help, route to the **Simulation-Based Mastery Learning (SBML)** framework in `references/pedagogical-frameworks.md` § SBML. Key elements of any procedural prompt:

- Behavioral checklist with critical failure points clearly flagged
- Reference to AAFP procedural training resources and specialty society guidance
- Angoff-method consensus framing (don't fabricate specific MPS thresholds)
- Retraining schedule for low-frequency procedures (skill degrades 2-6 months)
- Same-day paired clinic (SPPC) consideration when designing curricula

Common faculty requests in this area:
- "Build a checklist for [IUD insertion / skin biopsy / knee injection]"
- "Help me set up procedural training for [procedure]"
- "How do I assess procedural competency?"
- "Design a simulation scenario for [procedure]"

## Cross-cutting: telemedicine, POCUS, AI/ML curricula

Three areas with formal STFM curricula faculty can anchor to. When a request involves these, name the curriculum:

- **AiM-PC** — STFM curriculum for AI/ML in primary care (AI foundations, ethics, clinical implementation)
- **STFM telemedicine 5-module curriculum** — remote exams, access/equity, escalation criteria
- **STFM POCUS resources** — image acquisition, foundational physics, bedside decision-making

Don't reinvent the curriculum; build prompts that extend or supplement it.

---

## When the request doesn't fit a bucket

If the user's request straddles buckets (common — e.g., "a journal club for residents that I'll also turn into a patient handout"), pick the dominant bucket and note the secondary need in the strategy note. If you genuinely can't classify, ask one clarifying question: "Is this primarily for [bucket A] or [bucket B]? It changes how I anchor the evidence."