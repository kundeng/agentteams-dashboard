<!-- BEGIN:nextjs-agent-rules -->

# This is NOT the Next.js you know

This version has breaking changes — APIs, conventions, and file structure may all differ from your training data. Read the relevant guide in `node_modules/next/dist/docs/` (resolved from this file's directory; in monorepos the `next` package may not be visible from the repo root) before writing any code. Heed deprecation notices.

This block is written and re-added by `next dev` — verify at `node_modules/next/dist/server/lib/generate-agent-files.js`. Removing it from a diff only re-creates the uncommitted change; committing it with your work keeps the tree clean.

<!-- END:nextjs-agent-rules -->

## Bayes fork orientation

This branch is the source-controlled customization of the Dashboard deployed with AgentTeams
`v1.2.4` on `bayes-pop`. It starts from Dashboard tag `v1.2.4.9`. AgentTeams platform code remains
in the separate `kundeng/AgentTeams` fork; the later Omniteam enterprise product is not part of this
repository.

specs_root: docs/specs

Work specs in numeric order with at most one ACTIVE sprint. Stable UI architecture belongs under
`docs/design/`; current operator/product behavior belongs in `docs/`; dated fork evidence belongs
under `docs/history/`. Do not deploy this branch over the upstream Dashboard in place: use a distinct
container, port, session secret, and data volume until side-by-side acceptance is complete.
