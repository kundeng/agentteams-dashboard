# Owner control surface: onboarding and conversation briefs

## Problem

AgentTeams exposes resources and raw Matrix conversations, but the Owner must mentally join those
surfaces to understand company state. Room membership is a number, activity appears as low-level
tool cards, Manager task state does not necessarily appear on the task board, and blockers may be
buried in prose. Localization does not solve this comprehension problem.

The control surface must optimize for an Owner who intervenes rarely but remains accountable for
every consequential outcome.

## Owner questions

Every room and company overview must answer, without reading the full transcript:

1. What outcome is this room pursuing?
2. Who is present, what is each participant's role, and who is active now?
3. What work is pending, active, blocked, awaiting approval, or complete?
4. What meaningfully changed since the Owner last viewed the room?
5. Which decisions, assumptions, and approvals govern the work?
6. Which artifacts, Git diffs, tests, and deployment receipts support claims of progress?
7. What is the next action, who owns it, and when should the Owner care again?
8. What budget, model, tool, and permission envelope is in force?
9. Is each participant's identity, room, compute sandbox, and working state durable or replaceable?

## Conversation Brief

Pin a compact, expandable brief above each chat timeline:

```text
┌─ Product Demo · Version JSON ───────────────── ACTIVE · updated 2m ago ─┐
│ Goal      Add machine-readable version output                          │
│ People    Owner · Atlas (Manager) · Vega (Leader) · Dev-1 (working)   │
│ Progress  Spec ✓  Checkout ✓  Code 70%  Tests blocked  Review —       │
│ Blocker   Developer image lacks Python 3.12/uv                         │
│ Waiting   Infrastructure Owner: approve/reject toolchain image         │
│ Evidence  spec-01 · 2 files changed · tests not run · no commit        │
│ Next      Build governed dev image, then rerun focused tests           │
│ Budget    manager-route · 1 Worker · token/run ceiling 250k            │
└─────────────────────────────────────────────────────────────────────────┘
```

The brief must distinguish verified facts from generated interpretation and display freshness. It
must never infer completion solely from conversational confidence.

## Secretary architecture

Use one cross-room Secretary service for a single Owner, not one chatty agent per room.

```text
Matrix events ───────┐
Controller resources ├─> deterministic event reducer ─> room/company state
Task/Artifact records│                                 │
Git + test receipts ─┘                                 ├─> visual brief
Audit/approval events ─────────────────────────────────┤
                                                      └─> bounded LLM narrative
```

### Deterministic layer

The reducer owns facts and joins identifiers across Matrix rooms, Manager/Worker/Team resources,
Projects/Tasks, audit events, Git references, tests, artifacts, approvals, budgets, and heartbeats.
It computes participant presence, activity phase, blockers, outstanding approvals, terminal status,
freshness, and evidence links. Its output is a versioned `conversation_brief` record.

### LLM layer

The LLM may write `what changed`, `why it matters`, and `suggested next action` from the verified
record. It cannot change status, ownership, evidence, approvals, or budgets. Trigger it only on
meaningful state transitions, explicit Owner recap, or a bounded interval; cache the result.

### Secretary team successor

One Secretary is enough for one Owner. When multiple owners or many concurrent projects justify
separation of duties, split it into:

- **Chief of Staff:** priorities, summaries, handoffs, stale-work reminders;
- **Auditor:** permissions, approvals, evidence completeness, policy violations;
- **Project Secretary:** project-specific decisions, risks, and milestone narrative.

These roles share the same deterministic state and cannot privately invent project truth.

## Company overview

The landing page should aggregate briefs into lanes:

- **Needs you now:** approval, policy exception, irreversible action, ambiguous requirement;
- **Blocked:** blocker, owner, duration, attempted remedies;
- **In progress:** outcome, team, current phase, last evidence;
- **Quiet but healthy:** next scheduled check and budget;
- **Done awaiting acceptance:** diff, tests, review, artifact, rollback;
- **Degraded:** stale heartbeat, broken audit, missing task linkage, excessive loop/cost.

## Onboarding journey

The product needs a resumable readiness wizard before the first Goal can be assigned:

1. **Infrastructure:** health, persistence, tailnet ingress, backup, stop control.
2. **Models:** provider reachability, named Manager/Developer/Ops routes, caps and fallback.
3. **Owners:** explicit Human identities, RBAC, recovery credential, second-owner invitation later.
4. **Manager:** name, charter, language, decision rights, escalation and cost limits.
5. **Tools:** approved Skills/MCP registry, secret projection, denial defaults.
6. **Repository:** authoritative branch, checkout-transfer contract, write and publication gates.
7. **Worker templates:** runtime, developer toolchain, resources, sandbox and filesystem scope.
8. **Team:** separate Leader and Worker roles, Matrix-safe naming, communication topology.
9. **Observation:** conversation briefs, task/artifact linkage, audit and knowledge probes.
10. **Readiness journeys:** read-only, no-change checkout, documentation patch, then code patch.

Each step records pass/fail evidence and a remediation link. The **Start project** action remains
disabled until mandatory steps pass or the Owner records an explicit time-bounded exception.

## Minimum acceptance bar

- Every active chat has a fresh brief with participants, goal, progress, blocker, waiting-on, next
  action, and evidence.
- Every Worker tool run resolves to a structured task and an auditable permission envelope.
- Every completion claim links to Git/test/artifact evidence.
- Every owner-required action appears in one `Needs you now` queue.
- The Owner can stop one run, one Worker, one Team, or the workforce without deleting state.
- Worker cards separately show resource identity, room continuity, sandbox/container health,
  checkout persistence, restart/reconciliation policy, and last recoverability test.
- Summaries are reconstructable from underlying events; generated prose is labeled and disposable.
