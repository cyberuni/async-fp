# Change Log

## 10.0.0

### Major Changes

- b5e379e: Pin `type-plus` to `8.0.0-beta.10`, exactly, and raise the Node floor to `>= 20`.
  
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

### Patch Changes

- Updated dependencies [b5e379e]
  - @unional/async-context@10.0.0
  - @unional/gizmo@3.0.0

## 9.0.17

### Patch Changes

- Updated dependencies [c19caa2]
  - @unional/async-context@9.0.17
  - @unional/gizmo@2.3.5

## 9.0.16

### Patch Changes

- Updated dependencies [fc6d022]
  - @unional/async-context@9.0.16
  - @unional/gizmo@2.3.4

## 9.0.15

### Patch Changes

- defa0ac: Build with tsdown instead of `tsc` + `buddy ts build cjs`.
  
  Every path named in `exports` keeps its location and contents. Two differences in the
  tarball are worth naming: modules that are pure re-exports (`index`, `testing`,
  `gizmo_testing`) no longer ship `.js.map` / `.d.ts.map`, and the type-only `types`
  module no longer emits an empty `types.js`. `@unional/async-context` also gains small
  `_virtual/` runtime helper files, which is how the ES2020 target now downlevels its
  private class fields.
- Updated dependencies [defa0ac]
  - @unional/async-context@9.0.15
  - @unional/gizmo@2.3.3

## 9.0.14

### Patch Changes

- cf5324f: Point repository metadata at `cyberuni/async-fp` following the org transfer, and release
  through GitHub OIDC / npm trusted publishing instead of a stored `NPM_TOKEN`.
  
  No runtime change.
- Updated dependencies [cf5324f]
  - @unional/async-context@9.0.14
  - @unional/gizmo@2.3.2

## 9.0.13

### Patch Changes

- Updated dependencies [e2da176]
  - @unional/gizmo@2.2.0
  - @unional/async-context@9.0.13

## 9.0.12

### Patch Changes

- Updated dependencies [93e47d8]
- Updated dependencies [4dd0a08]
  - @unional/gizmo@2.1.0
  - @unional/async-context@9.0.12

## 9.0.11

### Patch Changes

- Updated dependencies [6ecc0e7]
- Updated dependencies [6ecc0e7]
  - @unional/gizmo@2.0.2
  - @unional/async-context@9.0.11

## 9.0.10

### Patch Changes

- Updated dependencies [ae98148]
  - @unional/gizmo@2.0.1
  - @unional/async-context@9.0.10

## 9.0.9

### Patch Changes

- Updated dependencies [58c52cc]
- Updated dependencies [3ea4879]
  - @unional/gizmo@2.0.0
  - @unional/async-context@9.0.9

## 9.0.8

### Patch Changes

- Updated dependencies [2e545ae]
  - @unional/gizmo@1.3.0
  - @unional/async-context@9.0.8

## 9.0.7

### Patch Changes

- Updated dependencies [8996c07]
- Updated dependencies [0c4e97a]
- Updated dependencies [3b6c56b]
- Updated dependencies [80f5dc5]
  - @unional/gizmo@1.2.0
  - @unional/async-context@9.0.7

## 9.0.6

### Patch Changes

- Updated dependencies [5d4041f]
  - @unional/gizmo@1.1.1
  - @unional/async-context@9.0.6

## 9.0.5

### Patch Changes

- Updated dependencies [5332cfc]
  - @unional/gizmo@1.1.0
  - @unional/async-context@9.0.5

## 9.0.4

### Patch Changes

- Updated dependencies [9a28ca3]
  - @unional/gizmo@1.0.4
  - @unional/async-context@9.0.4

## 9.0.3

### Patch Changes

- Updated dependencies [0e2b1ea]
- Updated dependencies [b63ae8b]
  - @unional/gizmo@1.0.3
  - @unional/async-context@9.0.3

## 9.0.2

### Patch Changes

- a2dca4a: expose `/gizmo/testing`
- Updated dependencies [2c26a4a]
  - @unional/gizmo@1.0.2
  - @unional/async-context@9.0.2

## 9.0.1

### Patch Changes

- Updated dependencies [228a724]
- Updated dependencies [9abf3c1]
  - @unional/gizmo@1.0.1
  - @unional/async-context@9.0.1

## 9.0.0

### Major Changes

- d0a68ba: Release as Node ESM module

### Minor Changes

- d0a68ba: Add `@unional/gizmo`

### Patch Changes

- Updated dependencies [d0a68ba]
- Updated dependencies [d0a68ba]
  - @unional/async-context@9.0.0
  - @unional/gizmo@1.0.0

## 8.1.1

### Patch Changes

- f6ee884: Fix support of async asserter
- Updated dependencies [41af4c1]
  - @unional/async-context@8.1.1

## 8.1.0

### Minor Changes

- dfdb419: Add `asyncAssert()`.

### Patch Changes

- @unional/async-context@8.1.0

## 8.0.3

### Patch Changes

- Updated dependencies [593f104]
  - @unional/async-context@8.0.3

## 8.0.2

### Patch Changes

- Updated dependencies [5ac78c2]
  - @unional/async-context@8.0.2

## 8.0.1

### Patch Changes

- 51517d9: Fix `workspace:*` -> `workspace:^` reference
- Updated dependencies [9c1ecfb]
  - @unional/async-context@8.0.1

## 8.0.0

### Major Changes

- 189343b: Restore the `extend()` behavior in 2.0,
  that it returns a new instance instead of itself.

  The `clone()` function is also removed as it is not necessary anymore.

  The behavior introduced in 3.0 makes the async context mutable.
  Which `clone()` was added to mitigate that.

  However, such design is architecturally unsound.

  With this change, make sure you are updating or assigning the new instance after `extend()`:

  ```ts
  // from
  const ctx = new AsyncContext()

  ctx.extend(...)

  // to
  const ctx = new AsyncContext()

  const extended = ctx.extend(...)
  ```

### Patch Changes

- Updated dependencies [189343b]
  - @unional/async-context@8.0.0

## 7.0.4

### Patch Changes

- Updated dependencies [66b9fc8]
  - @unional/async-context@3.2.0

## 7.0.3

### Patch Changes

- Updated dependencies [1091796]
  - @unional/async-context@3.1.1

## 7.0.2

### Patch Changes

- Updated dependencies [0e14e94]
  - @unional/async-context@3.1.0

## 7.0.1

### Patch Changes

- Updated dependencies [ba8202e]
  - @unional/async-context@3.0.1

## 7.0.0

### Major Changes

- ce259fe: The generic types of `AsyncContext` have changed.
  Instead of specifying the `Context`, you specify the `Init` value.

  Through `.extend()`, additional `Context` will be added to it to produce the final result.

  Signature of the `transformer` in `.extend()` have also changed.
  Instead of receiving a `AsyncContext<Context>`, you receive the `Context` itself,
  in which you return an additional context that augments or add to it.

  `.get()` can override the context type returned.
  This is useful when you extend the context out-of-band,
  and you are not reassigning the context to update the type,
  or when the creating code do not know how it will be extended.

### Patch Changes

- Updated dependencies [ce259fe]
  - @unional/async-context@3.0.0

## 6.0.0

### Major Changes

- a640275: Refactor and Rewrite

  `AsyncContext` has been moved to `@unional/async-context` and is re-written to simplify its API.

  `async-fp` will remain a collection of utility libraries including `AsyncContext`.

### Patch Changes

- f9d2494: Update documentation
- Updated dependencies [a640275]
- Updated dependencies [b564c85]
- Updated dependencies [f9d2494]
- Updated dependencies [d9637ab]
  - @unional/async-context@2.0.0

All notable changes to this project will be documented in this file.
See [Conventional Commits](https://conventionalcommits.org) for commit guidelines.

## 5.0.3 (2020-09-07)

**Note:** Version bump only for package async-fp

## 5.0.2 (2020-03-24)

**Note:** Version bump only for package async-fp

## 5.0.1 (2020-03-15)

**Note:** Version bump only for package async-fp
