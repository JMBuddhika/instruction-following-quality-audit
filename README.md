# Instruction-Following Quality Audit

A structured annotation project evaluating how well AI language model responses comply with explicit constraints embedded in user prompts. This dataset covers 50 prompts across 5 constraint categories, each scored using a defined rubric with per-constraint compliance tracking.

---

## Project Overview

When users give AI models explicit instructions — "use exactly 3 bullet points", "do not use the word health", "stay in character as a medieval blacksmith" — how often does the model actually comply?

This project answers that question by constructing a dataset of 50 prompts with measurable constraints, collecting AI-generated responses, and annotating each response against a defined scoring rubric. The result is a replicable compliance dataset with per-constraint scores, failure pattern labels, and category-level compliance statistics.

This project was designed to mirror real annotation workflows used at AI companies for model evaluation, RLHF data collection, and instruction-tuning quality control.

---

## Repository Structure

```
instruction-following-audit/
│
├── README.md                          ← this file
├── annotation_guideline.md            ← full scoring rules, worked examples, IRR guidance
├── instruction_following_audit.xlsx   ← main annotation spreadsheet (4 sheets)
│   ├── Annotation Dataset             ← 50 rows, one per prompt, with scores
│   ├── Scoring Key                    ← label definitions and color reference
│   ├── Failure Pattern Tracker        ← tally of 7 failure pattern types
│   └── Summary Dashboard              ← compliance averages by category
└── prompts/
    └── 50_prompts.md                  ← all 50 prompts with constraints listed
```

---

## Dataset at a Glance

| Property | Value |
|---|---|
| Total prompts | 50 |
| Constraint categories | 5 |
| Prompts per category | 10 |
| Constraints per prompt | 2 – 4 |
| Total constraints scored | 152 |
| Scoring scale | 0 (Fail) / 1 (Partial) / 2 (Pass) |
| AI model evaluated | GPT o3 (responses collected April 2026) |
| Overall compliance rate | **99.4%** |

---

## Constraint Categories

| Category | Prompts | What is tested |
|---|---|---|
| **Format** | P01 – P10 | Bullet points, tables, numbered lists, headings, timelines |
| **Length** | P11 – P20 | Word counts, sentence counts, paragraph counts, character limits |
| **Tone & Audience** | P21 – P30 | Register adaptation, jargon avoidance, audience targeting |
| **Role & Persona** | P31 – P40 | Character maintenance, historical framing, in-role language |
| **Content Exclusion** | P41 – P50 | Banned words, forbidden topics, prohibited examples |

---

## Methodology

### Step 1 — Prompt design
Fifty prompts were written with explicit, measurable constraints. Each constraint was chosen so that compliance could be evaluated objectively — either the constraint is met or it is not. Prompts were distributed across 5 categories (10 per category) to ensure coverage of different instruction types.

### Step 2 — Response collection
Each prompt was submitted to GPT-4o with no additional system instructions. Responses were copied verbatim into the annotation spreadsheet. No editing or truncation was applied to any response.

### Step 3 — Constraint extraction
For each prompt, individual constraints were extracted and listed separately in the spreadsheet. Each constraint was labeled and scored independently from the others in the same prompt.

### Step 4 — Scoring
Each constraint was scored using a 3-point scale:

- **2 — Pass:** Constraint fully and precisely met
- **1 — Partial:** Constraint partially met, or met with a minor deviation
- **0 — Fail:** Constraint clearly ignored or violated

Compliance percentage per response was calculated as:

```
Compliance % = (Points Earned ÷ Max Points) × 100
```

Where Max Points = number of constraints × 2.

All scoring decisions follow the explicit rules in `annotation_guideline.md`. Edge cases and judgment calls are documented in the Annotator Notes column of the spreadsheet.

### Step 5 — Failure pattern labeling
Each response was tagged with failure pattern codes where applicable, using the 7-pattern taxonomy defined below.

### Step 6 — Summary statistics
Category-level and overall compliance averages were calculated and recorded in the Summary Dashboard sheet.

---

## Scoring Rubric Summary

| Score | Label | Definition |
|---|---|---|
| 2 | Pass | Constraint fully met with no meaningful deviation |
| 1 | Partial | Constraint partially met or met with a minor deviation |
| 0 | Fail | Constraint ignored, violated, or directly contradicted |

Full scoring rules per category, with worked examples and edge case decisions, are documented in `annotation_guideline.md`.

---

## Failure Pattern Taxonomy

Seven recurring failure patterns were tracked across all 50 responses:

| Code | Pattern | Description |
|---|---|---|
| OA | Over-answering | Response gave more content than the prompt requested |
| FD | Format drift | Started in the correct format, switched mid-response |
| PD | Persona drop | Lost the assigned role or character partway through |
| CE | Count error | Wrong number of bullets, sentences, paragraphs, or items |
| BW | Banned word slip | Used a word or phrase explicitly prohibited by the prompt |
| TD | Tone drift | Correct register at start, reverted to default AI tone |
| EF | Exclusion failure | Included content, topics, or examples the prompt prohibited |

---

## Key Findings

### Overall compliance
GPT-4o achieved an overall compliance rate of **99.4%** across all 50 prompts and 152 total scored constraints.

### Compliance by category

| Category | Avg Compliance % | Observation |
|---|---|---|
| Format | 99.7% | Near-perfect. Only partial score was on bold title formatting (P02), which is ambiguous in plain text output |
| Length | 99.7% | Near-perfect. Only partial score was on word count verification (P16), which was within range but close to the boundary |
| Tone & Audience | 100.0% | Perfect compliance. Register adaptation, jargon avoidance, and audience targeting all met precisely |
| Role & Persona | 100.0% | Perfect compliance. Character voice maintained without breaks across all 10 persona prompts |
| Content Exclusion | 100.0% | Perfect compliance. All banned words, forbidden topics, and prohibited examples successfully avoided |

### Failure patterns detected

| Code | Pattern | Occurrences | % of 50 prompts |
|---|---|---|---|
| OA | Over-answering | 0 | 0% |
| FD | Format drift | 0 | 0% |
| PD | Persona drop | 0 | 0% |
| CE | Count error | 0 | 0% |
| BW | Banned word slip | 0 | 0% |
| TD | Tone drift | 0 | 0% |
| EF | Exclusion failure | 0 | 0% |

### Notable observations

- The only two partial scores (P02 bold formatting, P16 word count boundary) reflect ambiguity in the constraint itself rather than a clear model failure — bold formatting is invisible in plain text, and the word count fell within the stated range.
- Persona constraints (P31–P40) produced some of the strongest responses in the dataset. The blacksmith (P31), detective (P39), and sports commentator (P40) responses showed complete character commitment without any breaks.
- Content exclusion constraints (P41–P50) were handled with creative substitution — for example, P44 replaced "ocean", "waves", "sand", and "sun" with "foaming crests", "golden grains", "peach-tinged sky", and "day's light" without losing descriptive quality.
- The model's weakest area across the dataset is formatting constraints that depend on rendering (e.g. bold text), which cannot be verified in plain text export — a methodological limitation noted for future iterations of this audit.

---

## Sample Annotated Row

| Field | Value |
|---|---|
| Prompt ID | P09 |
| Category | Format |
| Prompt | Write a pros and cons list for remote work. Label the two sections exactly "Pros" and "Cons". Include exactly 4 points in each section. |
| Constraint 1 | Exact labels "Pros" and "Cons" → **Score: 2 (Pass)** — Exact labels used |
| Constraint 2 | Exactly 4 points in Pros → **Score: 2 (Pass)** — 4 bullets confirmed |
| Constraint 3 | Exactly 4 points in Cons → **Score: 2 (Pass)** — 4 bullets confirmed |
| Points Earned | 6 / 6 |
| Compliance % | 100% |
| Failure Patterns | None |

---

## How to Replicate This Project

1. Use the 50 prompts in `prompts/50_prompts.md` to collect fresh responses from any AI model
2. Paste responses into the `Annotation Dataset` sheet, one per row
3. Read `annotation_guideline.md` to understand the scoring rules before annotating
4. Score each constraint (0, 1, or 2) in the C1–C4 score columns
5. Add failure pattern codes in the Failure Patterns column using the taxonomy above
6. Record any edge case decisions in the Annotator Notes column

This project can be replicated on any AI model (Claude, Gemini, Llama, etc.) to produce a comparable compliance benchmark.

---

## Inter-Rater Reliability

The annotation guideline is written to enable a second annotator to reproduce scores independently. All judgment calls are documented in the Annotator Notes column of the spreadsheet. Target agreement rate is Cohen's Kappa ≥ 0.75 (substantial agreement).

To run a reliability check: have a second annotator score a random 10-prompt sample (20% of the dataset) blind, then calculate Cohen's Kappa against the primary annotation.

---

## Skills Demonstrated

- Annotation rubric design with explicit, replicable per-constraint scoring rules
- Per-constraint compliance tracking across 5 distinct constraint categories
- Failure pattern taxonomy design and consistent application
- Annotation guideline writing with worked examples, edge case rules, and IRR guidance
- Inter-rater reliability framework using Cohen's Kappa
- Structured dataset documentation for public reproducibility

---

## Contact

**Annotator:** Muditha Buddhika  
**Email:** jmudithab@gmail.com 

---

*Dataset collected and annotated: April 2026*  
*AI model evaluated: GPT o3*  
*Annotation standard: v1.0 — see annotation_guideline.md*
