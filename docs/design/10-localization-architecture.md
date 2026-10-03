# 10 — Localization architecture

## Ownership

The Dashboard owns browser-visible navigation, forms, validation hints, empty states, notifications,
and presentation formatting. AgentTeams Controller and runtime responses remain external input. The
UI must map stable machine codes to localized messages where codes exist and must display unknown
upstream prose honestly rather than pretending it was translated.

## Baseline

The fork starts from Dashboard `v1.2.4.9` (`45395a4`), matching the live `bayes-pop` deployment.
The baseline has no recognized i18n runtime or message catalog. A source inventory found Han
characters in 389 TypeScript/JavaScript files and 6,032 matching source lines; 115 affected files
are tests. Counts include comments and fixtures, so they define inventory scale rather than the
number of user-visible messages.

## Target boundary

```text
browser locale / saved preference
              |
              v
       locale resolver ------> typed message catalog
              |                        |
              +----> date/number formatting
              |
Dashboard components <---- stable semantic message keys
              |
              +----> upstream error adapter ----> known code -> localized copy
                                           `----> unknown prose -> labeled passthrough
```

English is the default and fallback locale. Locale selection is per authenticated Dashboard user
when identity is available and per browser otherwise. Missing keys fail tests and emit a development
diagnostic; production falls back to English without hiding the key from logs.

## Catalog rules

- Use stable semantic keys such as `teams.empty.noTeams`, never an English sentence as a key.
- Keep interpolation typed or mechanically checked; do not concatenate translated fragments.
- Use locale-aware date, time, relative-time, number, and plural formatting.
- Keep operational identifiers, runtime names, Matrix IDs, model aliases, and code literals
  untranslated.
- Translate accessibility names, tooltips, confirmation text, error and empty states, not only
  visible headings.
- Tests should query roles and stable behavior where possible; copy-specific assertions use the
  catalog so a locale addition does not duplicate component tests.

## Deployment boundary

The fork runs beside upstream with a different image name, container, host port, session secret,
and Dashboard data volume. Both may point at the same test Controller and Matrix backends. The fork
is not promoted until login, RBAC, chat, Workers, Managers, Teams, Humans, tasks, artifacts, models,
knowledge, audit, monitoring, diagnostics, and logout pass in English.

## License gate

The upstream README says to consult a root license file, but tag `v1.2.4.9` has no `LICENSE`,
`COPYING`, or `NOTICE` file and GitHub reports no detected license. Source inspection and internal
testing may proceed; publishing a derived image or offering the fork to others waits for an explicit
license grant or owner clarification.
