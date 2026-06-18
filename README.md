# Move Right — moveright.me

Marketing site + waitlist signup for Move Right.

## Stack

- Static HTML / CSS (no build step)
- Single "Join Us Thursday" / "Save My Seat" CTA points to the current [Luma](https://luma.com) event
- Hosted on [Vercel](https://vercel.com)

## Structure

```
moveright/
├── index.html         ← homepage (hero + fork + how it works + about + Luma CTA)
├── privacy.html       ← /privacy
├── terms.html         ← /terms
├── assets/            ← favicon, logos, hero image, OG image
├── images/            ← portraits, opt-in background
├── vercel.json        ← clean URLs config
├── .gitignore
└── README.md
```

Everything in this folder is production. No drafts, no unused sources.

## Local preview

Any static server works:

```bash
# Python 3
python3 -m http.server 8000

# Node
npx serve .
```

Then open <http://localhost:8000>.

## Deploy

### First-time setup

1. Push this folder to a GitHub repo (e.g. `moveright-site`).
2. Go to [vercel.com](https://vercel.com) → **Add New → Project → Import** your repo.
3. **Root Directory:** repo root (or `moveright/` if you nested it inside a larger repo).
4. **Framework Preset:** Other.
5. **Build Command:** leave empty.
6. **Output Directory:** leave empty.
7. Click **Deploy**.

### Ongoing

Every `git push` to `main` triggers a production deploy. Branches and PRs get preview URLs automatically.

### Custom domain

Project → **Settings → Domains** → add `moveright.me`. Vercel will give you DNS records to add at your registrar. HTTPS provisions automatically.

## Event signup (Luma)

There are no forms on the site. Every "Join Us Thursday" and "Save My Seat" button
links to the current week's Luma event. All of them are class `cta-luma` and are
pointed at a single source of truth.

### Updating the event weekly (the one thing to remember)

Open `index.html`, find the `LUMA_URL` constant in the `<script>` near the bottom:

```js
const LUMA_URL = "https://luma.com/xxxxxxxx";
```

Replace it with the next gathering's Luma URL, commit, and push. A small script
rewrites every `.cta-luma` link's `href` on load, so updating that one line updates
every button on the page. (The buttons also carry the URL inline as a no-JS fallback,
so for a belt-and-suspenders update you can find/replace the old URL across the file.)

## Routing notes

`vercel.json` enables `cleanUrls`, so:

- `/privacy` serves `privacy.html`
- `/terms` serves `terms.html`
- `.html` extensions in the URL bar redirect to the clean version

Internal links can use either form; clean URLs (`/privacy`) are preferred for the canonical version.
