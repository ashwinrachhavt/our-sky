# Core Track: Rails-First Interactive Apprenticeship

> Source: internal Notion document, migrated on 2026-09-20.

## Learning contract

This is not a paste-along tutorial. Ashwin types the code, predicts what Rails will do, inspects the result, breaks it deliberately, explains it back, and commits only after exit criteria pass.

## Eight-step loop

1. Understand one Rails concept.
2. Predict observable behavior.
3. Type one small change.
4. Run one command.
5. Inspect routes, SQL, logs, HTML, or tests.
6. Break one assumption safely.
7. Explain the trade-off in plain language.
8. Commit a small, working slice.

## Four lenses

Each lesson includes:

- Rails fundamental: framework behavior that can be explained.
- Engineering judgment: invariant, failure mode, security boundary, and operational signal.
- Product management: user problem, hypothesis, smallest experiment, and evidence.
- Business and ethics: value, trust, incentives, brand consequence, and what not to manipulate.

## Current checkpoint: Lesson 00

### Decision

The learning product begins as one Rails 8.1 application with PostgreSQL, server-rendered HTML, ERB, Hotwire, session cookies, Minitest, and the default Solid stack. A separate frontend is added only when measured product or team constraints justify it.

### First real product slice

A signed-in person writes one private reflection, revisits it later, and cannot read another person’s reflection. This slice teaches request flow, persistence, identity, ownership, testing, privacy, and the product promise without requiring astrology, AI, billing, or a feed.

### Exit criteria

- Current Git state is understood and no work is accidentally lost.
- Any existing split-stack scaffold has a named preservation plan.
- The intended monolith location is explicit.
- Ashwin can explain why the course starts with one Rails application.
- No application code is generated before the repository transition is approved.
