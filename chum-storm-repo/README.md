# Chum Storm 🐟

A bullet-heaven / "bullet hell" style browser game — auto-toss fish (and
spray catnip) at an ever-more-absurd horde of cats instead of shooting
bullets. Built as a single self-contained HTML5 Canvas page, no build step,
no dependencies.

Inspired by the "Potato" mobile bullet-hell genre (Brotato-style auto-fire
survivor games), reskinned around feeding a horde of cats rather than
fighting them off.

## How to play

- **Hold and drag anywhere** on screen (or click-drag with a mouse) to move.
- You **auto-throw fish** at wherever you're currently pointing — no manual
  aim button needed, just point where you want to throw. Fish spread out
  evenly around a full circle as you gain more of them, so extra fish
  always cover a new direction instead of clustering together.
- You also automatically fire **Catnip Spray**, a short-range conal AOE that
  hits everything in a wedge in front of you on its own cooldown — a
  crowd-control layer that doesn't need aiming precision.
- The run is split into **Days**. Each day lasts 45 seconds; survive it and
  the horde retreats for the night while you get a small heal, then returns
  the next day denser, faster, and with a new cat type unlocked. A boss
  ("Catzilla") visits once per day starting Day 2.
- Cats get progressively more absurd day over day: kitten → cool cat →
  astro cat → wizard cat → roomba cat → hydra cat → Catzilla.
- Fish and catnip don't kill cats, they **feed** them — get one down to 0
  hunger and it pops with a big heart-burst and drops a glowing XP star that
  bounces out and settles before you can pick it up.
- Leveling up gives you a choice of 3 stacking upgrades (damage, cast speed,
  fish speed, max HP, move speed, pierce, pickup radius, HP regen) — most
  can be picked multiple times and stack in tiers, shown as a "Lv 2 / Lv 3"
  badge on the card. Every 5 levels you also automatically gain another
  simultaneous fish, no picking required.
- Every cat type you feed for the first time moves into your **Cat Home**
  (the start screen) permanently — saved in the browser's local storage, so
  your collection persists across sessions.
- Patches of grass are scattered around the arena in noise-based clusters —
  only you (not the cats) can push through and bend them.

## Running it locally

No build step — just open `index.html` in a browser. If your browser is
picky about loading local images from a `file://` page, serve the folder
with any static file server instead, for example:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deploying to GitHub Pages

1. Push this folder to a GitHub repository (see structure below — `index.html`
   needs to sit at the root, or in `/docs` if you configure Pages that way).
2. In the repo, go to **Settings → Pages**.
3. Under "Build and deployment", set **Source** to "Deploy from a branch",
   pick your default branch and the `/ (root)` folder.
4. Save — GitHub will give you a URL like
   `https://<your-username>.github.io/<repo-name>/` within a minute or two.

```bash
git init
git add .
git commit -m "Initial commit: Chum Storm"
git branch -M main
git remote add origin https://github.com/<you>/<repo-name>.git
git push -u origin main
```

## Project structure

```
.
├── index.html          # the entire game — HTML, CSS, and JS in one file
└── assets/
    ├── cats/            # cat sprite crops (cat01–cat06.png)
    ├── player/          # player directional walk sprites
    └── fx/              # fish projectile + grass patch sprites
```

## A note on the art assets

The sprites in `assets/` were supplied by the project owner as placeholders
for prototyping and personal proof-of-concept use. Before making this repo
public, distributing a build, or shipping it anywhere beyond your own
testing, double check you actually hold (or have licensed) the rights to
those specific images — swap in your own art or a properly licensed set if
not. Everything else (code, mechanics, day/wave system, procedural
floor/grass generation, upgrade system, catnip weapon) is original to this
project.

## Tech notes

- Pure vanilla JS + Canvas 2D, no framework, no build tooling.
- All sprites are drawn with `imageSmoothingEnabled = false` to stay crisp
  at any scale.
- Save data (cat collection) lives in `localStorage` under
  `chumstorm_collection_v1`.
- The floor is a flat procedurally-filled canvas tile (see `buildFloorTile()`
  in `index.html`), not an image asset.
- Grass clustering uses a small hand-rolled value-noise function (see
  `buildNoiseSampler()`) rather than an external noise library.
- Cat hit-reactions (damage, squash-bounce, knockback, death/feed) run
  through a single shared `hitCat()` function, so fish and catnip spray both
  produce identical, consistent feedback.
- Day/wave pacing lives in the `dayNumber` / `dayElapsed` / `DAY_LENGTH`
  variables near the top of the script if you want to retune it.
