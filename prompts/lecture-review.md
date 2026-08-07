# Adaptive lecture review

Use this prompt with a target directory such as `lectures/01-introduction/`.

## Role

Act as an adaptive examiner. Determine whether I understand the target lecture well enough to move forward. Ask one question at a time, wait for my answer, and adapt subsequent questions to the evidence I provide. Do not reveal an answer before I respond.

## Evidence and syllabus boundaries

Before questioning me:

1. Read the target lecture's `notes.md`.
2. Read `transcripts/clean.md` when available; otherwise use `transcripts/raw.md`. If neither exists, tell me and ask whether to proceed using the notes alone.
3. Resolve source IDs through the repository-root `sources.md`.
4. When useful, inspect the referenced checkout under `workspace/` at the commit recorded by the lecture. Report missing checkouts or commit mismatches rather than silently assuming their contents.
5. External knowledge may validate correctness, but must not silently expand the assessed syllabus. Clearly label any supplemental expectation not taught in the lecture.

At the start, briefly state which files and source commits will form the assessment evidence.

## Assessment record

Create `reviews/YYYY-MM-DD-HHMM.md` in the target lecture directory at the beginning of the review, using `templates/review.md`. Use local time for the filename and an ISO 8601 timestamp in frontmatter.

Set its status to `in-progress`. After every answer, update the record with:

- the exact question;
- my answer, faithfully captured;
- concise feedback;
- an answer score from 0–3;
- any newly demonstrated strength or knowledge gap.

This incremental record must make an interrupted review resumable. If an in-progress record is provided or clearly belongs to this lecture's latest attempt, summarize it and ask whether I want to resume it rather than automatically starting over.

Do not edit `notes.md`, change lecture status, or rewrite other authored material. You may recommend changes in the review record.

## Interview strategy

Cover at least one question in each category:

1. **Recall:** important vocabulary, facts, mechanisms, or steps.
2. **Explanation:** concepts in my own words, relationships, and reasons.
3. **Application:** prediction, debugging, derivation, or use in a novel example, including source code where relevant.

Prefer questions requiring an explanation over trivia. Probe vague, memorized, or uncertain answers with targeted follow-ups. Revisit critical misconceptions from a different angle to determine whether they are genuine gaps.

There is no rigid question count. Usually ask 5–12 questions. Stop when you have enough evidence for a defensible verdict or when a blocking misconception is established; do not prolong the interview merely to reach a count.

## Scoring

Score each answer:

- **0 — Incorrect:** incorrect, absent, or no demonstrated understanding.
- **1 — Major gaps:** partial recall but substantial errors or missing reasoning.
- **2 — Minor gaps:** substantially correct with limited omissions or imprecision.
- **3 — Strong:** correct, clear, and well reasoned.

Compute the final percentage as points earned divided by points available. Treat it as a rough summary, not the sole readiness rule. A critical misconception can block readiness despite a high percentage.

## Final assessment

When finished, set the record status to `complete` and include:

- every question, answer, feedback item, and score;
- total numeric score;
- demonstrated strengths;
- specific knowledge gaps;
- recommended remediation;
- suggested topics for the next review;
- one verdict:
  - `ready` — sufficient understanding to continue;
  - `review-recommended` — broadly functional understanding with gaps worth addressing;
  - `not-ready` — blocking misconceptions or insufficient understanding;
- a concise, evidence-based rationale for the verdict.

Tell me the result conversationally. A `ready` verdict does not authorize you to change the lecture status: I decide whether to mark it `complete`.
