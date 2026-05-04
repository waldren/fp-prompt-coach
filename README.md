# FM Prompt Coach
**This is an experimental skill and should be used with caution**
**No Warranties Given**

A Claude skill for Family Medicine residency faculty and medical school FM departments. Builds high-quality, safe, evidence-grounded AI prompts for clinical teaching, clinical decision support, board prep, patient education, resident assessment, EBM, QI, procedural training, and program administration.

The user is the faculty member. The skill speaks peer-to-peer — direct, brief, no over-explanation.

## What it does

Transforms vague educator requests into precise prompts that:
- Lock outputs to named evidence sources (USPSTF, AAFP, specialty guidelines, ACGME Milestones 2.0, IHI for QI, AAFP procedural resources for SBML)
- Refuse to fabricate citations, DOIs, or page numbers
- Disclose knowledge cutoff and flag time-sensitive topics
- Support — never replace — clinical judgment when used for decision support
- Specify reading level and plain-language rules for patient material
- Apply anti-bias structural rules to milestone evaluations
- Surface PHI when present and walk through de-identification (educational, not gatekeeping)
- Adapt to Claude, ChatGPT, Gemini, reasoning models, and local/institutional tools
- **Scaffold prompts around the pedagogical frameworks FM faculty actually use** — One-Minute Preceptor for busy clinic teaching, SNAPPS for learner-led reasoning, R2C2 for formative feedback, Master Adaptive Learner for reflection, Simulation-Based Mastery Learning for procedures
- **Anchor learner-level to specific ACGME FM Milestones 2.0 subcompetencies** (PC1-5, MK1, SBP1, PROF1, PBLI, ICS) and developmental levels — not just year of training

## How it works

The skill recognizes when a Family Medicine faculty member is preparing to write a prompt for any AI tool, then runs:

1. Use-case classification (Clinical Teaching, Clinical Decision Support, Patient-Facing, Assessment, EBM/Research, or Admin/QI)
2. Safety triage (PHI screen with de-identification walkthrough, scope-of-practice screen, evidence-grounding screen, currency check, academic integrity check)
3. Pedagogical framework identification (OMP, SNAPPS, DEFT, R2C2, MAL, or SBML — when applicable)
4. Intent extraction across 11 dimensions (including ACGME Milestones 2.0 subcompetency/level and time-to-use)
5. Up to 3 clarifying questions, only if genuinely needed
6. Silent prompt-engineering framework selection (RTF-Med, BRAIN, CO-STAR-Med, EBM-Anchor, OSCE-Builder, Assessment-Frame, Patient-Ed-Frame, QI-Frame, Decompiler) with pedagogical scaffold layered in
7. Drafting using FM structural conventions (role, evidence rail, scope, output contract, refusal clause)
8. Audit against 15 FM-specific pitfalls (12 clinical + 3 pedagogical)
9. Productivity-teaching test (does this fit in 3-5 minutes if for clinic use?)
10. Delivery: one clean prompt + a 2–4 line strategy note

## Installation

### In Claude.ai

1. Download the `.skill` file
2. Go to claude.ai → Sidebar → Customize → Skills → Upload a Skill
3. Upload it

### Triggers

The skill activates when you mention things like:
- "Help me write a prompt for..."
- "I want to use ChatGPT/Claude/Gemini for..."
- "Make me a [didactic / handout / case / question / evaluation]..."
- "Help me think through this case for clinic"
- "Help me build a teaching session on..."
- "Help me give feedback on..."
- "Build me a procedural checklist for..."
- "Make a SNAPPS / OMP / R2C2 / case for..."

You can also ask it directly: "Use the FM Prompt Coach skill to help me build..."

## What's inside

- `SKILL.md` — main skill definition and workflow
- `references/safety-triage.md` — PHI screen with de-identification walkthrough, scope-of-practice screen, evidence tiers, currency rules
- `references/use-cases.md` — six FM use-case buckets with worked examples + cross-cutting notes on procedural training and STFM curricula
- `references/pedagogical-frameworks.md` — FM teaching frameworks (One-Minute Preceptor, SNAPPS, DEFT, R2C2, MAL, SBML), ACGME FM Milestones 2.0 subcompetency reference table, and the productivity-teaching tension
- `references/frameworks.md` — nine prompt-engineering frameworks adapted for medical education (RTF-Med, BRAIN, CO-STAR-Med, EBM-Anchor, OSCE-Builder, Assessment-Frame, Patient-Ed-Frame, QI-Frame, Decompiler)
- `references/pitfalls.md` — 15 FM-specific failure modes with before/after examples (12 clinical + 3 pedagogical)
- `references/model-routing.md` — adapting prompts for Claude, GPT, Gemini, reasoning models, and local tools

## What it won't do

- Generate prompts that ask AI to make individualized clinical decisions in place of clinician judgment (decision *support* prompts are fine — decision *replacement* prompts are not)
- Help you put PHI into a public LLM prompt; it walks you through de-identification first, then proceeds
- Help draft AI ghostwriting for peer-reviewed submission without flagging journal disclosure norms
- Bypass model safety features
- Produce lecture scripts or monologues for "teaching" prompts — it pushes back toward facilitation tools, Socratic questions, and active-learning structures

## Caveats

- This skill helps build *prompts*. It does not validate the AI output you get back. Always verify clinical claims against current guidelines before teaching or clinical use.
- LLM training data has a knowledge cutoff. Any guideline claim from any AI model needs verification.
- Institutional AI policies vary. Check your residency program or medical school's policy before using public LLMs for any clinical-data-adjacent work, and use your institution's BAA-covered tool when handling PHI.

## Acknowledgments

Inspired by the structural approach in [nidhinjs/prompt-master](https://github.com/nidhinjs/prompt-master), reworked end-to-end for the safety, evidence-grounding, and pedagogical needs of Family Medicine residency education. Pedagogical framework integration draws on the published medical education literature on One-Minute Preceptor, SNAPPS, R2C2 feedback, Master Adaptive Learner, and Simulation-Based Mastery Learning, anchored to ACGME Family Medicine Milestones 2.0.

## License

MIT
