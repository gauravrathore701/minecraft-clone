# minecraft-clone — Joystick Inversion Fix + More Trees

**Date:** 2026-06-11

## What Was Done

1. **Fixed inverted movement** — the velocity z-component in `physics()` was missing a
   negation (`vel.z = -(mx*sin + mz*cos)`). Forward/back was mirrored (joystick down moved
   forward, W also moved backward), and strafing skewed once the camera turned. Single sign
   fix corrects joystick *and* keyboard movement.
2. **Doubled tree density** — spawn threshold `hash2(...) > 0.985` → `> 0.965`
   (~1.5% → ~3.5% of eligible grass blocks).

No service restart needed — static files are read from disk per request. Verified the
live site serves the updated file.
