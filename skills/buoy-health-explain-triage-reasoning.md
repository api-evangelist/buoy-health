---
name: Explain why Buoy asked a question
description: >-
  Use the Buoy explain endpoint to surface the candidate diagnosis and predictors behind any interview
  question, for clinical review, model transparency, or answering "why are you asking me this?".
api: openapi/buoy-health-symptom-checker-openapi.yml
generated: '2026-08-08'
method: generated
source: openapi/buoy-health-symptom-checker-openapi.yml
operations:
  - questions_list
  - questions_read
  - questions_explain
  - interviews_read
---

# Explain why Buoy asked a question

Buoy exposes the reasoning behind its adaptive question sequence. This is the clinical-transparency
surface of the API and the answer to the most common user objection during a triage interview: *why
are you asking me that?*

## Prerequisites

An interview in progress with at least one generated question. See
`buoy-health-run-triage-interview.md` for how to get there. Bearer token required, as everywhere.

## Steps

### 1. Find the question — `questions_list` or `questions_read`

`GET /questions/?interview=<interview_token>` to enumerate, or
`GET /questions/{question_token}/` if you already hold the token.

Question tokens go stale whenever an earlier answer is edited, so re-list rather than reusing a
cached token after any `questions_update`.

### 2. Ask for the explanation — `questions_explain`

`GET /questions/{question_token}/explain/`

Note the trailing slash on the sub-resource path.

The `QuestionExplainSchema` response carries:

- `token` — the question this explains
- `interview` — the parent interview token
- `diagnosis` — the candidate condition the engine is currently probing
- `predictors` — the signals that raised that candidate

### 3. Present it honestly

Two constraints:

- **`diagnosis` here is a working hypothesis, not a result.** It is what the engine is testing at this
  step. Never present it to a patient as a finding — the only diagnostic output of the API is the
  `differential` on `results_read`.
- **Predictors are model inputs, not clinical evidence.** Describe them as "what the interview has
  picked up so far", not as test results.

For a patient-facing surface, prefer paraphrasing the intent ("this narrows down what's driving your
symptom") over dumping the raw `diagnosis` string. For a clinician-facing or audit surface, pass both
fields through unchanged.

### 4. Cross-check the alarm state — `interviews_read`

`GET /interviews/{interview_token}/`

If `should_display_alarm` is true, stop explaining and escalate. Emergency guidance always outranks
transparency.

## Errors

| Status | Meaning |
|---|---|
| `401` | Token missing/expired, or minted by the wrong environment's issuer |
| `404` | The question token does not exist, expired, or was invalidated by an earlier answer edit |

A `404` here most often means the question was regenerated after an answer was edited upstream —
re-run `questions_list` and explain the current question instead. See
`errors/buoy-health-problem-types.yml`.
