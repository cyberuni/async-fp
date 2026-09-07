---
'@unional/async-context': major
'async-fp': major
'@unional/gizmo': major
---

Pin `type-plus` to `8.0.0-beta.10`, exactly, and raise the Node floor to `>= 20`.

All three packages already declared `type-plus: ^8.0.0-beta.10` in their last publish
(`@unional/async-context@9.0.17`, `async-fp@9.0.17`, `@unional/gizmo@2.3.5`), so the
`typescript >= 5.6.0` peer type-plus 8 introduced already reaches consumers today.
Tightening the caret to an exact pin removes headroom (later 8.0.0 prereleases, `8.0.0`,
`8.1.0`) but does not change what a fresh install resolves right now — 8 is a prerelease
line where breaking changes land between betas (beta.10 -> beta.11 changed `Equal`'s
signature and removed `isType.f`), so an exact version makes the next bump a reviewable
PR instead of something a lockfile refresh does silently.

**Breaking: the Node floor moves from `>= 14.16` to `>= 20`.** `type-plus` 8 depends on
`unpartial@^1.0.7`, which declares `engines: { node: '>= 20' }`. `@unional/async-context`
and `@unional/gizmo` carry `type-plus` as a runtime `dependency` (its types are re-exported
in their emitted `.d.ts`), so their actual runtime floor was already 20 as of their last
publish — this only makes the declared `engines` field honest. `async-fp` has no direct
runtime dependency on `type-plus` (it's a devDependency there, and none of its own emitted
declarations reference it), but it depends on `@unional/async-context` and `@unional/gizmo`
at runtime, so its effective floor moves too. Because narrowing supported Node is breaking
for consumers regardless of which path pulls it in, all three packages take `major`.

Downstream value: `@unional/gizmo@2.3.0` was found, during a sibling sweep of
`justland/just-web-react`, published on an old `type-plus`, contributing a stale copy to
consumers' dependency trees. A fresh `@unional/gizmo` release from this repo fixes that at
the source for `mocktomata` and `justland/just-web` (via `@unional/gizmo`), both of which
consume this repo mid-graph.

`assertron` moves `11.5.3` -> `11.6.0` and `iso-error`/`satisfier`/`tersify` pick up their
latest patches as a side effect of exempting first-party packages from the 24h release-age
soak (see `pnpm-workspace.yaml`) — without the exemption, `assertron@11.5.2` (declaring the
older `type-plus: ^7.6.2` / `tersify: ^3.12.1`) would have been silently resolved instead,
duplicating both packages in the tree.
