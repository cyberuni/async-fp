# @unional/gizmo

## 3.0.0

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

## 2.3.5

### Patch Changes

- c19caa2: Depend on `type-plus@^8.0.0-beta.10` — back on the 8.x line, as a range rather than an exact pin.
  
  The previous release moved these packages to `^7.6.2`. That changed the major line when the actual defect was the *shape* of the dependency, not the line: `8.0.0-beta.8` was pinned **exactly**, so it could never resolve forward to the upstream fix. `type-plus` 8.x is the line these packages are built against and the one their maintainer is actively developing, so they stay on it.
  
  `^8.0.0-beta.10` resolves to `>=8.0.0-beta.10 <9.0.0-0`, so the CJS packaging fix in `8.0.0-beta.10` — `cjs/package.json` = `{"type": "commonjs"}`, without which Node parses type-plus's CommonJS build as ESM and `require()` of these packages throws (#366) — and every later 8.x release flow in on their own.
  
  No source change: `8.0.0-beta.10`'s root barrel is byte-identical to `8.0.0-beta.8`'s, so every symbol these packages use (`AnyFunction`, `KeyTypes`, `LeftJoin`, `RequiredKeys`, `assertType`, `isType`, `testType`) resolves exactly as before, with no local shims. `require()` of both built packages is verified against the resolved `8.0.0-beta.10` tree.

## 2.3.4

### Patch Changes

- fc6d022: Depend on `type-plus@^7.6.2` instead of pinning `8.0.0-beta.8` exactly.
  
  Fixes `require()` of these packages (#366). `type-plus@8.0.0-beta.8` is `"type": "module"` and ships `cjs/` without a `{"type":"commonjs"}` marker, so Node parses its CommonJS output as ESM and the entry throws. The exact pin meant consumers could not resolve forward to the upstream fix in `8.0.0-beta.10`. `type-plus@latest` is `7.6.2`, which ships the `cjs/package.json` marker correctly; whether to move to the 8.x line stays a separate decision.
  
  Type-level only — the emitted `.js`, `.d.ts` and source maps are byte-identical under both versions, and every symbol used (`AnyFunction`, `KeyTypes`, `LeftJoin`, `RequiredKeys`, `assertType`, `isType`, `testType`) resolves from the 7.6.2 root barrel unchanged.

## 2.3.3

### Patch Changes

- defa0ac: Build with tsdown instead of `tsc` + `buddy ts build cjs`.
  
  Every path named in `exports` keeps its location and contents. Two differences in the
  tarball are worth naming: modules that are pure re-exports (`index`, `testing`,
  `gizmo_testing`) no longer ship `.js.map` / `.d.ts.map`, and the type-only `types`
  module no longer emits an empty `types.js`. `@unional/async-context` also gains small
  `_virtual/` runtime helper files, which is how the ES2020 target now downlevels its
  private class fields.

## 2.3.2

### Patch Changes

- cf5324f: Point repository metadata at `cyberuni/async-fp` following the org transfer, and release
  through GitHub OIDC / npm trusted publishing instead of a stored `NPM_TOKEN`.
  
  No runtime change.

## 2.3.1

### Patch Changes

- 210192b: Fix TypeScript 6 compatibility when `create()` returns an array of result objects (or a promise of that array). Introduces `GizmoResult` and updates `InferGizmo` so those shapes type-check and infer merged record types correctly.

## 2.3.0

### Minor Changes

- 1f02393: Remove `DefineContext` and `UnionToIntersection` from exported types.

## 2.2.0

### Minor Changes

- e2da176: Supports defining gizmo with sync `create()`.

## 2.1.0

### Minor Changes

- 93e47d8: Support `cleanup()` of _gizmo_.

### Patch Changes

- 4dd0a08: Update `type-plus`

## 2.0.2

### Patch Changes

- 6ecc0e7: Add `type-plus` as dependency.

  It was mistakenly added as devDependency.

## 2.0.1

### Patch Changes

- ae98148: Remove `ctx` from `GizmoBase`. The ctx is empty.

## 2.0.0

### Major Changes

- 58c52cc: Change `incubate` to take a base object instead of a gizmo.
  This change allows a gizmo to create other gizmos within the `create()` method.
- 3ea4879: Remove `ctx.with()`

## 1.3.0

### Minor Changes

- 2e545ae: Add `incubate.Infer<I>`

## 1.2.0

### Minor Changes

- 8996c07: `.create()` now accepts a `start` function to perform initialization.

  ```ts
  incubate().with(...).with(...).create(gizmo => {
    // initialization
  })
  ```

- 0c4e97a: Adds an `.init()` function to perform initialization before the gizmo is created.

  This optional function allows you to perform some initialization before the gizmo is created.

  This is useful if you are exposing the incubator directly to the outside world,
  which you can use this function to perform some initialization when the gizmo is created.
  The incubator caller can still pass in their own start function to the `create()` function, to perform additional initialization specific to the caller.

  When this function is called, the gizmo is considered to be final.
  The `.with()` function will be removed from the incubator.

  ```ts
  const incubator = incubate().with(...).init(g => { /* initialize */ })

  // incubator.with() is not available
  const gizmo = await incubator.create()
  ```

- 80f5dc5: Add `incubate().merge()` function to merge _gizmo instance_ into the _incubator_.

  This allows consumers to compose _gizmos_ when they only have access to the resulting _gizmo instances_ or just plain objects.

  ```ts
  import { define } from '@unional/gizmo'

  export async function activate({ mic }) {
  	const miku = await incubate().merge(mic).with(mikuGizmo).create()
  }
  ```

### Patch Changes

- 3b6c56b: Expose `/testing` for CJS usage

## 1.1.1

### Patch Changes

- 5d4041f: Remove the `with` method from resulting gizmo

## 1.1.0

### Minor Changes

- 5332cfc: Allow gizmo to compose from other gizmos through `ctx.with()`.

## 1.0.4

### Patch Changes

- 9a28ca3: Fix `required()` -> `require()`.

  The original fix only did that at the top level.

## 1.0.3

### Patch Changes

- 0e2b1ea: Add additional doc
- b63ae8b: Allow `start()` function to be sync.

## 1.0.2

### Patch Changes

- 2c26a4a: Add testing exports

## 1.0.1

### Patch Changes

- 228a724: Add `DynamicLoader<T>` type.

  This is added so that when the `ctx` needs to be specified manually,
  This type can be used for the dynamic dependency.

- 9abf3c1: Export more types.
  To fix the type cannot be named issue.

## 1.0.0

### Major Changes

- d0a68ba: Initial release.

  In this release, the basic functionality are working,
  except that dynamic dependencies are treated as static dependencies.

  This means the required dynamic dependencies need to be provided before adding the dependent gizmo.
