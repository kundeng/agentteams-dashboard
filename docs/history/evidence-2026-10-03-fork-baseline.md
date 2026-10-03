# Fork baseline evidence — 2026-10-03

## Repository boundary

- Upstream: `https://github.com/agentteams-group/agentteams-dashboard`
- Fork: `https://github.com/kundeng/agentteams-dashboard`
- Branch: `bayes/dashboard-i18n-v1.2.4.9`
- Baseline: tag `v1.2.4.9`, commit `45395a4`
- Related platform fork: `https://github.com/kundeng/AgentTeams`, branch
  `bayes/agentteams-v1.2.4`
- Omniteam: separate later enterprise control-plane product; not an owner of this customization

## Localization inventory

At the pinned baseline, `package.json` and the source tree contain no recognized application i18n
library, provider, message catalog, or translation hook. `rg` found Han characters in 389
TypeScript/JavaScript source files and 6,032 matching source lines, including 115 test files. These
figures include comments and fixtures and therefore measure migration scope, not visible strings.

## License finding

The upstream README says to refer to a license file in the repository root. No `LICENSE`, `COPYING`,
or `NOTICE` file exists at the pinned tag, and GitHub reports no detected license. Do not repeat the
earlier unsupported Apache-2.0 claim for this repository. Distribution of a derived image remains
gated on explicit license clarification.

## Baseline verification

- `npm ci`: completed; npm reported 27 inherited audit findings (1 low, 9 moderate, 17 high).
- `npm run lint`: completed with 0 errors and 6 inherited warnings.
- `npm run typecheck`: passed.
- `npm test`: 213 files and 2,011 tests passed.
- `python3 scripts/spec_lint.py`: 2 specs passed.

The audit findings and lint warnings predate this fork documentation and are not silently rewritten
during repository-boundary cleanup. They require a dedicated dependency/security review before the
fork is treated as a production UI.
