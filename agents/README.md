# Agent Documentation

This directory is the durable context for human and AI contributors working on Our Sky.

## Structure

- `ideation/`: source product observations and product/engineering apprenticeship context migrated from Notion.
- `specs/`: decision-oriented specifications that govern implementation.

## Working rules

1. Treat files in `ideation/` as source material, not automatically approved product scope.
2. Record decisions, assumptions, trade-offs, and open questions in `specs/product-tech-spec.md`.
3. Preserve privacy, consent, uncertainty, and explainability as first-class product requirements.
4. Keep the first release narrow: ship complete vertical slices before adding models, dependencies, or separate services.
5. Do not copy Co–Star code, assets, trademarks, private APIs, or authored horoscope text.

## Current status

The repository contains documentation scaffolding only. The first implementation decision to validate is the Rails-first monolith repository shape before application code is generated.
