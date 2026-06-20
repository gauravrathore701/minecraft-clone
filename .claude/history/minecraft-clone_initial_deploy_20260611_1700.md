# minecraft-clone — Initial Build & Deploy

**Date:** 2026-06-11

## What Was Done

Built "CursedCraft", a browser-based Minecraft clone, and deployed it at
**https://minecraft.cursedshrine.com**.

### Game (`public/index.html`, single file)
- Three.js (v0.160, CDN importmap) voxel engine — all rendering is client-side; the Pi only serves static files
- Chunked world: 16×64×16 chunks, render distance 4 (desktop) / 3 (mobile)
- Procedural terrain: 4-octave value noise, seeded (1337) — grass, dirt, stone, sand, snow peaks, water at sea level, deterministic trees
- Per-chunk meshing with hidden-face culling, vertex-color shading (no textures = fast on mobile)
- Block break/place with DDA raycast (6-block reach) + wireframe highlight
- AABB physics: gravity, jump, swimming (slower + buoyancy in water), void respawn
- 9-slot hotbar: grass, dirt, stone, sand, wood, leaf, plank, brick, snow
- Edits persist in localStorage (`cursedcraft_edits`), reapplied on chunk gen
- **Desktop:** WASD + pointer-lock mouse, LMB break / RMB place, 1–9 hotbar
- **Mobile:** virtual joystick (move), drag-to-look, ⬆ jump / ⛏ break / 🧱 place buttons, tap hotbar; reduced pixel ratio + render distance for perf

### Infra
- systemd: `minecraft-clone.service` — `python3 -m http.server 4175 --bind 127.0.0.1 --directory .../public`, enabled
- Tunnel: added `minecraft.cursedshrine.com → http://localhost:4175` to `~/.cloudflared/config.yml`, restarted `cloudflare-tunnel.service`
- DNS: CNAME `minecraft` → `a9e04ab3-...cfargotunnel.com` (proxied) via Cloudflare API
- Verified: local HTTP 200 + public HTTPS 200
