---
type: concept
repo: keyframe-live-avatar-demos
status: growing
lifecycle_stage: design
importance: high
last_reviewed: 2026-06-28
aliases:
  - Interview Prep Demo
  - Mock Interview Avatar
tags:
  - code-pkm
  - repo/keyframe-live-avatar-demos
  - pkm/type/concept
  - pkm/domain/product
---

# Interview Preparation Avatar Demo

## Objective
Demonstrate how a Keyframe Labs live avatar can run realistic mock interviews using job-specific and interviewer-specific context.

## Description
### What
The user uploads or selects a job description and an interviewer profile or resume. The avatar becomes a mock interviewer who asks role-relevant questions, adapts follow-ups to the interviewer's background, and provides a structured post-call feedback report.

The strongest version has two modes:

- Interviewer mode: the avatar behaves like a realistic interviewer and withholds coaching until the end.
- Coach mode: the avatar pauses, explains what went well, and helps the candidate practice a better answer.

### Why
This demo teaches a customer that Keyframe live avatars can do more than generic conversation. They can bind to a high-stakes context, maintain a role, capture a live interaction, and produce an artifact that a user can act on.

### When
Use this demo for customers interested in training, recruiting, onboarding, sales enablement, leadership coaching, or any workflow where practice with a lifelike human presence matters.

### How
Recommended flow:

1. User selects "Interview Preparation."
2. User adds a job description, optional candidate resume, and interviewer profile or resume.
3. System creates a concise interview brief, rubric, and first question.
4. Avatar starts the interview and keeps the user in the experience for a fixed round length.
5. Avatar asks follow-ups based on the user's answers and the uploaded context.
6. Session ends with a feedback artifact: strengths, gaps, suggested answer patterns, and practice tasks.

## Scope
| Area | MVP | Improvement |
| --- | --- | --- |
| Input context | Job description and interviewer profile | Candidate resume, company values, interview stage, question bank |
| Interview flow | Behavioral and role-fit questions | Technical rounds, case interviews, panel simulation |
| Avatar role | One curated interviewer persona | Multiple personas for recruiter, hiring manager, peer, executive |
| Feedback | Scorecard and summary | Rubric-backed evidence, transcript snippets, next practice plan |
| Customer education | Show per-session context | Show privacy controls, data deletion, scoring, and evaluation |

## Use Case
Use this demo when a customer needs to see avatars as high-fidelity practice partners rather than simple support agents.

## Stage In Lifecycle
Design. This is the best candidate for the first prototype because it does not require meeting-bot infrastructure and clearly demonstrates per-session context.

## Importance
High. The demo is emotionally intuitive and shows why a face-to-face avatar changes the training experience.

- Submit I have had no income 30 days and I used savings to cover my monthly savings.
- Submit new income and reevaluate.
## Risks
- Interviewer resumes and candidate resumes are sensitive personal data.
- The avatar must avoid discriminatory or legally risky hiring advice.
- Feedback should be framed as practice guidance, not an employment decision.
- The system should not pretend to know private company interview criteria unless provided.
- Mock interviews can feel harsh if the avatar tone is not carefully designed.
- A single generic scoring rubric may not fit technical, behavioral, and executive interviews.

## Input / Output
| Input | Output |
| --- | --- |
| Job description | Competency map, role themes, and interview question plan |
| Interviewer resume or profile | Interviewer-style context and likely areas of emphasis |
| Optional candidate resume | Tailored questions and gap analysis |
| Live interview transcript | Feedback report, scorecard, and next practice plan |

## Resources
- [[Demo Roadmap]]
- [[Demo Tech Stack]]
- [[Shared Supporting Features]]
- [[Demo Questions]]
- [[Repositories/monorepo/Concepts/Per-Session Context|Per-Session Context]]
- [[Repositories/monorepo/Concepts/Dynamic Variables|Dynamic Variables]]
- [[Repositories/monorepo/Workflows/Real-Time Persona Session|Real-Time Persona Session]]

External references:
- Self managed quickstart: https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed
- JavaScript SDK reference: https://docs.keyframelabs.com/sdk-reference/javascript
- Persona library: https://docs.keyframelabs.com/guides/resources/persona-library

## Related Topics
- [[Helpful Student Aid Avatar Demo]]
- [[Shared Supporting Features]]
- [[Repositories/monorepo/Concepts/Persona|Persona]]
- [[Repositories/monorepo/Technologies/OpenAI Realtime|OpenAI Realtime]]
- [[Repositories/monorepo/Technologies/ElevenLabs|ElevenLabs]]

## Evidence
- User-provided idea: interview preparation should allow mock interviews using the job description and interviewer's resume as context.
- Keyframe Labs self managed quickstart describes using `PersonaView` with session details and provider-specific voice agent details: https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed
- Keyframe Labs JavaScript docs describe `PersonaEmbed` for hosted pipelines and `PersonaView` for self managed pipelines: https://docs.keyframelabs.com/sdk-reference/javascript
- Existing vault context: `Repositories/monorepo/Concepts/Per-Session Context.md`
- Existing vault context: `Repositories/monorepo/Concepts/Dynamic Variables.md`
