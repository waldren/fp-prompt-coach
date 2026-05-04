# Model Routing for Family Medicine Prompts

Different LLMs respond to different structural cues. A prompt that works beautifully for Claude can underperform on GPT-4 or Gemini, and vice versa. This reference covers the routing decisions that matter for FM educational prompts.

A general note: faculty often don't know which model is best, and asking is fair — but if they don't have a strong preference, default to the model with the best documented performance on medical tasks at the time of use, and tell them they can adapt the prompt to whichever tool their institution has approved.

---

## Model-by-model adaptation

### Claude (Sonnet, Opus)

**Strengths for FM:** Strong performance on long-context, structured medical reasoning. Reliable XML tag parsing. Tends to be appropriately hedged on clinical claims. Good at refusal clauses.

**How to structure prompts:**
- Use XML-style tags for major sections: `<role>`, `<task>`, `<evidence_rail>`, `<output_format>`
- Long, well-structured context blocks work well — Claude reads them
- Refusal clauses are reliably honored
- "Think through this step-by-step before answering" works on non-thinking variants; on extended-thinking variants, leave it out — Claude thinks internally

**Example structural shell:**
```
<role>Family Medicine educator preparing PGY-2 didactic content...</role>

<task>Generate a 20-minute teaching outline on...</task>

<evidence_rail>
Cite only USPSTF, AAFP, and ADA Standards of Care.
For each recommendation: source + grade.
Refuse to fabricate citations.
</evidence_rail>

<output_format>
1. Three learning objectives
2. ...
</output_format>

<safety>
If a question requires individualized clinical judgment, respond: "...refer to the supervising physician."
</safety>
```

---

### ChatGPT / GPT-4 / GPT-5.x

**Strengths for FM:** Broad medical knowledge base, strong at structured outputs, good at MCQ generation.

**Watch for:** Tendency toward sycophancy (validating bad reasoning), occasional confident hallucination of citations, inconsistent guideline currency.

**How to structure prompts:**
- Plain section headers (## ROLE, ## TASK) work better than XML
- Explicit "If you are uncertain, say so" rules are needed — GPT models hedge less by default than Claude
- Output contract should be very explicit (numbered structure, exact word counts) — the model sometimes ignores soft format hints
- Anti-sycophancy clauses help: "Do not validate or compliment the user's reasoning. Critique it directly."

**Example shell:**
```
## ROLE
You are a Family Medicine educator...

## TASK
[concrete, single action]

## EVIDENCE RAIL
- Cite only [list].
- For uncertain claims, write "I don't have a confirmed source."
- Do not generate DOIs or page numbers.
- State your knowledge cutoff date.

## OUTPUT FORMAT
1. ...
2. ...

## SAFETY
[refusal clause]

## ANTI-SYCOPHANCY
Do not begin the response with praise or validation. Begin directly with the requested content.
```

---

### Gemini (1.5 Pro, 2.x)

**Strengths for FM:** Long context window (useful for pasting full guidelines or articles for journal club), multimodal (can take in PDFs/images of guidelines).

**Watch for:** Variable performance on guideline currency, sometimes shorter or more compressed outputs than requested.

**How to structure prompts:**
- Plain section headers
- Explicit length specifications ("at least 600 words," "no shorter than 4 paragraphs") help counter compression tendency
- For grounding on user-provided documents, lead with: "Base your answer ONLY on the document I provide. Do not supplement with external knowledge."
- Citation rules need to be explicit and repeated

**Best uses for FM:** Journal club appraisal where the user pastes the full article; guideline summarization where the user pastes the guideline PDF.

---

### Reasoning models (o1, o3, o4-mini, DeepSeek-R1)

**Strengths for FM:** Strong on multi-step clinical reasoning, board-style question analysis, EBM appraisal.

**Watch for:** Do NOT add "think step-by-step" — these models think internally and explicit chain-of-thought instructions degrade output. Do NOT add many examples — these models do better with clean, terse instructions.

**How to structure prompts:**
- Short, clean instructions
- No "think step-by-step" or "show your reasoning"
- No few-shot examples unless format is highly unusual
- Output format spec at the end, brief

**Best uses for FM:** Differential diagnosis reasoning exercises, complex EBM appraisal, board question generation with detailed rationales.

---

### Local / on-device models (Llama, Mistral, Qwen, on-device clinic AI)

**Strengths for FM:** PHI safety — when run locally, no data leaves the device. Some institutions have on-device models specifically for clinical workflows.

**Watch for:** Smaller models compress prompts more aggressively, follow complex instructions less reliably, and may have older training data.

**How to structure prompts:**
- Shorter, simpler structure
- One task per prompt — don't chain
- More explicit format specification
- Lower expectations for citation accuracy — verify everything

**Best uses for FM:** PHI-adjacent workflows where the institutional AI policy permits local models but not cloud LLMs.

---

## Choosing a model when the user doesn't specify

Rough decision tree:

| Task | Best fit (general guidance) |
|---|---|
| Long didactic with citations | Claude or GPT-5.x |
| Patient handout at 6th grade | Any major model — verify reading level |
| Critical appraisal of pasted article | Gemini (long context) or Claude |
| Differential diagnosis reasoning | Reasoning models (o1/o3) or Claude |
| MCQ writing with rationales | GPT or Claude |
| Milestone narrative draft | Claude or GPT |
| Quick clinical fact summary | Any — but verify |
| Anything involving PHI | Institutional AI tool with BAA, or local model — never public LLMs |

---

## Institutional considerations

Most academic medical centers and residency programs have an approved AI tool list. The skill should ask, when relevant: "Does your institution have an approved AI tool for this kind of work? Some prompts are best run inside an institutional tool with a BAA rather than a public LLM."

Common institutional setups (as of recent guidance):
- **Microsoft Copilot for Microsoft 365 with EDU/enterprise tenancy:** often approved for non-PHI educational work
- **Institutional ChatGPT Edu / Enterprise:** sometimes approved with restrictions
- **Epic-integrated AI (e.g., MyChart message drafting, ambient documentation):** these are different products, governed by Epic + institutional BAAs
- **Nabla, DAX, Suki, Augmedix:** ambient-listening clinical tools with separate compliance frameworks

The skill does not need to know each institution's specific policy — it should remind faculty to check institutional policy for any clinical-data-adjacent use.

---

## When to recommend the user change tools

Sometimes the right answer is "this prompt should not be sent to the AI tool you're using." Cases:

- **PHI in the prompt** + public LLM → Recommend institutional tool or de-identification
- **Time-critical patient care decision** → Not an AI use case; refer to supervising physician
- **Manuscript ghostwriting for peer-reviewed submission** → Remind of disclosure norms; recommend editing-only assistance
- **High-stakes assessment writing (e.g., a real ITE-equivalent)** → Question banks need expert review; AI is a draft tool

In each case, explain the concern briefly and offer a safer reframing rather than refusing outright.