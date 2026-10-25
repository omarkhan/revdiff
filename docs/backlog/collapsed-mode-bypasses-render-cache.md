---
worth: maybe
where: app/ui/collapsed.go:renderCollapsedDiff
added: 2026-10-25
---
# collapsed mode bypasses the per-line render cache

`renderDiff` returns `renderCollapsedDiff()` before it reaches the `renderCache` loop, and
`renderCollapsedDiff` rebuilds every visible diff line's styled block from scratch on every call. So the memoization
that brought expanded-mode renders down to ~4.5ms at 10k lines does nothing in collapsed mode: each
cursor move, search step or annotation keystroke there pays a full cold render.

A scratch benchmark during the #334 review (M1 Ultra, `benchModel` at 160x44) measured a collapsed render
at 3.3ms for 500 lines, 12.9ms for 2k, 59ms for 10k and 248ms for 50k, about the cost of an uncached
expanded render. No user has reported collapsed-mode lag.

`maybe` because the fix is not mechanical: collapsed rows depend on hunk expansion state, the modified-set
classification and the hidden-line rules, so the cache key and invalidation would need those inputs. The
cache-equivalence matrix in `TestModel_RenderDiffCacheMatchesColdRender` has one collapsed state but no
collapsed-to-collapsed transitions varying hunk expansion or other rendering inputs.
