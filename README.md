# Move Right — moveright.me

Marketing site + waitlist signup for Move Right.

## Stack

- Static HTML / CSS (no build step)
- Forms post to [Substack](https://substack.com) (`zkosturos.substack.com`)
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

## Forms (Substack)

Both signup forms hand the user off to your Substack publication served on your own custom domain:
`https://letters.moveright.me/subscribe`

The custom domain (`letters.moveright.me`) is mapped to the `zkosturos` Substack publication via a CNAME pointing at `target.substack-custom-domains.com`. Substack serves the subscribe page, all published posts, and the archive on that subdomain — readers never see a `*.substack.com` URL.

The hero form is tagged `data-source="hero"` and the map-section form is tagged `data-source="map"` — that value is passed through to Substack as `utm_medium` on the handoff URL, so you can tell later in Substack analytics which form a subscriber came from. Both flow into the same Substack list.

### How the handoff works

- The forms are native `<form method="get" action="https://letters.moveright.me/subscribe">` elements with an `<input name="email">`. A small JS handler intercepts the submit, swaps the button to a "Taking you to Substack…" state, and redirects to `https://letters.moveright.me/subscribe?email=<email>&utm_source=moveright.me&utm_medium=<hero|map>`.
- Substack's subscribe page picks up the prefilled email, the subscriber confirms with one click, and Substack sends the welcome email.
- If JavaScript is disabled, the native form still submits as a GET to the same URL and works identically — just without the button-state polish.
- Because the destination is `letters.moveright.me`, the URL bar stays on-brand throughout. No third-party API call, no opaque responses, no undocumented endpoints.

To change the Substack publication or custom domain, search `index.html` for `letters.moveright.me` and replace.

## Routing notes

`vercel.json` enables `cleanUrls`, so:

- `/privacy` serves `privacy.html`
- `/terms` serves `terms.html`
- `.html` extensions in the URL bar redirect to the clean version

Internal links can use either form; clean URLs (`/privacy`) are preferred for the canonical version.
