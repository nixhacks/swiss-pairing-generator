# Swiss Pairing Generator

A single-file, browser-only Swiss-system pairing tool for a monthly club tournament
(**4–5 Rd SS, G/90; +30** — round 1 on the first Thursday, final round on the last Thursday).

No server, no build step, no dependencies. Open the HTML file and everything runs locally in
your browser; the event is stored in `localStorage` and nothing is uploaded anywhere.

## Features

- **Event setup** — pick month/year and the Thursday schedule is generated automatically. The
  round count is capped by the number of Thursdays in the month (a 4-Thursday month cannot be a
  5-round event).
- **Flexible player import** — paste from a wall chart, a spreadsheet, or type by hand. Only
  **name** and **USCF rating** are read; pairing numbers, scores (`0.0`), result codes
  (`H---`, `W12`, `B---`) and extra columns are ignored. An 8+ digit US Chess ID is captured
  when present, and `UNR` / blank marks a player unrated.

  ```text
  Joshini Sudhakar, 1989
  Mrinalini Sudhakar, 1139
  1  Eric Gahlon        1939   H---   0.0
  Kevin Landman 1790
  ```

- **Pairing engine** — score groups paired top-half vs bottom-half with backtracking, so
  rematches are avoided, down-floats are chosen intelligently, and colour history is respected
  (alternation, balance, and avoiding three of the same colour in a row). Round 1 uses the USCF
  colour alternation down the boards.
- **Byes** — click a round chip on any player to cycle through:
  - `R2` playing
  - `R2 ½` half-point bye (0.5, shown as `H---`)
  - `R2 U` unplayed (0.0, shown as `U---`)

  A player can request several rounds off. If the remaining field is odd, the lowest-scoring
  eligible player who has not already had one receives the full-point bye (`B---`).
- **Results and standings** — enter each board's result (including forfeits); standings show
  score, per-round result with colour, Solkoff and cumulative tiebreaks.
- **Output** — copy pairings or standings to the clipboard, download standings as CSV, print a
  pairing sheet, and export/import the whole event as JSON.

## Usage

1. Open `swiss-pairing-generator.html` (double-click it, or use the hosted link below).
2. **Event** tab — name the event, choose the month, rounds and time control.
3. **Players** tab — paste or import the roster; set any ½-point / unplayed byes.
4. **Pairings** tab — *Generate next round pairings*, post them, then record results.
5. Repeat for each round; check the **Standings** tab at any time.

Tip: export the JSON after each round as a backup, especially if you might use a different
device or clear your browser data.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | Landing page linking to the tools (entry point for GitHub Pages) |
| `swiss-pairing-generator.html` | The pairing generator (this project) |
| `swiss-pairing-ledger.html` | Alternate ledger-style interface |
| `LICENSE` | MIT license |

## Hosting on GitHub Pages

The project is fully static, so GitHub Pages can serve it as-is.

### 1. Create the repository

```bash
cd "swiss-pairing-generator"
git init
git add .
git commit -m "Swiss pairing generator"
git branch -M main
```

Create an empty repo on github.com (do **not** add a README/license there — they already exist
here), then:

```bash
git remote add origin https://github.com/<your-username>/swiss-pairing-generator.git
git push -u origin main
```

### 2. Turn on GitHub Pages

1. Repository → **Settings** → **Pages**.
2. **Source**: *Deploy from a branch*.
3. **Branch**: `main`, folder `/ (root)` → **Save**.
4. Wait ~1 minute, then open:

   ```
   https://<your-username>.github.io/swiss-pairing-generator/
   ```

`index.html` is served automatically; the generator is at
`.../swiss-pairing-generator/swiss-pairing-generator.html`.

### 3. Updating

```bash
git add -A
git commit -m "Describe the change"
git push
```

Pages redeploys within a minute. If you don't see the change, hard-refresh (Cmd+Shift+R) — the
browser caches HTML aggressively.

### Notes

- The repository must be **public** for Pages on a free account (private repos need GitHub Pro).
- Do not add a `.nojekyll` file requirement — none of the filenames start with `_`, so Jekyll
  processing is harmless. Add an empty `.nojekyll` only if you later add underscore-prefixed
  folders.
- Tournament data lives in each visitor's browser (`localStorage`), so the hosted page never
  shows your data to anyone else — and clearing site data wipes the event.

## Which `.gitignore` template?

There is no build tooling here, so on GitHub's "Add .gitignore" dropdown you can pick
**None** — the `.gitignore` included in this repo is all you need. It ignores macOS/editor
junk (`.DS_Store`, `.vscode/`) plus exported tournament files (`swiss-*.json`,
`standings-*.csv`) so real player data doesn't get committed by accident.

If you later add tooling (npm scripts, a bundler, tests), switch to GitHub's **Node** template
and merge these entries into it.

## License

MIT — see [LICENSE](LICENSE).

Not affiliated with or endorsed by US Chess. Verify current pairing/tiebreak rules and your
affiliate/TD status with US Chess before running a rated event.
