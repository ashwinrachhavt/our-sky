# Build Our Sky: A Rails Engineering Apprenticeship

> Source: internal Notion document, migrated on 2026-09-20.

## The name

Our Sky is a better working name than Costar. It says what the product is about: a private sky shared with someone you care about. It is warm, direct, and not tied to the Co–Star brand. Check trademark and domain availability before shipping.

## The outcome

No tutorial makes someone senior. Repeated ownership does. This apprenticeship trains the work senior engineers are paid to do: choose boundaries, model durable truth, make failure boring, protect data, debug across layers, operate production, and explain trade-offs.

**Engineering bar:** defend every table, endpoint, background job, dependency, cache, and service boundary. "The tutorial said so" is not a defense.

## The system

- One Rails 8.1 monolith owns routing, HTML rendering, domain rules, authorization, jobs, and durable state.
- PostgreSQL is the source of durable truth. Use database constraints to defend invariants.
- ERB and Hotwire deliver the responsive browser experience. Add a second frontend only after evidence demands it.
- Solid Queue handles background work. External astrology and AI remain replaceable inputs; the core product works when they are down.

## Initial model constraint

Begin with three models:

- `User`: identity and ownership; start with Rails authentication conventions.
- `Session`: a revocable login record managed by the Rails authentication flow.
- `Reflection`: one private piece of durable user value.

Add `Relationship` and `Reading` only when their behavior enters the next vertical slice.

## Working method

1. Read the point and write the decision in your own words.
2. Type it yourself; do not paste generated domain code.
3. Inspect the schema, SQL, headers, logs, and browser network trace.
4. Break it: senior judgment grows from failure, not only happy paths.
5. Use review prompts only after an attempt; reject machinery without a use case.
6. Meet exit criteria, commit, and keep diffs small.

Maintain `ENGINEERING_LOG.md` with the decision, alternatives, invariant, failure mode, operational signal, and a revisit trigger for every lesson.

## Course map

1. See the Rails request cycle
2. Persist the first private reflection
3. Add identity and revocable sessions
4. Enforce ownership and authorization
5. Model a relationship and consent
6. Make forms and errors humane
7. Add Turbo without hiding HTTP
8. Generate a deterministic daily reflection
9. Run jobs and notifications with Solid Queue
10. Integrate astrology behind a replaceable boundary
11. Test behavior, security, and failure
12. Operate, deploy, back up, and recover
13. Measure product learning without surveillance
14. Build the brand and content channel
15. Monetize and add AI without surrendering trust

## Clean-room product charter

Our Sky is an independent, open-source astrology and relationship app. Reproduce useful product mechanics from public behavior. Do not copy Co–Star’s code, private APIs, visual assets, trademarks, or authored horoscope text.

### Free product scope

- Personal natal chart: signs, houses, planets, aspects, and plain-language explanations.
- Daily reading: headline, explanation, three Dos, three Don’ts, life areas, and source/provenance.
- People: privately add partners, friends, or crushes with consent-aware sharing.
- Compatibility: communication, emotional connection, attraction, friction, and long-term themes.
- Notifications: the daily reading plus meaningful transit events; no engagement spam.
- Ask Our Sky: bounded questions answered from the user’s stored chart and current transits.
- Relationship guidance: shared prompts and everyday suggestions derived from both charts.

### Product advantages

- Explainability: show which placements and transits informed a reading.
- Privacy: local export, deletion, visibility controls, and no sale of birth or location data.
- Uncertainty: exact, approximate, and unknown birth time are explicit.
- Shared space: private reflections, rituals, and check-ins without turning intimacy into a feed.
- Open algorithms: chart calculations, scoring rules, prompt versions, and limitations are inspectable.

### Product promise

Give a person something useful in under a minute. Help them understand themselves and someone they care about. Never claim certainty where the inputs or interpretation are uncertain.
