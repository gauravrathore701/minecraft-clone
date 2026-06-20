# minecraft-clone — Broken Hash Fix (terrain + trees) + Grid-Spaced Trees

**Date:** 2026-06-11

## Root Cause Found

`hash2()` multiplied 32-bit ints with plain `*`, overflowing JS float precision (>2^53).
The output range collapsed (never above ~0.5 for the tree call pattern). Consequences:

- **Zero trees ever spawned** — the `> 0.90` threshold was unreachable; earlier density
  tweaks (0.985 → 0.965 → 0.90) did nothing
- **Terrain was flattened** — heights spanned only 15–23 instead of 14–40; 71% of the
  world was underwater, spawn itself was in the sea (h=19 < SEA=20)

## Fix

1. Replaced `hash2` with `hashT` using `Math.imul` (murmur-style finalizer) — terrain
   noise now uses it too. World regenerates with proper hills/snow: 93% land, spawn at h=25
2. **Grid-spaced trees**: world divided into 10×10 cells; 85% of cells grow one tree at a
   deterministic jittered spot → ~10 block average spacing (min ~5), per user request
3. Tree pass runs over a 2-block chunk margin so canopies cross chunk borders seamlessly
   (old code skipped trees near borders entirely)

Verified via node simulation: 278 trees per 200×200 area (was 0), 16 trees near spawn.

## Note

Old localStorage edits were made against the flat world — they may appear floating/buried
in the new terrain. Clearing them: `localStorage.removeItem('cursedcraft_edits')`.
