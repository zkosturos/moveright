# Move Right — moveright.me

Marketing site + waitlist signup for Move Right.

## Stack

- Static HTML / CSS (no build step)
- Forms post to [Kit](https://kit.com) form `9446430`
- Hosted on [Vercel](https://vercel.com)

## Structure

```
moveright/
├── index.html         ← homepage (waitlist + about + map opt-in)
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

## Forms (Kit)

Both signup forms post to a single Kit form:
`https://app.kit.com/forms/9446430/subscriptions`

The hero form is tagged `data-source="hero"` and the map-section form is tagged `data-source="map"` so you can split signups by location later — for now they flow into the same Kit list.

To change the form ID, search `index.html` for `9446430` and replace.

## Routing notes

`vercel.json` enables `cleanUrls`, so:

- `/privacy` serves `privacy.html`
- `/terms` serves `terms.html`
- `.html` extensions in the URL bar redirect to the clean version

Internal links can use either form; clean URLs (`/privacy`) are preferred for the canonical version.
