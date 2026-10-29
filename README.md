# Buoy Health

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Buoy Health is a Boston-based digital health company whose clinically-trained AI symptom checker and triage engine is offered to health systems, payers and employers as an embeddable API. The Buoy Symptom Checker API v2.0 exposes a conversational triage interview as REST resources — interviews, complaints, queries, intents, questions and results — so an integrator can create a de-identified anonymous interview from basic demographics, accept a chief complaint, walk an adaptively generated question sequence, and read back a differential diagnosis with a recommended level of care and customizable care-handoff destinations. The API is OAuth 2.0 protected against an Auth0-backed authorization server with separate sandbox and production environments, is documented on ReadMe with a published OpenAPI 3.0.1 definition and an llms.txt, and Buoy is HITRUST CSF certified.

**Areas:** Symptom Checker · medical-triage · Digital Health · Healthcare · Clinical AI · Care Navigation · Patient Engagement · Diagnosis · Telehealth · Authentication

## Buoy Health on APIs.io

Everything profiled for Buoy Health is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/buoy-health/](https://apis.io/providers/buoy-health/)** — or go straight to one of the APIs or properties below.

### APIs (7)

- [Buoy Authorization Server](https://apis.io/apis/buoy-health/buoy-authorization-server/) — Auth0-backed OAuth 2.0 / OpenID Connect authorization server that issues the bearer tokens the Buoy Symptom Checker API requires. Publishes anonymous RFC…
- [Buoy Health Complaints API](https://apis.io/apis/buoy-health/buoy-health-complaints-api/) — The Complaints API from Buoy Health — 2 operation(s) for complaints.
- [Buoy Health Intents API](https://apis.io/apis/buoy-health/buoy-health-intents-api/) — The Intents API from Buoy Health — 2 operation(s) for intents.
- [Buoy Health Interviews API](https://apis.io/apis/buoy-health/buoy-health-interviews-api/) — The Interviews API from Buoy Health — 2 operation(s) for interviews.
- [Buoy Health Queries API](https://apis.io/apis/buoy-health/buoy-health-queries-api/) — The Queries API from Buoy Health — 1 operation(s) for queries.
- [Buoy Health Questions API](https://apis.io/apis/buoy-health/buoy-health-questions-api/) — The Questions API from Buoy Health — 3 operation(s) for questions.
- [Buoy Health Results API](https://apis.io/apis/buoy-health/buoy-health-results-api/) — The Results API from Buoy Health — 1 operation(s) for results.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [buoy-health](https://apis.io/servers/buoy-health/buoy-health/)

#### Collections (6)

Postman and Open Collection request collections.

- [Buoy Symptom Checker Complaints API](https://apis.io/collections/buoy-health/open-buoy-health-complaints-api/)
- [Buoy Symptom Checker Intents API](https://apis.io/collections/buoy-health/open-buoy-health-intents-api/)
- [Buoy Symptom Checker Interviews API](https://apis.io/collections/buoy-health/open-buoy-health-interviews-api/)
- [Buoy Symptom Checker Queries API](https://apis.io/collections/buoy-health/open-buoy-health-queries-api/)
- [Buoy Symptom Checker Questions API](https://apis.io/collections/buoy-health/open-buoy-health-questions-api/)
- [Buoy Symptom Checker Results API](https://apis.io/collections/buoy-health/open-buoy-health-results-api/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Buoy Health Symptom Checker Overlay](https://apis.io/overlays/buoy-health/buoy-health-symptom-checker-overlay/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Buoy Health Authentication](https://apis.io/security/buoy-health/buoy-health-authentication/)
- [Buoy Health Domain Security](https://apis.io/security/buoy-health/buoy-health-domain-security/)
- [Buoy Health Vulnerability Disclosure](https://apis.io/security/buoy-health/buoy-health-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Buoy Health Scopes](https://apis.io/scopes/buoy-health/buoy-health-scopes/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.buoyhealth.com/)
- [Documentation](https://buoyhealth.readme.io/reference/interviews_anonymous)
- [API reference](https://buoyhealth.readme.io/reference)
- [Support](https://www.buoyhealth.com/contact-us)
- [Blog](https://www.buoyhealth.com/blog)
- [Security](https://www.buoyhealth.com/security-and-privacy)
- [Compliance](https://www.buoyhealth.com/security-and-privacy)
- [Terms of service](https://www.buoyhealth.com/terms)
- [Privacy policy](https://www.buoyhealth.com/privacy)

## API Evangelist rating

**Kin Score 45.5** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.2 |
| Contract quality | 52.1 |
| Contract governance | 18.2 |
| Operational transparency | 10.5 |
| Developer ergonomics | 53.0 |
| Access clarity | 28.9 |

**Agent readiness 34.9** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | derived (partial credit) |
| OpenAPI Examples | verified |
| Rate Limit Signal | — |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | — |
| Dynamic Client Registration | yes |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | distinct |

**Regulatory alignment 51.6** — Health

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/buoy-health/](https://apis.io/providers/buoy-health/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Buoy Health: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/buoy-health/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
