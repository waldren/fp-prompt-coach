# Prompt Frameworks for Family Medicine Education

These are the eight prompt architectures this skill picks from. The selection is silent — the user sees the finished prompt, not the framework name. Each framework here is adapted from general-purpose prompt engineering with FM-specific modifications.

## Selection guide (decide silently)

| Use case bucket | Default framework |
|---|---|
| Quick clinical teaching, brief explainer | RTF-Med |
| Pedagogically-explicit teaching design | BRAIN |
| Polished didactic, professional document | CO-STAR-Med |
| Anything citing guidelines or research | EBM-Anchor |
| Clinical decision support (faculty thinking through a case) | EBM-Anchor + reasoning-transparency rules |
| OSCE / case-based teaching | OSCE-Builder |
| Milestone evaluations, MCQs, OSCE rubrics | Assessment-Frame |
| Patient handouts, scripts, discharge instructions | Patient-Ed-Frame |
| QI projects, curriculum design | QI-Frame |
| Procedural training, simulation curricula | SBML-Frame (see pedagogical-frameworks.md) |
| Fixing or adapting an existing prompt | Decompiler |

**Pedagogical scaffolding overlay:** If the user's intent involves teaching, feedback, reflection, or procedural training, layer the appropriate FM teaching framework (OMP, SNAPPS, R2C2, MAL, SBML) on top of the prompt-engineering framework chosen above. See `references/pedagogical-frameworks.md` for the full set. The prompt-engineering framework is the *container*; the pedagogical framework is the *structure inside it*.

---

## 1. RTF-Med (Role, Task, Format)

**When to use:** Fast, focused, single-purpose prompts. Good for quick teaching tools, summary requests, or when the user has clear intent.

**Structure:**
```
ROLE: [FM clinician/educator role]
LEARNER (if applicable): [level + setting]
TASK: [single concrete verb-led action]
EVIDENCE RAIL: [source priority + citation discipline + uncertainty rule]
OUTPUT FORMAT: [structure + length + reading level if patient-facing]
SAFETY: [refusal clause if needed]
```

**FM modification:** The standard RTF gets two mandatory additions — Evidence Rail and Safety — even for "simple" prompts. There is no such thing as a medical education prompt without an evidence anchor.

---

## 2. BRAIN

**When to use:** Pedagogically-explicit teaching design. BRAIN is from the medical education literature and surfaces the *teaching context* — what's already established about the learner, what specific skill is being targeted — in a way RTF-Med doesn't. Use it when the request involves designing a teaching interaction, a case-based reasoning exercise, or a feedback session, and the faculty member has specific learner context to share.

**Structure:**
```
BACKGROUND: [What's already established about the learner — current rotation, recent observed performance, specific gap or strength, prior teaching that's happened]
ROLE: [The faculty/AI role — Senior Medical Educator, Family Medicine Preceptor, Clinical Competency Committee member, etc.]
AIM: [The specific learning goal in behavioral terms — what the learner will be able to do after this]
INSTRUCTIONS: [Concrete instruction set for what the AI should produce — the case, the questions, the rubric]
NEXT STEPS: [What happens after — follow-up resources, check-in plan, the learner's homework, the reference table they'll get]
EVIDENCE RAIL: [Always included]
SAFETY: [Always included for clinical content]
```

**Worked shape:**
```
BACKGROUND: A third-year resident is struggling with the "Analyze" step of SNAPPS — they tend to commit to a single diagnosis without comparing alternatives.

ROLE: Senior Family Medicine Educator.

AIM: Improve the resident's ability to compare and contrast a differential diagnosis using explicit decision criteria.

INSTRUCTIONS: Given a case of a 65-year-old with chronic heart failure and new-onset shortness of breath, generate three probing questions that force the resident to justify why they prioritized pulmonary edema over pneumonia. Each question should require the resident to name a specific finding that distinguishes the two.

NEXT STEPS: Provide a structured comparison table for these two conditions based on current ACC/AHA guidelines that the resident can use as a reference for future cases.

EVIDENCE RAIL: Cite ACC/AHA Heart Failure Guideline and AAFP. State your knowledge cutoff and remind me to verify against current editions. No fabricated citations.
```

**FM modification:** BRAIN was developed for medical education specifically, so it already fits — the FM additions are the standard Evidence Rail, and the requirement that "Aim" be stated in behavioral, milestone-mappable terms.

---

## 3. CO-STAR-Med (Context, Objective, Style, Tone, Audience, Response)

**When to use:** Professional documents, scholarly drafts, formal communications, lectures intended for a wider audience.

**Structure:**
```
CONTEXT: [Setting, learner level, what's already established, why this is needed]
OBJECTIVE: [The educator's actual goal — what changes after they use this output]
STYLE: [Voice — academic, conversational, plain-language patient-facing, etc.]
TONE: [Formal/informal, hedged/confident, motivational/factual]
AUDIENCE: [Who reads this — separate from learner level when they differ]
RESPONSE: [Format, length, structure]
EVIDENCE RAIL: [Always included]
SAFETY: [Always included for clinical content]
```

**FM modification:** Audience and Learner are separated. Many FM prompts are written *by* faculty *for* a resident *to give to* a patient — three different audiences. CO-STAR-Med makes that chain explicit.

---

## 4. EBM-Anchor

**When to use:** Any prompt where the output will be cited to learners or affect clinical reasoning. This is the most-used framework in this skill, including for clinical decision support requests where a faculty member is using AI to think through a case.

**Structure:**
```
ROLE: [FM educator with relevant clinical expertise]
TASK: [What to produce]
EVIDENCE TIER: [Highest tier acceptable — see safety-triage.md]
SOURCE LIST (allow): [Specific named sources only]
SOURCE LIST (forbid): [E.g., "Do not cite Wikipedia, Medscape, or general web content"]
CITATION DISCIPLINE:
  - Every claim → source + grade (USPSTF letter / SORT / GRADE)
  - Distinguish guideline statement from clinical interpretation
  - "I don't have a confirmed source for this" if uncertain
  - No fabricated citation numbers, DOIs, or page numbers
CURRENCY RULE: [State knowledge cutoff; flag time-sensitive topics]
OUTPUT FORMAT: [Standard format spec]
SAFETY: [Refusal clause + scope-of-practice]
```

**For clinical decision support — add these:**
- **Reasoning transparency:** "Show your reasoning for each recommendation. Don't just state a conclusion — name which guideline supports it and what aspect of the patient's phenotype invokes it."
- **What-would-change-your-answer clause:** "List 3-5 patient or contextual facts that would clearly tip the decision. This is how I'll know what to ask the patient."
- **Dosing offload:** "Do not recommend specific dosing — name the drug class and refer to standard dosing references."

**FM modification:** This is the core innovation of this skill. EBM-Anchor treats evidence rules as a separate, structurally distinct prompt section — not a sentence buried in the task. This is the section that prevents the most clinical errors. The reasoning-transparency additions are what convert it from a teaching framework into a decision-support framework.

---

## 5. OSCE-Builder

**When to use:** Writing standardized patient cases, OSCE stations, simulation scenarios, case-based small-group teaching.

**Structure:**
```
ROLE: You are an FM educator writing an OSCE case.
LEARNER: [MS3, MS4, intern, etc. + rotation]
COMPETENCY: [What this case is testing — e.g., communication, differential, procedure]
CASE PARAMETERS:
  - Setting (FM clinic, urgent care, hospital, home visit)
  - Time limit for the encounter
  - Standardized patient demographics (de-identified, fully fictional)
  - Chief complaint
  - Key teaching point
EVIDENCE RAIL: [Standard]
OUTPUT FORMAT:
  1. Case summary (for the writer)
  2. Standardized patient script (door note, opening line, history responses, affect notes)
  3. Examiner instructions
  4. Scoring rubric (specific behaviors, weighted)
  5. Pitfalls and red herrings (so the case is challenging without being unfair)
  6. Debrief points
SYNTHETIC CASE FLAG: "This case is fully synthetic and not based on any real patient. Do not draw on real cases or PHI."
```

**FM modification:** The synthetic-case flag is mandatory. Without it, models occasionally pull demographic details that pattern-match to real published cases. The rubric is required to be behavioral, not impressionistic.

---

## 6. Assessment-Frame

**When to use:** Milestone evaluations, narrative comments, MCQ writing, formative feedback drafts.

**Structure:**
```
ROLE: [FM educator role]
TASK: [Specific assessment artifact]
LEARNER CONTEXT:
  - Level (intern/PGY-2/PGY-3)
  - Specific behaviors I observed (you must wait for me to provide these)
  - Frequency, trajectory
  - ACGME FM Milestones 2.0 subcompetency + level (e.g., PC2 Level 3, ICS1 Level 2). See pedagogical-frameworks.md for the subcompetency reference table.
EVIDENCE RAIL:
  - Use ACGME FM Milestones 2.0 language for evaluations — anchor to specific subcompetencies, not generic "milestones"
  - Use exam-specific style for MCQs (ABFM/ITE/USMLE/COMLEX)
PEDAGOGICAL OVERLAY (for feedback prompts):
  - Use R2C2 structure (Relationship, Reactions, Content, Coaching) — see pedagogical-frameworks.md
  - Specify R2C2 full vs. R2C2 in-the-moment based on the time available
OUTPUT FORMAT: [Standard for the artifact type]
ANTI-BIAS RULES:
  - Behavior only — no personality language
  - No gendered, racialized, cultural, or appearance-based descriptors
  - SBI model (Situation-Behavior-Impact) for narrative
  - Avoid documented bias-prone descriptors: "articulate," "aggressive," "soft-spoken," "mature," "pleasant," "difficult," "hard-working"
NO FABRICATION RULE:
  - Do not invent observations I haven't told you about
  - If I haven't given you specifics, ask before drafting
SUCCESS CRITERIA: [Specific, behavioral, milestone-mapped]
```

**FM modification:** The anti-bias rules and no-fabrication rule are structural, not aspirational. LLMs will pad evaluations with plausible-sounding but invented behaviors if not constrained. The R2C2 overlay is what makes feedback prompts genuinely useful — without it, the output is a static comment rather than a structured conversation faculty can run.

---

## 7. Patient-Ed-Frame

**When to use:** Any patient-facing content — handouts, after-visit summaries, scripts, discharge instructions.

**Structure:**
```
ROLE: [FM clinician writing patient education]
TASK: [Specific deliverable]
PATIENT CONTEXT:
  - Population (general adult, pediatric, geriatric, prenatal, etc.)
  - Health literacy assumption
  - Language(s)
  - Use case (in-clinic, take-home, before-visit, post-visit)
EVIDENCE RAIL: [Patient-appropriate sources — AAFP, AHRQ, NIH plain-language, USPSTF]
OUTPUT FORMAT:
  - Reading level: [specify Flesch-Kincaid grade — typically 6th for general, 4th for discharge]
  - Length cap
  - Section structure (always include red-flag/when-to-call section)
LANGUAGE RULES:
  - "You" and "your" throughout
  - Replace every clinical term with plain language
  - No jargon: "high blood pressure" not "hypertension"; "feeling tired" not "fatigue"
  - Concrete actions, not "as directed"
CULTURAL HUMILITY RULES:
  - No assumptions about diet, family structure, religion, transportation
  - Gender-neutral defaults unless clinically relevant
DOSING RULE: Do not include specific dose numbers. Say "the dose your doctor prescribed."
SAFETY: Include footer: "This is general information. Always follow what your doctor and pharmacist tell you specifically."
```

**FM modification:** The dosing rule prevents one of the most dangerous LLM failure modes in patient material — transposing a digit or pulling an outdated dose. Specific dosing comes from the prescribing clinician, not the handout.

---

## 8. QI-Frame

**When to use:** PDSA cycles, root cause analysis, quality improvement project planning, AAR (after-action review).

**Structure:**
```
ROLE: [FM QI educator]
TASK: [Specific QI artifact — PDSA worksheet, fishbone, driver diagram, AAR]
PROJECT CONTEXT:
  - Clinical/operational problem
  - Setting (small clinic, residency continuity clinic, FQHC, etc.)
  - Resources (with or without QI analyst, EHR data access)
  - Timeline
  - Stakeholders
EVIDENCE RAIL:
  - Methodology: IHI Model for Improvement (or named alternative — Lean, Six Sigma)
  - Clinical interventions anchored to AAFP, CDC, USPSTF, Community Preventive Services Task Force
  - Evidence strength noted for each intervention (strong / moderate / limited / unknown)
OUTPUT FORMAT: [Specific to the artifact]
SMART AIM: Always require a SMART aim statement with placeholders for clinic-specific numbers
MEASURES RULE: Always include outcome + process + balancing measures with operational definitions
REALISM CHECK: Must be doable by a resident-led team in the stated timeline
```

**FM modification:** Forces operational definitions and the balancing measure (often skipped). The realism check prevents AI from producing PDSA cycles that require resources the user doesn't have.

---

## 9. Decompiler

**When to use:** The user has an existing prompt that's not working, or wants to adapt a prompt from one tool to another, or wants to understand why a prompt is failing.

**Structure:**
```
1. Diagnose the input prompt:
   - What's the actual task being asked?
   - What dimensions of intent are missing? (run the 10-dimension check)
   - What FM-specific safety rails are absent?
   - What pitfalls (see pitfalls.md) is it triggering?
2. Rebuild the prompt using the appropriate framework above
3. Show before/after with a 1-2 sentence diagnosis of each change
```

**FM modification:** The diagnosis must explicitly cite which FM safety rail or pitfall triggered each change, not just "I made it more specific." Faculty learn prompt engineering by seeing the reasoning, not just the better output.

---

## A note on framework mixing

Real prompts often blend frameworks — a journal club prompt is EBM-Anchor + RTF-Med, a patient-handout prompt for a teaching session is Patient-Ed-Frame + Assessment-Frame. Pick the dominant framework, then borrow elements from secondary frameworks as needed. The structural conventions in the main SKILL.md (role, evidence rail, scope, output contract, refusal clause) apply across all frameworks.