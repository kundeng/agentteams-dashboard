# Bayes AgentTeams Dashboard documentation

Read numbered documents in order. Numbers advance by ten so later material can be inserted without
renaming the library. Existing upstream documents remain available through [INDEX.md](INDEX.md).

## Design

1. [10 — Localization architecture](design/10-localization-architecture.md)
2. [20 — Owner control surface and Secretary](design/20-owner-control-surface.md)

## Delivery

1. [10 — Dashboard internationalization](specs/10-dashboard-i18n/spec.md) — DRAFT
2. [20 — Conversation Brief and Secretary](specs/20-conversation-secretary/spec.md) — DRAFT

## Evidence

- [Fork baseline and licensing gap](history/evidence-2026-10-03-fork-baseline.md)

The AgentTeams platform fork owns controller, runtime, installer, and image-wiring changes. This
repository owns Dashboard UI behavior. The deployment-plumbing repository owns `bayes-pop`
operation. Omniteam is a separate later enterprise product.
