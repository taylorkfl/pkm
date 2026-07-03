---
type: concept
repo: keyframe-live-avatar-demos
status: growing
lifecycle_stage: design
importance: high
last_reviewed: 2026-06-29
aliases:
  - Travel Consultant Demo
  - Itinerary Avatar
  - Flight Planning Avatar
tags:
  - code-pkm
  - repo/keyframe-live-avatar-demos
  - pkm/type/concept
  - pkm/domain/product
---

# Travel Consultant Avatar Demo

## Objective
Demonstrate a Keyframe Labs live avatar that acts as a travel consultant with access to flight information and guides a user through an itinerary.

## Description
### What
The user tells the avatar where they want to go, when they need to travel, and what constraints matter. The avatar checks flight information, explains tradeoffs, and walks the user through a realistic itinerary: outbound flight, return flight, layovers, airport timing, hotel or ground-transport notes, and contingency plans.

The strongest version feels like a patient travel expert rather than a booking form. The avatar can say, "Here are the two best options, here is the risk with the short layover, and here is what I would choose if arrival time matters most."

### Why
This demo teaches customers that live avatars can orchestrate tool-backed workflows. It is not only conversational search; it is a guided decision experience that combines user preferences, real-time data, explanation, and a durable itinerary artifact.

### When
Use this demo for customers interested in travel, hospitality, concierge services, customer support, insurance, corporate travel, event planning, or any workflow where users need help comparing options with changing external data.

### How
Recommended flow:

1. User selects "Travel Consultant."
2. Avatar asks for destination, origin, dates, traveler count, budget, arrival constraints, and preferences.
3. Backend retrieves flight options, flight status if relevant, airport details, and known schedule constraints.
4. Avatar presents a small number of itinerary paths and explains the tradeoffs.
5. User chooses or adjusts priorities such as cheapest, fastest, lowest risk, refundable, or loyalty-friendly.
6. Avatar builds a final itinerary and highlights next actions.
7. Session ends with a travel brief containing selected flights, timing, assumptions, alerts, and unresolved booking steps.

## Scope
| Area | MVP | Improvement |
| --- | --- | --- |
| Flight information | Curated sample flight data or one flight-search provider | Live search, flight status, fare rules, seat maps, baggage, and disruption alerts |
| User preferences | Origin, destination, dates, budget, arrival deadline | Loyalty programs, accessibility, family travel, visa/passport notes, carbon preference |
| Itinerary guidance | Compare 2-3 options and pick a recommended path | Multi-city trips, hotels, ground transport, calendar handoff, expense policy |
| Avatar role | Friendly travel consultant | Corporate travel advisor, luxury concierge, event travel coordinator, airline support agent |
| Artifact | Trip summary with selected options | Booking handoff, calendar events, wallet passes, alerts, and support escalation |

## Use Case
Use this demo when a customer needs to see a live avatar operate over changing external data and help a user make a practical decision.

## Stage In Lifecycle
Design. This is a strong follow-on demo after the shared shell exists because it exercises tool calls, external data freshness, and structured post-session output.

## Importance
High. Travel planning is familiar, emotionally legible, and naturally benefits from a conversational guide that can explain tradeoffs.

## Risks
- Flight data can be stale, incomplete, delayed, or inconsistent across providers.
- Travel recommendations may affect money, missed flights, accessibility needs, visas, or safety, so uncertainty must be visible.
- The demo should avoid implying that a flight is booked unless a booking handoff actually happened.
- Fare prices and availability can change while the user is talking.
- Tool latency can make the avatar feel slow unless the experience includes progress states and concise summaries.
- Customer pilots may require provider contracts, travel data licensing, and clear data-retention policies.

## Input / Output
| Input | Output |
| --- | --- |
| Origin, destination, dates, traveler count, budget, and preferences | Search query and ranked itinerary options |
| Flight search, schedule, status, airport, and policy data | Spoken recommendations with tradeoffs |
| User selection and constraints | Final itinerary brief |
| Session transcript and tool-call results | Follow-up tasks, alerts, and booking handoff checklist |

## Resources
- [[Demo Roadmap]]
- [[Demo Tech Stack]]
- [[Shared Supporting Features]]
- [[Demo Questions]]
- [[Repositories/monorepo/Concepts/Per-Session Context|Per-Session Context]]
- [[Repositories/monorepo/Concepts/Voice Agent Details|Voice Agent Details]]
- [[Repositories/monorepo/Workflows/Real-Time Persona Session|Real-Time Persona Session]]

External references:
- Integration overview: https://docs.keyframelabs.com/guides/integrate/overview
- Self managed quickstart: https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed
- JavaScript SDK reference: https://docs.keyframelabs.com/sdk-reference/javascript

## Related Topics
- [[Interview Preparation Avatar Demo]]
- [[Helpful Student Aid Avatar Demo]]
- [[Zoom Active Participant Avatar Demo]]
- [[Repositories/monorepo/Technologies/OpenAI Realtime|OpenAI Realtime]]
- [[Repositories/monorepo/Technologies/WebSockets|WebSockets]]
- [[Repositories/monorepo/Technologies/Supabase|Supabase]]

## Evidence
- User-provided idea on 2026-06-29: the AI avatar is a travel consultant that has access to flight information and walks the user through an itinerary.
- Keyframe Labs docs describe hosted and self managed integration lanes for live avatar sessions: https://docs.keyframelabs.com/guides/integrate/overview
- Keyframe Labs JavaScript docs describe `PersonaEmbed` for hosted avatar experiences and `PersonaView` for self managed experiences: https://docs.keyframelabs.com/sdk-reference/javascript
- Existing vault context: `Repositories/monorepo/Concepts/Per-Session Context.md`
- Existing vault context: `Repositories/monorepo/Concepts/Voice Agent Details.md`
