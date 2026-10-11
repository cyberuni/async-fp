---
'@unional/async-context': patch
'@unional/gizmo': patch
---

Update `type-plus` to 8.0.0.

`@unional/gizmo` no longer loads `type-plus` at runtime, so its CommonJS build does not `require` the now ESM-only package.
