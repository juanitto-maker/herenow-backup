# here.now backup

Weekly snapshot + failover mirror of all persistent here.now sites for digitalAIventures.

## How it works

- GitHub Actions runs every **Sunday 03:00 UTC** (or trigger manually via Actions tab → *here.now backup* → **Run workflow**)
- For each slug in `slugs.txt`, fetches `https://<slug>.here.now/` with 5 retries + 10 s backoff
- Saves to `sites/<slug>/index.html`
- Commits only if something changed → git history = versioned snapshots of every site

## Failover

GitHub Pages serves this repo at:

**https://juanitto-maker.github.io/herenow-backup/**

Each site is reachable at:

`https://juanitto-maker.github.io/herenow-backup/sites/<slug>/`

## Adding a new site

Edit `slugs.txt` → add one slug per line → commit. Next run picks it up.
Lines starting with `#` are ignored.

## Limitations

- Backs up **only `index.html`** per site (single-file pages, which is how the here-now skill publishes). If a site has external assets or subpages, they won't be mirrored.
- If a site returns 5xx after all retries, the previous backup is kept and the failure is logged in the Actions run summary.
