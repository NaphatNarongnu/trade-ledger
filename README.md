# Trade Ledger

A private, single-file trading journal for disciplined forex/crypto trading. No backend, no build step — it's one `index.html` you can open locally or host anywhere static files are served (GitHub Pages, Netlify, etc.).

Originally built as a Claude.ai artifact; this version adds a `localStorage` fallback so it also runs as a fully standalone web page.

## Features

- **Trade log** — symbol, direction, entry price, lot size, P/L, RR, TP/SL, free-text notes and a dedicated "psychology / lessons" field per trade
- **Chart screenshots** — attach an image to any trade, view it later in a lightbox
- **Pre-trade checklist** — a configurable list of rules (time window, daily trade limit, SL/TP set, real confluence, lot-sizing formula, etc.) that must all be checked before the "Log trade" button unlocks
- **Cool-down timer** — locks new entries for 30 minutes after logging a trade, to interrupt revenge-trading / overtrading loops
- **Risk dashboard** — auto-computed stats across every trade in the ledger: win rate, net P/L, % of trades with a stop-loss set, stop-out (margin call) count, largest lot size used, biggest win/loss, and a per-symbol breakdown
- **Trades-per-day chart** — a stacked bar chart (wins vs. losses) with a symbol filter dropdown and net P/L labeled above each day's bar
- **Everything is private** — data is stored per-user, never shared

## Running it

Just open `index.html` in a browser. That's it — no install, no dependencies.

To host it:

```bash
git clone <this-repo>
cd trade-ledger
# open index.html directly, or serve it:
python3 -m http.server 8000
```

### Deploying to GitHub Pages

1. Push this repo to GitHub.
2. Repo Settings → Pages → Deploy from branch → `main` / root.
3. Your journal will be live at `https://<username>.github.io/<repo>/`.

## Data & storage

- Inside Claude.ai, trade data is saved through Claude's artifact `window.storage` API (private per user).
- Outside Claude (standalone / GitHub Pages), the same code automatically falls back to the browser's `localStorage`, so everything still works — data just lives in that browser only. Clearing browser data will clear the journal, and it won't sync across devices.
- If you want cross-device sync for the standalone version, you'd need to swap the storage shim at the top of the `<script>` block for a call to your own backend (e.g. a small API, Firebase, Supabase, etc.) — the rest of the app only relies on `window.storage.get/set/delete/list`, so a drop-in replacement is enough.

## Customizing the checklist

The pre-trade checklist items are plain `<label>` elements inside `#checklistCard` in `index.html`. Edit, add, or remove them to match your own trading rules — the "all boxes checked" logic in `updateChecklist()` adapts automatically since it just queries `.chk` checkboxes.

## Disclaimer

This is a personal record-keeping tool, not trading advice, and not connected to any broker or exchange. Nothing in this app executes trades or fetches live prices — you log what you did after the fact (or right before, as a plan).

## License

MIT — see [LICENSE](LICENSE).
