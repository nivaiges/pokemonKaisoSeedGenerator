# Kaizo Seed Generator (UI prototype)

A dashboard for picking out the best randomizer seeds for a Kaizo-style Pokémon run. It lists seeds, scores them, and lets you filter and sort them so you can quickly find a seed with a strong starter and strong early encounters.

**Live demo:** https://nivaiges.github.io/pokemonKaisoSeedGenerator/

> **Status: front-end prototype.** This repo contains the UI only. It does **not** generate, randomize or analyze ROM seeds yet. Every seed, score and count on the page is hardcoded sample data in `index.html`.

## The problem it's meant to solve

In Kaizo (extra-hard) randomized Pokémon runs, the seed decides your starter and what you can catch early, and that largely decides whether the run is winnable. Finding a good seed by hand means rolling and checking many of them. The idea here is to scan seeds automatically, score each one, and keep only the "keepers" worth playing.

The sample data uses early Unova locations (Route 19, Route 20, Floccesy Ranch), which suggests Black 2 / White 2 as the target game. The repo doesn't name a specific game or randomizer.

## What the page shows

- **Header and stat cards:** seeds scanned, keepers, carry hits and top score. These are fixed numbers in the HTML and don't change with the data.
- **Filters:** minimum score (0–2000), minimum floor (0–600), "Carry hits only", and "Starter must be carry".
- **Seed table:** one row per seed, sortable by clicking any column header (click again to flip the direction). The default sort is score, highest first.
- **Expanded row:** click a seed to see each early area, its score, how many good encounters it has, and each Pokémon's base stat total (BST) and encounter rate. Pokémon marked as a carry get a `CARRY` badge.

### Columns

| Column | Meaning in the sample data |
|---|---|
| Seed | Seed ID shown as hex, e.g. `0xA42F19B` |
| Starter | Starter Pokémon, its BST, and a `CARRY` badge if it's flagged as one |
| Score | Overall seed score |
| Good | Number of "good" encounters across the listed areas |
| Floor | A per-seed value that, in every sample seed, equals the lowest area score |
| Carry | Number of carry-tier encounters |

The page has no scoring formula. Score, Good, Floor and Carry are stored values, not calculated, so they describe how the generator is meant to rate seeds rather than how it currently does.

## How it works

It's a single static file, `index.html`, with inline CSS and plain JavaScript. There's no build step, no dependencies and no network calls.

1. A `seeds` array holds the sample data. Each seed has a few summary fields plus an `areas` list, and each area lists Pokémon as `[name, bst, encounterRatePercent, isCarry]`.
2. `getFiltered()` reads the sliders and checkboxes and drops seeds that fail any filter.
3. `render()` sorts the filtered list by the active column and rebuilds the table as HTML strings. It's called again on every filter change, header click or row expand.
4. Expanded rows are tracked by seed ID in a `Set`, so they stay open when you change filters or sort.

## Running it

Open the live demo above, or run it locally:

```bash
git clone https://github.com/nivaiges/pokemonKaisoSeedGenerator.git
```

Then open `index.html` in any modern browser. If you'd rather serve it, any static server works, for example:

```bash
python -m http.server 8000
```

and visit http://localhost:8000.

There's no configuration, environment variables or API keys. To try other data, edit the `seeds` array in `index.html`.

## Limitations

- **No real seed generation.** Nothing reads a ROM, runs a randomizer or calculates scores. All data is the 7 hardcoded sample seeds.
- **Static header and stat cards.** "1,247 seeds processed", "18 keepers" and the other stat values don't follow the data or filters.
- **Pause is visual only.** It changes the button label and the status dot color. There's no scan to pause.
- **Placeholder buttons.** "Export keepers" does nothing. "Analyze" and "Favorite" just show an alert.
- **Score and Floor aren't defined.** The page displays them but doesn't say how they're computed.

## Tech

HTML, CSS (custom properties, grid) and vanilla JavaScript. Hosted on GitHub Pages.
