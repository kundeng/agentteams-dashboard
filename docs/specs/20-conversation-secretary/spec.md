---
spec_id: 20-conversation-secretary
status: DRAFT
closed_as: null
since: 2026-10-03
until: null
epic: owner-experience
features: [conversation-brief, owner-secretary]
supersedes: []
superseded_by: null
depends_on: [10-dashboard-i18n]
anchors: [docs/design/20-owner-control-surface.md]
---

# 20 — Conversation Brief and owner Secretary

## Goal

Let one human operate an agent company without reading agent-speed transcripts. Every active room
gets a compact visual brief, and the company view aggregates decisions, blockers, evidence, and
owner-required actions across rooms.

## Requirements

- **R1:** Each active room SHALL show participants and roles, objective, phase/progress, decisions,
  blockers, waiting-on, next action, freshness, budget envelope, and evidence links.
- **R2:** Verified state SHALL come from a deterministic reducer over Matrix, Controller resources,
  tasks, artifacts, Git/test receipts, approvals, audit events, budgets, and heartbeats.
- **R3:** Generated narrative SHALL be labeled, cached, disposable, and unable to mutate factual
  status, ownership, approvals, evidence, or budgets.
- **R4:** The company overview SHALL expose `Needs you now`, `Blocked`, `In progress`, `Quiet but
  healthy`, `Done awaiting acceptance`, and `Degraded` lanes.
- **R5:** Worker identity, room continuity, compute/sandbox health, checkout persistence, restart
  policy, and last recovery proof SHALL appear as separate state.
- **R6:** Every summary claim SHALL link back to reconstructable source events or be explicitly
  marked as interpretation.

## Tasks

- [ ] 1. Define the versioned `conversation_brief` schema and source identifiers.
- [ ] 2. Build replayable event ingestion and deterministic state reduction.
- [ ] 3. Render the per-room brief above chat with drill-down to evidence.
- [ ] 4. Build the cross-room owner queue and company lanes.
- [ ] 5. Add the bounded Secretary narrative trigger and prompt contract.
- [ ] 6. Test replay, stale/missing signals, conflicting events, and hallucination containment.
- [ ] 7. Run an Owner journey across at least three concurrent rooms.

## Verification

- Replaying the same ordered events produces byte-equivalent factual brief state.
- Removing the LLM narrative leaves all operational facts and controls usable.
- Injected false conversational completion cannot override absent Git/test/task evidence.
- Owner journey identifies a blocker and required approval without opening raw chat.

## Log

- 2026-10-03: Created from the live Dashboard audit. Raw Matrix chat is useful for intervention but
  is not an adequate human control surface for an agent-speed company.
- 2026-10-03: Relocated into the Dashboard fork before implementation began; Omniteam remains a
  separate later enterprise product.
