# stylized-cozy-3d-games-v1

Images of 39 **very stylized 3D** games: cozy builders (Townscaper, Tiny Glade, ISLANDERS, Dorfromantik),
cozy adventures (A Short Hike, Lil Gator Game, Alba, Untitled Goose Game), painterly / graphic adventures
(Sable, TUNIC, Journey, ABZU, Outer Wilds), stylized puzzle games (The Witness, Return of the Obra Dinn,
Monument Valley, Lara Croft GO) and stylized action (SUPERHOT, SUPERHOT: MIND CONTROL DELETE, Hi-Fi RUSH,
Sayonara Wild Hearts, Okami HD). Photoreal titles and flat 2D games were deliberately excluded.
Collected 2026-10-05. Companion to `lowpoly-block-games-v1`.

- **476 images**: 320 official Steam gameplay screenshots (5–10 per game, almost all 1920×1080) and 156
  pieces of official Steam key art (4 per game: header, main capsule, library portrait, library hero).
- **Source:** the Steam store for every game (`store.steampowered.com/api/appdetails` + Steam CDN). No
  login needed.
- **Categories:** `stylized-adventure` (12), `cozy-builder` (7), `stylized-puzzle` (7), `cozy-adventure` (6),
  `stylized-action` (6), `stylized-strategy` (1: Bad North).
- **Required titles** (`must_include: true` in `games.jsonl`): Townscaper, Cloud Gardens, Alba: A Wildlife
  Adventure, Bad North: Jotunn Edition, SUPERHOT, SUPERHOT: MIND CONTROL DELETE, A Short Hike, Sable,
  Lil Gator Game, ISLANDERS, Untitled Goose Game.

## Layout

```
images/<slug>/ss-NN.jpg              Steam screenshots, full size (usually 1920x1080), store order
images/<slug>/header.jpg             Steam header capsule (460x215)
images/<slug>/capsule.jpg            Steam main capsule (616x353)
images/<slug>/library_600x900.jpg    Steam library portrait art
images/<slug>/library_hero.jpg       Steam library hero banner (1920x620)
thumbs/                              480px previews used by index.html
games.jsonl                          one row per game: slug, name, category, source, url, steam_appid, website,
                                     developers, publishers, genres, release_date, short_description,
                                     must_include, n_images, n_screenshots, images[]
manifest.jsonl                       one row per image: slug, kind (screenshot|art), path, source_url,
                                     px_width, px_height, bytes, sha256, captured_at
index.html                           static gallery with category filter and text search
meta/<slug>.json                     trimmed Steam appdetails response + source URL of every downloaded file
steam_games.txt                      slug|appid|category
```

## Procedure

1. App IDs were resolved with Steam's public `storesearch` API and checked by hand (e.g. Bad North: Jotunn
   Edition is app 688420, the base game; TOEM is the original 1307580, not TOEM 2).
2. `fetch_steam.py` downloads up to 10 `path_full` screenshots per game plus the four standard
   capsule/library images from the Steam CDN, and writes `meta/<slug>.json`. Description GIFs are also
   harvested when present (none of these games had any >=300px wide).
3. `build_index.py` writes `games.jsonl`, `manifest.jsonl`, `thumbs/` and `index.html`.

Considered and dropped: Wilmot Works It Out, Spiritfarer, Cozy Grove (flat 2D).

## Reproduce

```
uv venv .venv && uv pip install --python .venv/bin/python pillow
.venv/bin/python fetch_steam.py      # skips existing files; pass slug,slug to limit
.venv/bin/python build_index.py
```

All images are the property of their respective developers and publishers. They were collected from
public store pages for research use only.
