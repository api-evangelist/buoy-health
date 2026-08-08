---
name: Run a Buoy triage interview end to end
description: >-
  Create an anonymous Buoy symptom-checking interview, submit and clarify a chief complaint, answer the
  adaptively generated questions, and read the differential result including the emergency alarm flag.
api: openapi/buoy-health-symptom-checker-openapi.yml
generated: '2026-08-08'
method: generated
source: openapi/buoy-health-symptom-checker-openapi.yml
operations:
  - interviews_anonymous
  - queries_list
  - complaints_create
  - complaints_read
  - complaints_update
  - questions_list
  - questions_read
  - questions_update
  - interviews_read
  - interviews_update
  - results_read
---

# Run a Buoy triage interview end to end

This is the marquee flow of the Buoy Symptom Checker API: turn a person describing symptoms into a
ranked differential diagnosis and a recommended level of care.

## Before you start

**This is a medical triage surface, not a general chat API.** Two rules override everything else:

1. If `results_read` returns `alarm: true`, or `interviews_read` returns
   `should_display_alarm: true`, surface Buoy's emergency guidance **verbatim and immediately**. Do
   not summarise it, do not defer it to the end of a response, and do not continue the interview.
2. Never present the differential as a diagnosis. It is a ranked list of candidate conditions with a
   recommended level of care.

**Auth.** Every operation requires an OAuth 2.0 bearer token. The token must come from the issuer
that matches the host you are calling — `auth.sandbox.buoyhealth.com` for
`api.sandbox.buoyhealth.com`, `auth.buoyhealth.com` for `api.buoyhealth.com`. The OpenAPI hardcodes
the *sandbox* issuer in its oauth2 flow even though it lists both servers, so a client generated
straight from the spec will get a 401 against production until you override the token URL. See
`authentication/buoy-health-authentication.yml`.

**Trailing slashes are required.** Every path ends in `/`. Do not normalise them away.

## Steps

### 1. Create the interview — `interviews_anonymous`

`POST /interviews/anonymous/`

Send the de-identified profile. Age and sex are the minimum. This creates an interview with no stored
user identity — prefer it over an authenticated interview unless you genuinely need a persistent
profile.

Keep the returned `token`. It is the `interview` value every subsequent call needs.

The interview starts in `mode: input`.

### 2. (Optional) Help the user phrase the complaint — `queries_list`

`GET /queries/?text=<partial>`

Symptom autocomplete. `text` is **required** — omitting it returns `400` with
`["Must provide query parameter 'text'"]`. This is the only operation in the API that is not scoped to
an interview, so it is safe to call before step 1.

### 3. Submit the chief complaint — `complaints_create`

`POST /complaints/?interview=<interview_token>`

Send the user's free-text description. Two outcomes matter:

- **`201`** — the complaint was created and the AI returned `interpretations`. Continue to step 4.
- **`204`** — the complaint was accepted but the AI could not interpret the text. This is **not an
  error**. Ask the user to rephrase and call `complaints_create` again. Do not retry the same string.

If the interview was already in `protocol` or `differential` mode, adding a complaint resets it to
`input`.

There is no `Idempotency-Key`. If the request times out, call `complaints_list` to check whether it
landed before retrying — a blind retry creates a duplicate complaint.

### 4. Clarify the complaint — `complaints_read` then `complaints_update`

`GET /complaints/{complaint_token}/` returns the candidate `interpretations`, each with a `token`,
`title` and `description`.

Present them to the user and let them choose. Then
`PUT /complaints/{complaint_token}/` with the chosen interpretation as the `clarification`.

Do not choose on the user's behalf when two interpretations are clinically distinct — that choice
steers the entire question sequence.

### 5. Walk the question sequence — `questions_list`, `questions_read`, `questions_update`

`GET /questions/?interview=<interview_token>` lists the questions generated so far.

For each unanswered question:

- `GET /questions/{question_token}/` for its `text`, `options` and any `media`.
- Respect `options[].exclusive` — an exclusive option cannot be combined with others.
- `PUT /questions/{question_token}/` with the user's `answer`.

Then follow `_links.next` on the response to the next question. Loop until `_links.next` is null.

**The critical side effect:** if the user goes back and changes an earlier answer, the interview
truncates at that question and regenerates every downstream question. Every question token you cached
after that point is stale. Re-run `questions_list` rather than reusing them.

### 6. Advance the interview mode — `interviews_update`

`PUT /interviews/{interview_token}/` to move between `input`, `protocol` and `differential`.

- **`202`** means accepted-but-still-generating. Poll `interviews_read` until the new sequence exists.
- **`409`** means the interview cannot transition — it is complete, has no chief complaint, or has an
  unanswered question. The body is a bare string such as `"Interview has an unanswered question."`
  Drain the question queue and retry.

### 7. Read the result — `results_read`

Follow `_links.result` from `interviews_read` — do not construct the URL yourself. Then
`GET /results/{result_token}/`.

Handle three cases:

- **`alarm: true`** — emergency. Escalate immediately per the rule at the top of this skill.
- **`undiagnosed: true`** — the engine reached no differential. Say so plainly; do not fill the gap
  with your own guess.
- **`204`** — no result body yet. Not an error. The interview did not produce a differential.

Otherwise render the `differential` array in the order Buoy returned it. **Do not re-rank it.**

## Errors you will actually hit

| Status | Body shape | What it means |
|---|---|---|
| `400` | `["message", ...]` | Missing/invalid parameter or body field |
| `401` | *(empty)* | Token missing, expired, or from the wrong environment's issuer |
| `404` | `{"detail": "Not found."}` | Token does not exist, expired, or belongs to another client |
| `409` | `"..."` (bare string) | Interview not in an updatable state |

There is no RFC 9457 problem+json, no stable error code, and no declared `5xx`. Branch on the status
code, and treat any `5xx` as an undocumented transport failure. See
`errors/buoy-health-problem-types.yml`.

No rate limit is published and no `RateLimit-*` headers are returned, so apply your own conservative
client-side pacing rather than probing for the ceiling.
