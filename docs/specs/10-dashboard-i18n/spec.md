---
spec_id: 10-dashboard-i18n
status: DRAFT
closed_as: null
since: 2026-10-03
until: null
epic: owner-experience
features: [dashboard-i18n, english-default-dashboard]
supersedes: []
superseded_by: null
depends_on: []
anchors: [docs/design/10-localization-architecture.md, docs/design/20-owner-control-surface.md]
---

# 10 — Dashboard internationalization and English fork

## Goal

Create a reversible AgentTeams Dashboard fork whose default operator experience is
complete English and whose UI text is backed by a real locale catalog rather than hard-coded
Chinese/English strings.

## Requirements

- **R1:** The fork SHALL preserve upstream attribution and SHALL NOT publish a derived image until
  the missing upstream license file or other applicable license grant is resolved and recorded.
- **R2:** English SHALL be the default locale and every owner-facing navigation, onboarding,
  lifecycle, error, empty-state, and confirmation string SHALL come from a message catalog.
- **R3:** The architecture SHALL support adding another locale without editing feature components.
- **R4:** The Owner SHALL be able to select a locale and retain it across authenticated sessions.
- **R5:** The fork SHALL run beside upstream on its own container, port, session secret, and data
  volume while sharing only the intended Controller and Matrix backends.
- **R6:** Automated checks SHALL detect untranslated keys, accidental source-language literals, and
  catalog drift; screenshot journeys SHALL cover every sidebar section and primary lifecycle.
- **R7:** Deployment SHALL be reversible without mutating upstream Dashboard state.

## Design constraints

- Do not begin by mechanically translating components. First inventory strings, error sources,
  runtime-provided labels, formatting rules, and locale-sensitive dates/numbers.
- Baseline evidence is Dashboard tag `v1.2.4.9` at `45395a4`. It contains 389 TypeScript/JavaScript
  source files and 6,032 matching source lines with Han characters, including 115 test files, and no
  recognized application i18n dependency or catalog.
- Keep upstream Dashboard available at `127.0.0.1:13000`; target the fork at
  `127.0.0.1:13001` and a separate tailnet-only HTTPS listener.
- Use stable semantic message keys. Do not use the English sentence itself as the key.
- Treat controller/runtime messages not owned by the Dashboard as a separate translation boundary.
- Preserve login, RBAC, Matrix chat, Worker/Manager/Team lifecycle, Task Board, Artifacts, Audit,
  Monitor, diagnostics, and logout behavior.

## Tasks

- [ ] 1. Inventory every owner-visible literal and its source boundary.
- [ ] 2. Choose and document the catalog/runtime approach and fallback rules.
- [ ] 3. Create the private fork and reproducible immutable image build.
- [ ] 4. Extract strings and implement English as the complete default catalog.
- [ ] 5. Add locale selection, persistence, and missing-key diagnostics.
- [ ] 6. Deploy the parallel stack on a distinct port, secret, and volume.
- [ ] 7. Run side-by-side functional and screenshot acceptance across all sidebars.
- [ ] 8. Publish upgrade, rollback, and upstream-sync instructions.

## Verification

- Catalog completeness and source-literal lint pass.
- Component/unit tests pass.
- Upstream and fork can be open simultaneously against the same test resources.
- Screenshot matrix shows complete English for each navigation and lifecycle state.
- Removing the fork restores the upstream-only deployment without data loss.

## Log

- 2026-10-03: Relocated from the deployment-plumbing repository before implementation began. The
  deployed Dashboard has no application-level i18n dependency or locale catalog; this is a product
  change, not a hidden settings toggle. The upstream README refers to a root license file, but tag
  `v1.2.4.9` contains none; distribution remains gated on license clarification.
