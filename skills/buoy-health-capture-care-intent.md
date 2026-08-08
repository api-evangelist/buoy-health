---
name: Capture care-navigation intent
description: >-
  Record what a user intends to do at the start and end of a Buoy triage interview, so an integrator
  can measure whether triage actually moved them to the right level of care.
api: openapi/buoy-health-symptom-checker-openapi.yml
generated: '2026-08-08'
method: generated
source: openapi/buoy-health-symptom-checker-openapi.yml
operations:
  - intents_create
  - intents_list
  - intents_read
  - intents_update
  - intents_delete
  - results_read
---

# Capture care-navigation intent

Intents are the measurement hook in the Buoy API. They record what the user *intended to do* before
the interview and what they *decided to do* after it — the delta that shows whether triage changed
behaviour. This is the flow that produces the over-triage / under-triage numbers Buoy sells on.

## The two events

The `IntentEventProperty` on `IntentSchema` takes one of two lifecycle events:

- **`INTERVIEW_START`** — captured at the beginning of an interview, or immediately after the chief
  complaint is entered. "What were you going to do about this?"
- **`INTERVIEW_RESULT`** — captured at the end, after `results_read`. "What are you going to do now?"

Buoy's reference documentation states that an empty string (`""`) should always be submitted in the
`detail` parameter.

## Steps

### 1. Capture the starting intent — `intents_create`

`POST /intents/?interview=<interview_token>`

Send `event: INTERVIEW_START` and the user's `decision`.

Do this **before** the question sequence biases the answer. If you ask after triage, you have
measured Buoy's output, not the counterfactual.

Keep the returned `token`.

There is no `Idempotency-Key` on this API. A retried POST after a timeout creates a second intent and
double-counts the user. Call `intents_list` to check before retrying.

### 2. Run the interview

See `buoy-health-run-triage-interview.md`.

### 3. Capture the resulting intent — `intents_create`

After `results_read` returns, `POST /intents/?interview=<interview_token>` again with
`event: INTERVIEW_RESULT` and the decision the user actually made.

If the result carried `alarm: true`, escalate first and capture the intent second. Never delay
emergency guidance to collect analytics.

### 4. Read back and correct — `intents_list`, `intents_read`, `intents_update`

- `GET /intents/?interview=<interview_token>` — every intent on the interview.
- `GET /intents/{intent_token}/` — one intent.
- `PUT /intents/{intent_token}/` — correct an `event` or `decision`. Prefer updating a mis-recorded
  intent over creating a second one, or your funnel will double-count.

### 5. Delete only on user request — `intents_delete`

`DELETE /intents/{intent_token}/` returns `204`.

Use this to honour a user's request to withdraw their recorded decision. Do not use it to clean up
analytics you dislike.

## Analysis note

The pair of intents is only meaningful together. An `INTERVIEW_RESULT` with no matching
`INTERVIEW_START` tells you where someone landed but not whether Buoy moved them, so treat unpaired
intents as unusable rather than as evidence of impact.

## Errors

| Status | Body | Meaning |
|---|---|---|
| `400` | `["message", ...]` | Invalid `event`, `decision`, or a bad `interview` token |
| `401` | *(empty)* | Token missing/expired or from the wrong environment's issuer |
| `404` | `{"detail": "Not found."}` | The intent token does not exist |

See `errors/buoy-health-problem-types.yml` and `conventions/buoy-health-conventions.yml`.
