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

Both signup forms post to Substack's free-subscribe endpoint for the `zkosturos` publication:
`https://zkosturos.substack.com/api/v1/free`

The hero form is tagged `data-source="hero"` and the map-section form is tagged `data-source="map"` — that value is passed through to Substack as the signup `source` field, so you can tell later which form a subscriber came from. Both flow into the same Substack list.

### Notes on the integration

- The request is sent as JSON (`email`, `first_url`, `first_referrer`, `current_url`, `current_referrer`, `referral_code`, `source`) using `fetch` in `no-cors` mode. The response is opaque, which is fine — Substack still records the signup.
- Subscribers stay on the page; the form swaps to an inline "You're on the list" success state. Substack handles the welcome email.
- If the network call fails, the page optimistically shows success anyway so the UX never feels broken. The signal that something went wrong is the missing welcome email — test with your own address after any change.
- `substack.com/api/v1/free` is **undocumented**. It has been stable for years and is what Substack's own embed iframe uses internally, but there's no public SLA. If it ever breaks, the fallback is to switch each form's `action` to `https://zkosturos.substack.com/subscribe` and change the JS handler to redirect with the email as a query param (`window.location = f.action + '?email=' + encodeURIComponent(input.value)`). A breadcrumb comment in `index.html` calls this out.

To change the Substack publication, search `index.html` for `zkosturos.substack.com` and replace.

## Routing notes

`vercel.json` enables `cleanUrls`, so:

- `/privacy` serves `privacy.html`
- `/terms` serves `terms.html`
- `.html` extensions in the URL bar redirect to the clean version

Internal links can use either form; clean URLs (`/privacy`) are preferred for the canonical version.
