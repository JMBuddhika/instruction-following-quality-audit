# Annotation Guideline Document
## Instruction-Following Quality Audit

**Version:** 1.0  
**Project:** Instruction-Following Compliance Audit — 50 AI Responses  
**Annotator:** Muditha Buddhika
**Date:** April 2026  
**Overall Compliance Rate:** 99.4%

---

## 1. Purpose and Scope

This document defines the standards, procedures, and decision rules used to annotate AI-generated responses in the Instruction-Following Quality Audit project.

The audit evaluates whether AI language model responses comply with explicit constraints embedded in user prompts. Each of the 50 prompts in this dataset contains between 2 and 4 measurable constraints. Each constraint is scored independently on a 3-point scale, and an overall compliance percentage is calculated per response.

**Goals of this annotation project:**
- Quantify how often AI responses fully, partially, or fail to follow explicit instructions
- Identify recurring failure patterns across constraint types
- Produce a replicable dataset that a second annotator could reproduce with high agreement

**Scope — what this audit does and does not cover:**
- This audit covers instruction compliance only. It does not evaluate factual accuracy, writing quality, or creativity unless those are explicitly stated constraints in the prompt.
- Five constraint categories are covered: Format, Length, Tone/Audience, Role/Persona, and Content Exclusion.
- 50 prompts were annotated, containing 152 total individual constraints.

---

## 2. Label Definitions and Scoring Scale

Each constraint within a prompt is scored on the following 3-point scale:

| Score | Label | Definition |
|-------|-------|------------|
| **2** | PASS | The constraint is fully and precisely met with no meaningful deviation |
| **1** | PARTIAL | The constraint is partially met, or met in spirit but with a minor deviation |
| **0** | FAIL | The constraint is clearly ignored, violated, or directly contradicted |

### Calculating the Compliance Score

For each response:

```
Points Earned = sum of all constraint scores
Max Points    = number of constraints × 2
Compliance %  = (Points Earned ÷ Max Points) × 100
```

**Example:**  
A prompt has 3 constraints. Scores are: PASS (2), PARTIAL (1), FAIL (0).  
Points Earned = 3 | Max Points = 6 | Compliance = 50%

---

## 3. Constraint Category Rules

### 3A. Format Constraints

Format constraints require the response to follow specific structural rules: bullet points, numbered lists, tables, headings, or timelines.

**Scoring rules:**
- If the prompt says "bullet points" and the response uses a numbered list → PARTIAL (format intent met, exact format missed)
- If the prompt says "exactly 4 rows" in a table and the response has 3 or 5 → FAIL (count errors are always FAIL)
- If the prompt specifies exact heading text (e.g. "Ingredients", "Steps", "Tips") and the response uses different words → PARTIAL if close, FAIL if entirely different
- If the prompt says "no introduction" and the response adds even one sentence of preamble → FAIL
- Bold formatting cannot be verified in plain text exports. If a response uses bold-equivalent emphasis (ALL CAPS, asterisks, or clear typographic distinction), score PARTIAL. If the structure otherwise correctly labels the titles, do not score FAIL on bold alone.

### 3B. Length Constraints

Length constraints require responses to meet specific word counts, sentence counts, paragraph counts, or character limits.

**Scoring rules:**
- "Exactly N sentences" — count every sentence using terminal punctuation (. ? !). Off by 1 = PARTIAL. Off by 2 or more = FAIL.
- "Under N words" — count the full response text. Exceeding the limit by 1–5 words = PARTIAL. Exceeding by 6+ words = FAIL.
- "Between X and Y words" — within range = PASS. 1–5 words outside range = PARTIAL. More than 5 words outside range = FAIL.
- Character limits (e.g. tweet): use a character counter. Any overage = FAIL (no partial for character limits, as these are hard platform constraints).

**Note on sentence counting:** Bullet points that are full sentences are counted as sentences. Bullet points that are fragments are not counted as sentences.

**Note on word count boundaries:** For prompts specifying a range (e.g. "60–80 words"), use a consistent word counter. If the count falls clearly within the range, score PASS. If the count is ambiguous at the boundary (±3 words depending on how hyphenated words or abbreviations are counted), score PARTIAL and note the ambiguity.

### 3C. Tone and Audience Constraints

Tone constraints require the response to adopt a specific register, style, or target audience.

**Scoring rules:**
- Evaluate tone holistically across the full response, not just the opening.
- If the tone is correct for 75%+ of the response → PASS
- If the tone is correct for 40–74% of the response → PARTIAL
- If the response defaults to a generic AI tone throughout → FAIL
- "No jargon" — any single use of technical terminology not explained for a lay reader → PARTIAL. Multiple uses → FAIL.
- "No slang" — any use of slang terminology → FAIL.
- "End with a question" — if the response ends with a statement instead of a question → FAIL (binary constraint).
- "No contractions" — any contraction (it's, don't, I'm, etc.) → FAIL (binary constraint).

### 3D. Role and Persona Constraints

Persona constraints require the model to maintain a specific character, role, or historical perspective throughout the response.

**Scoring rules:**
- The persona must be established from the first sentence and maintained to the last.
- A single sentence breaking character → PARTIAL
- Two or more sentences breaking character, or breaking character at a key moment (opening/closing) → FAIL
- Historical persona constraints (e.g. "in 1985, before the internet") — any reference to internet, smartphones, or post-period technology → FAIL
- "Do not break character" — treat any character break as FAIL regardless of how brief
- Persona framing at the opening ("As your nutritionist..." or "Team, listen up.") is a positive signal — score PASS if maintained through the closing sentence

### 3E. Content Exclusion Constraints

Exclusion constraints prohibit the use of specific words, phrases, topics, examples, or types of content.

**Scoring rules:**
- Banned words are case-insensitive. "Health", "HEALTH", and "health" are all violations.
- Each unique banned word or phrase in the prompt is treated as a separate constraint.
- One violation of a banned word → FAIL for that constraint (no partial for exclusion constraints — either the word appears or it does not).
- Banned examples (e.g. "do not mention cutting out coffee") — if the response references the banned topic even indirectly → FAIL.
- "No advice" — any sentence using imperative mood or containing words like "should", "try", "consider", "recommend" → FAIL.
- Creative substitution for banned words (e.g. replacing "waves" with "foaming crests") → score PASS on that constraint and note the substitution in Annotator Notes.

---

## 4. Edge Case Decision Rules

**Ambiguous prompt constraints:**  
If a constraint could be interpreted in two reasonable ways, annotate based on the most literal reading of the prompt text. Document your interpretation in the "Annotator Notes" column.

**Implicit vs explicit constraints:**  
Only score constraints that are explicitly stated in the prompt. Do not penalize responses for missing things the prompt did not ask for.

**Overlapping constraints:**  
If two constraints are closely related (e.g. "use bullet points" and "no paragraphs"), treat them as separate constraints and score each independently.

**Rendering-dependent formatting:**  
Bold, italic, and other rich-text formatting cannot be verified in plain text. If a constraint requires formatting that only renders in a word processor or markdown viewer, note the limitation in the Annotator Notes column and score based on the intended formatting signal (e.g. asterisks around titles = intended bold).

**Partial responses:**  
If the AI response is clearly cut off mid-sentence, score only the constraints that can be evaluated from the available text. Note "truncated response" in the Annotator Notes column.

**Constraint not applicable:**  
If a constraint becomes unevaluable due to a preceding error (e.g. the prompt asks for exactly 3 bullet points and the response has none — the "under 10 words per bullet" constraint cannot be evaluated), score the unevaluable constraint as 0 (FAIL) and note the dependency in the Annotator Notes column.

---

## 5. Worked Examples

### Worked Example 1 — Full PASS

**Prompt (P09):**  
"Write a pros and cons list for remote work. Label the two sections exactly 'Pros' and 'Cons'. Include exactly 4 points in each section."

**Response:**  
> Pros  
> • Flexible schedule increases autonomy  
> • Saves commuting time and costs  
> • Expands job opportunities regardless of location  
> • Allows personalized workspace  
> Cons  
> • Blurs boundaries between work and personal life  
> • Can create feelings of isolation  
> • Harder to collaborate spontaneously  
> • Home distractions may reduce productivity

**Annotation:**

| Constraint | Score | Reasoning |
|---|---|---|
| Exact labels "Pros" and "Cons" | 2 — PASS | Exact labels used with no deviation |
| Exactly 4 points in Pros | 2 — PASS | 4 bullets confirmed by count |
| Exactly 4 points in Cons | 2 — PASS | 4 bullets confirmed by count |

**Points Earned:** 6 / 6 — **Compliance: 100%** — **Failure Patterns:** None

---

### Worked Example 2 — PARTIAL score

**Prompt (P02):**  
"Give me 3 tips for improving focus while studying. Number each tip. Bold the tip title. Write exactly 2 sentences of explanation under each tip."

**Response:**  
> 1. Set a Clear Goal  
> Define what you will accomplish in this session. A specific target directs your attention and reduces wandering thoughts.  
> 2. Use the Pomodoro Technique  
> Study for 25-minute intervals followed by 5-minute breaks. These short sprints sustain concentration and prevent mental fatigue.  
> 3. Eliminate Digital Distractions  
> Silence notifications and place your phone out of reach. Removing tempting alerts lets your brain stay fully on the material.

**Annotation:**

| Constraint | Score | Reasoning |
|---|---|---|
| Exactly 3 tips | 2 — PASS | 3 tips confirmed |
| Numbered list | 2 — PASS | 1. 2. 3. format used correctly |
| Bold titles | 1 — PARTIAL | Titles are structurally distinct but bolding cannot be verified in plain text output — marked partial per rendering-dependent formatting rule |
| Exactly 2 sentences each | 2 — PASS | Each tip has exactly 2 sentences of explanation confirmed |

**Points Earned:** 7 / 8 — **Compliance: 87.5%** — **Failure Patterns:** None  
**Annotator Notes:** Bold formatting ambiguous in plain text export. Scored partial on bold constraint only; all other constraints fully met.

---

### Worked Example 3 — Strong Persona Execution (P39)

**Prompt:**  
"You are a detective briefing your team on a missing object case — the missing object is a stapler from an office. Stay in character as a serious detective throughout. Treat it with the full gravity of a real case."

**Response:**  
> Team, listen up.  
> Incident Overview: At 09:17 this morning, Accounting reported the disappearance of their red Swingline-type stapler from Desk A-14. Last confirmed sighting was 18:05 yesterday.  
> Evidence Collected: Desk surface bears a faint rectangular dust outline consistent with recent removal. Single staple found on the floor, two metres east of the workstation. CCTV in corridor 4B experienced a five-minute blackout starting 22:12.  
> Immediate Actions: Interview cleaning personnel individually within the next hour. Pull badge-reader data for adjacent departments. Forensically examine the loose staple for latent prints.

**Annotation:**

| Constraint | Score | Reasoning |
|---|---|---|
| Detective persona | 2 — PASS | Police briefing format (incident, evidence, actions) used precisely and maintained throughout |
| Serious tone maintained | 2 — PASS | No irony or comedy anywhere — full earnestness |
| Mundane subject treated gravely | 2 — PASS | "Forensically examine the loose staple for latent prints" — complete commitment to the premise |

**Points Earned:** 6 / 6 — **Compliance: 100%** — **Failure Patterns:** None  
**Annotator Notes:** One of the strongest persona responses in the dataset. The briefing structure mirrors real police procedure.

---

## 6. Actual Findings from This Annotation

### Compliance by category

| Category | Prompts | Avg Compliance % |
|---|---|---|
| Format | P01–P10 | 99.7% |
| Length | P11–P20 | 99.7% |
| Tone & Audience | P21–P30 | 100.0% |
| Role & Persona | P31–P40 | 100.0% |
| Content Exclusion | P41–P50 | 100.0% |
| **Overall** | **P01–P50** | **99.4%** |

### Failure patterns observed

No failure patterns (OA, FD, PD, CE, BW, TD, EF) were observed across any of the 50 responses. All 7 pattern counters recorded 0 occurrences.

### Key observations from annotating
- The two partial scores (P02 bold formatting; P16 word count boundary) reflect constraint ambiguity rather than clear model failures.
- Persona constraints showed complete character commitment in all 10 responses — no breaks detected.
- Exclusion constraints were handled with creative lexical substitution rather than awkward avoidance.
- The model's most reliable constraint types were content exclusion and tone — both achieved 100% compliance.

---

## 7. Inter-Rater Reliability

This dataset is designed so that a second annotator could reproduce the scores. To achieve this:

- All scoring decisions follow the explicit rules in Sections 3 and 4.
- Any decision that required judgment beyond these rules is documented in the "Annotator Notes" column.
- A second annotator should be given only this guideline document and the raw prompt/response pairs — not the completed scores — to perform a blind re-annotation.

**Target agreement rate:** Cohen's Kappa ≥ 0.75 (substantial agreement) across all scored constraints.

**How to calculate Cohen's Kappa:**  
For any pair of annotations, calculate observed agreement (percentage of constraints scored identically) and expected agreement (based on marginal frequencies of each score level). Cohen's Kappa = (Observed − Expected) / (1 − Expected).

**Suggested reliability sample:** Have a second annotator score a random 10-prompt sample (20% of the dataset) blind, then calculate Cohen's Kappa against the primary annotation scores.

---

## 8. Failure Pattern Code Reference

| Code | Pattern | Description | Prompts Triggered |
|------|----------|-------------|-------------------|
| OA | Over-answering | Response gave more content than the prompt requested | 0 |
| FD | Format drift | Correct format at start, switched format mid-response | 0 |
| PD | Persona drop | Lost the assigned role or character partway through | 0 |
| CE | Count error | Wrong number of bullets, sentences, paragraphs, or items | 0 |
| BW | Banned word slip | Used a word or phrase explicitly prohibited by the prompt | 0 |
| TD | Tone drift | Correct register at start, reverted to default AI tone | 0 |
| EF | Exclusion failure | Included content, topics, or examples the prompt prohibited | 0 |

Multiple patterns per response are recorded as comma-separated codes (e.g. "PD, TD").

---

## 9. Dataset Structure Reference

The annotation spreadsheet (`instruction_following_audit.xlsx`) contains four sheets:

| Sheet | Purpose |
|-------|---------|
| Annotation Dataset | Main annotation workspace — one row per prompt with all scores filled in |
| Scoring Key | Color-coded reference table for score definitions (green/amber/red) |
| Failure Pattern Tracker | Running tally of each failure type with auto-calculated percentages |
| Summary Dashboard | Compliance averages by category and key findings summary |

**Key columns in the Annotation Dataset sheet:**

| Column | Contents |
|--------|----------|
| A | Prompt ID (P01–P50) |
| B | Category |
| C | Prompt Text |
| D | AI Response |
| E | Constraints (pipe-separated) |
| F | Number of Constraints |
| G | Max Points |
| H | C1 Label |
| I | C1 Score (0/1/2 — color coded) |
| J | C1 Notes |
| K–M | C2 Label / Score / Notes |
| N–P | C3 Label / Score / Notes |
| Q–S | C4 Label / Score / Notes |
| T | Points Earned |
| U | Compliance % (color coded: green ≥95%, amber 75–94%, red <75%) |
| V | Failure Patterns |
| W | Annotator Notes |

---

*End of Annotation Guideline Document v1.0*
