# FM Pedagogical Frameworks

These are the teaching, feedback, and procedural-training frameworks Family Medicine faculty actually use day-to-day. Generic prompt engineering treats "explain to a learner" as a single act. FM faculty distinguish between teaching during a busy clinic (OMP), facilitating learner-driven reasoning (SNAPPS), giving formative feedback (R2C2), and training procedures (SBML) — each has its own structure and each maps to a different prompt shape.

When a faculty member's request maps to one of these frameworks, scaffold the AI prompt around it — that's how they teach, so that's how the AI output should land.

## Quick selection

| Faculty intent | Framework | When to use |
|---|---|---|
| Teach during a 1-5 minute clinic moment | One-Minute Preceptor (OMP) | Brief teachable moments, busy clinic |
| Build the resident's clinical reasoning | SNAPPS | When you have 10-15 minutes and want learner-led analysis |
| Ensure the "evidence" probe doesn't get skipped | DEFT (OMP variant) | When training new preceptors |
| Give formative feedback after an encounter | R2C2 (full) or R2C2 in-the-moment | After observed encounters, semi-annual reviews |
| Foster self-regulated learning and reflection | Master Adaptive Learner (MAL) | Reflective prompts, learning plans |
| Train and assess procedures | Simulation-Based Mastery Learning (SBML) | Procedural curricula, checklists |

---

## 1. One-Minute Preceptor (OMP)

The dominant framework for teaching in busy outpatient clinics. Five microskills, designed for 1-5 minute interactions. Preceptor-led.

The five steps:
1. **Get a commitment** — "What do you think is going on with this patient?"
2. **Probe for supporting evidence** — "What led you to that conclusion?"
3. **Teach a general rule** — One generalizable teaching point, not a lecture
4. **Reinforce what was right** — Specific, behavioral praise
5. **Correct mistakes** — Specific, behavioral correction with the right answer

**Common faculty failure mode:** skipping step 2 (probing) and going straight to step 5 (correcting). The DEFT variant (Diagnosis, Evidence, Feedback, Teaching) was created specifically to force the evidence probe.

### Prompt shell when faculty wants OMP-aligned output

```
ROLE: You are a Family Medicine preceptor using the One-Minute Preceptor model in a busy outpatient clinic.

LEARNER: [Level, rotation]

TASK: [What the faculty wants to produce — script, teaching outline, simulated encounter, etc.]

OMP STRUCTURE: Organize the output around the five microskills:
1. Get a commitment — provide an opening question that elicits the learner's clinical impression
2. Probe for evidence — provide 2-3 follow-up questions that ask "what led you to that?"
3. Teach a general rule — one generalizable teaching point, ≤2 sentences, not a mini-lecture
4. Reinforce what was right — specific behavioral praise (not "good job")
5. Correct mistakes — specific behavioral correction with the corrected reasoning

EVIDENCE RAIL: [Standard]

CONSTRAINT: The full interaction should fit in 3-5 minutes of clinic time. No long explanations. No lecturing.
```

### When to use this in a prompt

When faculty say:
- "Help me teach this case to my resident in clinic"
- "Give me prompts I can use during precepting"
- "Practice OMP with me on [topic]" (where the AI plays the resident)
- "Help me train a new preceptor on OMP"

---

## 2. SNAPPS

Learner-led six-step framework. Used when there's time (typically 10-15 minutes) and the goal is to develop the resident's diagnostic reasoning rather than transmit content efficiently. Evidence shows SNAPPS produces more justified differentials and more learner-initiated study questions than traditional case presentations.

The six steps (learner does these; preceptor facilitates):
1. **Summarize** the history and findings briefly
2. **Narrow** the differential to 2-3 likely possibilities
3. **Analyze** the differential by comparing and contrasting
4. **Probe** the preceptor with questions about uncertainties
5. **Plan** management for the patient
6. **Select** a case-related issue for self-directed study

**Common faculty failure mode:** taking over at step 3 or 4. The whole point is that the learner drives the analysis.

### Prompt shell when faculty wants SNAPPS-aligned output

```
ROLE: You are a Family Medicine preceptor facilitating a SNAPPS case presentation. You are the consultant the learner probes — you do not lead.

LEARNER: [Level, rotation]

TASK: [What faculty wants — a SNAPPS worksheet, a practice case, a feedback rubric, etc.]

SNAPPS STRUCTURE: Scaffold the output around the six learner-led steps:
1. Summarize — concise, ≤5 sentences
2. Narrow — 2-3 differential possibilities, no more
3. Analyze — explicit compare/contrast across the narrowed differential
4. Probe — at least 2 questions the learner asks the preceptor
5. Plan — initial management plan with rationale
6. Select — one case-related question for self-directed study

EVIDENCE RAIL: [Standard]

CONSTRAINT: The framework is learner-led. If producing a worksheet or scenario, structure it so the learner generates content for steps 1-6 — do not pre-fill the analysis.
```

### When to use this in a prompt

When faculty say:
- "Help me create a SNAPPS worksheet for my resident"
- "Set up a case for the resident to present using SNAPPS"
- "Help my resident develop their differential reasoning"
- "I want the resident to do the thinking, not me"

---

## 3. R2C2 Feedback Model

The evidence-based standard for formative feedback in residency. Four phases, designed to make feedback bidirectional and reflective rather than top-down.

The four phases:
1. **Relationship** — establish rapport, set the stage, agree on what's being observed
2. **Reactions** — explore the learner's emotional response and self-assessment ("How do you feel that went?")
3. **Content** — confirm shared understanding of the performance data
4. **Coaching** — co-create a Learning Change Plan with specific, actionable next steps

**Two flavors:**
- **Full R2C2** — semi-annual review or scheduled feedback session, 30+ minutes
- **R2C2 in-the-moment** — 5-8 minutes immediately after an encounter; same four phases compressed

**Common faculty failure mode:** Skipping the Reactions phase because it's uncomfortable, or skipping Coaching because time runs out. Disconfirming feedback (where the learner's self-assessment is wrong) is particularly hard and is where R2C2 most adds value.

### Prompt shell when faculty wants R2C2-aligned output

```
ROLE: You are a Family Medicine educator helping me prepare an R2C2-aligned feedback conversation.

LEARNER CONTEXT: [Level, rotation, the specific behaviors I observed — wait for me to provide these]

TASK: Generate prompts/phrases I can use across the four R2C2 phases for this specific feedback scenario.

R2C2 STRUCTURE:
1. Relationship — 1-2 opening phrases that set the stage and agree on focus
2. Reactions — 2-3 questions that invite the learner's self-assessment and emotional response. Include language for handling disconfirming feedback (when their self-assessment differs from mine).
3. Content — 2 phrases for confirming shared understanding without lecturing
4. Coaching — a template for a Learning Change Plan: 1-2 specific goals, measurable outcomes, named resources, a check-in date

EVIDENCE RAIL: Use ACGME FM Milestones 2.0 language for behavior descriptions. Map to specific subcompetencies if I provide them.

ANTI-BIAS RULES:
- Behavior only — no personality language
- No gendered, racialized, cultural, or appearance-based descriptors
- Avoid documented bias-prone descriptors: "articulate," "aggressive," "soft-spoken," "mature," "pleasant," "difficult"

NO FABRICATION: Do not invent observations I haven't told you about. If I haven't given you specifics, ask.

FORMAT VARIANT: Specify whether this is for full R2C2 (30+ min, scheduled) or in-the-moment (5-8 min, post-encounter).
```

### When to use this in a prompt

When faculty say:
- "I need to give feedback to a resident about [behavior/event]"
- "Help me write a Learning Change Plan"
- "How do I tell a resident their self-assessment is off?"
- Any narrative evaluation prompt — the underlying structure should still be R2C2-informed

---

## 4. Master Adaptive Learner (MAL)

A framework for fostering self-regulated, lifelong learning — particularly relevant for reflective prompts, learning plans, and any output that asks the resident to identify their own gaps. MAL treats clinical uncertainty as a *trigger* for learning rather than a problem.

The four MAL phases:
1. **Planning** — identify a learning gap from a specific encounter
2. **Learning** — engage with new knowledge or skills
3. **Assessing** — try the new knowledge in practice
4. **Adjusting** — incorporate or refine based on outcomes

### Prompt shell when faculty wants MAL-aligned output

```
ROLE: You are a Family Medicine educator helping a resident develop Master Adaptive Learner habits.

TASK: [Reflective worksheet, learning plan template, post-encounter prompt, etc.]

MAL STRUCTURE: Scaffold the output around the four phases. Treat clinical uncertainty as the trigger:
1. Planning — what specific uncertainty or gap did this encounter surface?
2. Learning — what evidence-based resources will the resident use to address it? (Force named sources — UpToDate, AAFP, USPSTF, specific journal — not "the literature")
3. Assessing — how will the resident test their new understanding next time?
4. Adjusting — what's the check-in plan?

EVIDENCE RAIL: [Standard]

CONSTRAINT: The output is the *resident's* artifact, not the faculty member's. Phrase prompts in second person ("identify a clinical uncertainty you faced...").
```

### When to use this in a prompt

When faculty say:
- "Give me a reflection prompt for residents after clinic"
- "Help me build a self-directed learning plan template"
- "I want residents to track their uncertainties"

---

## 5. Simulation-Based Mastery Learning (SBML)

The standard for procedural training. Learners must meet a high passing standard (Minimum Passing Standard, MPS) on a checklist through deliberate practice — *before* doing the procedure on a patient. The "see one, do one, teach one" model is explicitly rejected as unsafe.

Core elements:
- Behavioral checklist developed via literature review + expert consensus, often using the **Angoff method** to set the MPS
- Deliberate practice with feedback until MPS is met
- Critical failure points identified (sterility breaks, anatomical landmark errors, etc.) — failing one of these means failing the whole assessment regardless of total score
- Skill retention plan — most procedures degrade by 2-6 months without retraining
- Same-Day Paired Clinic (SPPC) model when possible: morning simulation, afternoon supervised clinical performance

### Prompt shell when faculty wants SBML-aligned output

```
ROLE: You are a Family Medicine procedural educator developing a Simulation-Based Mastery Learning curriculum component.

PROCEDURE: [Specific procedure — IUD insertion, knee injection, skin punch biopsy, etc.]

LEARNER: [Level, prior procedural experience if known]

TASK: [Checklist development, deliberate practice scenario, retention plan, etc.]

EVIDENCE RAIL:
- Anchor the checklist to AAFP procedural training resources, specialty society procedural guidelines, and current peer-reviewed procedural literature.
- For each checklist item, indicate whether it's a "critical failure point" (failing this item means failing the whole assessment) vs. standard performance.
- Do not fabricate specific statistical thresholds — note where Angoff-method consensus would be needed.

OUTPUT FORMAT (for checklist requests):
1. Pre-procedure: indications, contraindications, consent elements
2. Setup: equipment, sterility, positioning
3. Procedure steps in sequence, broken into micro-skills
4. Post-procedure: immediate care, documentation, complication monitoring
5. Critical failure points clearly flagged
6. Retraining schedule recommendation (low-frequency procedures: every 2-6 months)

CONSTRAINT: Each checklist item must be observable and dichotomous (done correctly / not done correctly). No items that require subjective judgment without anchoring criteria.
```

### When to use this in a prompt

When faculty say:
- "Build a checklist for [procedure]"
- "Help me set up procedural training for IUD/biopsy/injection/etc."
- "Develop a simulation scenario for [procedure]"
- "How do I assess procedural competency?"

---

## ACGME Milestones 2.0 — quick reference

When a faculty prompt targets a specific competency, anchor the output to the relevant subcompetency rather than to "milestones" generically. The six FM Milestones 2.0 domains contain these high-frequency subcompetencies:

| Subcompetency | Domain | Level 1 (novice) | Level 4 (graduation-ready) |
|---|---|---|---|
| **PC1** Acutely Ill Patient | Patient Care | Generates basic differential; recognizes protocols | Independently manages complex acute conditions; mobilizes teams |
| **PC2** Chronic Illness | Patient Care | Recognizes common conditions; basic plans | Leads multidisciplinary initiatives; population innovations |
| **PC3** Health Promotion | Patient Care | Identifies screening guidelines | Reconciles competing guidelines; population-based plans |
| **PC4** Undifferentiated Signs | Patient Care | Generates broad differential | Manages complex undifferentiated concerns w/ psychosocial integration |
| **PC5** Procedural Care | Patient Care | Identifies indications/risks | Demonstrates technical proficiency, independent performance |
| **MK1** Knowledge Breadth | Medical Knowledge | Basic biomedical knowledge | Depth/breadth for independent practice |
| **SBP1** Patient Safety / QI | Systems-Based Practice | Describes basic safety principles | Leads QI projects; identifies system failures |
| **PROF1** Ethical Principles | Professionalism | Recognizes ethical dilemmas | Consistent ethical behavior in complex scenarios |

Other domains include Practice-Based Learning and Improvement (PBLI) and Interpersonal and Communication Skills (ICS) with their own subcompetencies.

When a faculty prompt could benefit from milestone anchoring, include language like:

> Target ACGME FM Milestones 2.0 subcompetency [PC2 Level 3]: "Manages chronic illness in patients with multiple comorbidities." Output should reflect this developmental level — not novice, not graduation-ready.

This is more useful than "PGY-2 level" alone, because Milestones 2.0 are explicitly developmental rather than time-based. A PGY-2 might be Level 2 in PC1 and Level 3 in PC2; the prompt should target the *milestone level*, not the year of training.

### When to use Milestones 2.0 anchoring

- Any milestone evaluation prompt (always — this is the standard language)
- Case scenarios designed to target a specific competency level
- Reflective prompts asking residents to self-assess against a milestone
- CCC (Clinical Competency Committee) prep
- Curriculum mapping to ensure all subcompetencies are covered

### Osteopathic Recognition note

Programs with ACGME Osteopathic Recognition must additionally report on Osteopathic Principles and Practice (OPP) subcompetencies. If a faculty member is working in an OR-designated program, surface this — milestone evaluations and curriculum prompts should include OPP language.

---

## Productivity-teaching tension

A consistent constraint across all FM teaching frameworks: faculty have minutes, not hours. Any prompt that would take more than 5 minutes to use during a clinic session needs to either:

- Be reframed as a between-clinic or end-of-day activity
- Be compressed into an in-the-moment variant (e.g., R2C2 in-the-moment instead of full R2C2)
- Be scaffolded so part can be done asynchronously (resident pre-fills, faculty reacts)

When drafting a teaching prompt, ask in your head: *Could a busy preceptor use this output in 3 minutes between patients?* If not, build in compression or async-friendly structure.

---

## Telemedicine, POCUS, and AI/ML curricula

Three areas where STFM has formal curricula faculty can anchor to. Mention these by name when relevant.

| Topic | Curriculum reference |
|---|---|
| AI/ML in primary care | AiM-PC curriculum (STFM) — covers AI foundations, ethical implications, clinical implementation |
| Telemedicine | STFM 5-module telemedicine curriculum — remote exams, access/equity, escalation criteria |
| Point-of-care ultrasound | STFM POCUS resources — image acquisition, foundational physics, bedside decision-making |

When a faculty member asks for prompts in these areas, anchor to the relevant curriculum and structure the output as modules or sessions that fit within the existing curriculum framework. Don't reinvent — extend.