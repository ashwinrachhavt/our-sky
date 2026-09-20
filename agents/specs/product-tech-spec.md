# Our Sky: Product and Technical Specification

**Status:** Discovery draft

**Last updated:** 2026-09-20

**Decision status legend:**

- **Proposed:** a documented default that requires explicit product-owner confirmation.
- **Accepted:** explicitly confirmed and governing for implementation.
- **Open:** cannot be responsibly decided from the current evidence.

## 1. Product summary

Our Sky is an independent astrology and relationship-reflection product. It gives a person a fast, personalized daily insight, explains the astrological inputs and uncertainty behind it, and can later help them reflect on a relationship with someone they care about.

The product is inspired by publicly observable engagement mechanics in consumer astrology products, but it is a clean-room project: it must not copy Co–Star’s proprietary code, private APIs, visual assets, trademark, or authored horoscope text.

## 2. Product promise

> Give a person something useful in under a minute. Help them understand themselves and someone they care about. Never claim certainty where the inputs or interpretation are uncertain.

## 3. Goals

1. Deliver an immediately useful daily experience that feels personal without misrepresenting certainty.
2. Build trust through explainability, privacy, consent, and durable user control.
3. Learn the full ownership path of a production Rails application through small vertical slices.
4. Keep astrology and AI providers replaceable so that outages or vendor changes do not compromise core user data or core flows.

## 4. Non-goals for the first product slice

- A public social feed, follower graph, or virality-first engagement model.
- Relationship scoring presented as objective fact.
- AI-generated chart calculations or fabricated astrological claims.
- Billing, subscriptions, notifications, native apps, or a separate JavaScript frontend before the core private-reflection slice works.
- A large schema designed for hypothetical future features.

## 5. Users and open product decisions

| Question | Current status | Decision needed |
| --- | --- | --- |
| Primary launch segment | Open | Define the first user and their immediate alternative. |
| Core under-one-minute outcome | Open | Define the emotion and concrete value delivered on first open. |
| Relationship v1 boundary | Open | Choose solo only, one-way private records, mutual connections, or a shared space. |
| Astrology posture | Proposed hybrid | Use rigorous/inspectable calculations with accessible, non-deterministic reflection language. |
| Open-source posture | Open | Choose public from day one, delayed open source, or an open-core boundary. |
| Monetization boundary | Open | Decide when premium depth is relevant; do not gate initial value. |

## 6. Core experience hypothesis

A user opens Our Sky, receives a concise daily insight, sees limited but complete actionable guidance, and can choose to explore the explanation behind it. The experience should be useful even when no AI or external astrology provider is available.

### Candidate daily-reading contract

A daily reading may contain:

- Headline
- Short explanation
- Three Dos
- Three Don’ts
- Relevant life areas
- Input provenance: chart placements, transits, ruleset and prompt version
- Uncertainty statement tied to birth-data precision

This contract is **proposed**, not accepted.

## 7. Product requirements

### 7.1 Privacy and user control

- Private data is private by default.
- A user can export their data and request account deletion.
- The product does not sell birth, relationship, or location data.
- Birth-time precision is stored and displayed as `exact`, `approximate`, or `unknown`.
- No relationship information is shared with another account without explicit consent and visibility rules.

### 7.2 Explainability and uncertainty

- Every persisted reading records the inputs and versioned logic that produced it.
- The UI can distinguish calculated astronomical data from interpretive content.
- The UI must not present approximate or unknown birth data as precise.
- Copy must avoid medical, financial, legal, or deterministic relationship claims.

### 7.3 Reliability

- A provider timeout cannot make private reflections, authentication, or already-persisted readings unavailable.
- Repeated generation requests must be safe to retry and not create duplicate daily readings.
- Background work is observable, retryable where appropriate, and idempotent.

## 8. Technical architecture

### 8.1 Accepted architecture defaults

- **Application shape:** Rails 8.1 modular monolith.
- **Rendering:** server-rendered HTML using ERB and Hotwire.
- **Primary database:** PostgreSQL.
- **Async execution:** Solid Queue.
- **Authentication:** Rails-native session-cookie flow with a durable, revocable session record.
- **Tests:** Minitest initially.

### 8.2 System boundaries

```text
Browser
  -> Rails routes/controllers
  -> application/domain layer
  -> PostgreSQL (durable truth)
  -> Solid Queue workers (scheduled and retryable work)
  -> replaceable adapters: astrology calculation/provider, AI provider, notification provider
```

Rails owns authorization, HTML rendering, domain rules, jobs, and durable state. External providers are inputs behind interfaces; they must not become the source of truth for identity, user-owned content, authorization, consent, or persisted reading provenance.

### 8.3 Initial data model

The first implementation slice has three models only:

| Model | Responsibility | Essential invariant |
| --- | --- | --- |
| `User` | Identity and ownership | User-owned records reference a valid user. |
| `Session` | Revocable authenticated session | Revoking a session prevents future authenticated use of that credential. |
| `Reflection` | Private durable user content | A user can read and modify only their own reflection. |

Do not create `Relationship`, `Person`, `BirthProfile`, `Chart`, `Transit`, `Reading`, `Subscription`, or `Notification` tables until a planned vertical slice requires their behavior.

### 8.4 First vertical slice

**User story:** As a signed-in user, I can create a private reflection, view it later, and know that other users cannot read it.

**Acceptance criteria:**

1. An unauthenticated request to reflections redirects or returns an authentication failure as appropriate.
2. A signed-in user can create and view their own reflection.
3. A signed-in user cannot retrieve, edit, or delete another user’s reflection by changing an identifier.
4. The database uses foreign keys and `NOT NULL` constraints where required to protect ownership.
5. Tests cover the success path and cross-user authorization failure.
6. Logs and browser/network inspection make the request path explainable.

## 9. Future slices and dependencies

| Slice | Introduces | Why it comes later |
| --- | --- | --- |
| Identity and revocable sessions | `Session` behavior | Protects user data before richer features. |
| Daily reflection | Deterministic generation and scheduling | Requires an idempotent persistence contract. |
| Birth profile and chart | Birth-data precision and calculation adapter | Requires explicit uncertainty UX and validation. |
| Daily reading | `Reading`, provenance, life areas | Requires deterministic inputs and versioned interpretation. |
| Relationships | Relationship/consent model | Requires a confirmed product boundary and privacy rules. |
| Compatibility | Cross-chart computation and explanation | Requires relationship visibility and consent policies. |
| Notifications | Preferences, delivery adapter, jobs | Requires demonstrated recurring value. |
| Monetization | Entitlements and billing integration | Requires validated free value and a chosen premium boundary. |
| AI assistance | Bounded AI adapter and evaluation | Requires stable deterministic product inputs and safety rules. |

## 10. Operational requirements

- Every background job has a named purpose, retry policy, idempotency strategy, and operational signal.
- Every migration includes a rollback and deployment-risk consideration.
- Backups and restoration are practiced before calling the product production-ready.
- Secrets live outside version control.
- Product analytics measure learning and value without surveillance or selling sensitive data.

## 11. Decision log

| ID | Decision | Status | Rationale | Revisit trigger |
| --- | --- | --- | --- | --- |
| ARC-001 | Rails-first modular monolith | Proposed | Optimizes for coherent full-stack ownership and avoids premature split-stack complexity. | Multiple independently deployed clients or a measured team/product need. |
| ARC-002 | PostgreSQL as durable truth | Proposed | Strong relational integrity and a natural fit for Rails domain data. | Material operational or product constraint. |
| ARC-003 | ERB + Hotwire as first UI | Proposed | Delivers responsive browser interactions without maintaining a second client application. | Evidence that user needs cannot be met acceptably. |
| ARC-004 | Solid Queue for jobs | Proposed | Keeps early asynchronous work within the Rails stack. | Operational limits demonstrated in production. |
| PRD-001 | Under-one-minute useful insight | Proposed | Supports immediate perceived value and retention without withholding the core experience. | User research falsifies the hypothesis. |
| PRD-002 | Explainability, privacy, consent, and uncertainty | Proposed | These are essential trust constraints for sensitive personal and relationship data. | Never; only implementation details may change. |

## 12. Open questions for discovery

1. Who is the first target user, and what alternative are they replacing?
2. What precise outcome should a first-time user experience in under one minute?
3. Is v1 solo-only, one-way private compatibility, mutual connection, or a shared workspace?
4. Is the product traditional-astrology-first, reflection-first, or a hybrid?
5. What exactly is public under the open-source posture, and when?
6. What data is required for useful personalization, and what is the minimum consentful collection flow?
7. What is the chosen source or implementation strategy for astronomical/chart calculations?
8. What reading components must be deterministic, and where—if anywhere—may AI transform an explanation?
9. What are the safety and copy constraints for relationship guidance?
10. What evidence would justify a paid tier, and what should remain free forever?

## 13. Repository next steps

1. Confirm the product decisions in Section 12 before modeling the astrology domain.
2. Decide whether this repository should contain a fresh Rails 8.1 application at the root or preserve a prior scaffold on a tagged branch.
3. Create `ENGINEERING_LOG.md` before the first application change.
4. Implement the private-reflection vertical slice before adding astrology, AI, relationship, billing, or notification infrastructure.
