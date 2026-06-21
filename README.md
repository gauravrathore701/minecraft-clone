# CursedCraft — Minecraft Clone

A browser-based voxel sandbox game inspired by Minecraft, built entirely in Three.js as a single-file app. Place and break blocks in a procedurally styled 3D world — no install, runs in any browser.

**Live URL:** https://minecraft.cursedshrine.com

---

## Features

- 3D voxel world rendered with Three.js
- Block placement and destruction
- First-person camera controls (WASD + mouse look)
- Multiple block types
- Runs fully in-browser — no server-side logic

## Tech Stack

| Layer | Technology |
|-------|-----------|
| 3D Engine | Three.js |
| Frontend | Vanilla HTML/CSS/JS (single file) |
| Server | Python `http.server` (static file hosting) |
| Hosting | Raspberry Pi → Cloudflare Tunnel |

## Project Structure

```
minecraft-clone/
└── public/
    └── index.html     # Complete game (Three.js + game logic inline)
```

## Running Locally

```bash
cd public
python3 -m http.server 4175
# open http://localhost:4175
```

## Deployment

```bash
sudo systemctl status minecraft-clone
sudo systemctl restart minecraft-clone
```

Port `4175` → Cloudflare Tunnel → `minecraft.cursedshrine.com`.
