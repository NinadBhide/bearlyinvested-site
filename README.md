# Bearly Invested — apex site (`bearlyinvested.app`)

Static site served at the apex domain. Today it hosts the two required legal pages plus a
minimal landing placeholder. **This folder is the root of the future marketing website** —
when the real landing page is built, it grows in `index.html` (see the comment block in that
file). The `/privacy` and `/terms` URLs must stay live regardless: the app, the app stores,
and the Google OAuth consent screen all depend on them.

```
legal-site/
  index.html          → https://bearlyinvested.app/         (landing placeholder — future marketing site)
  privacy/index.html  → https://bearlyinvested.app/privacy  (matches constants/legal.ts PRIVACY_URL)
  terms/index.html    → https://bearlyinvested.app/terms     (matches constants/legal.ts TERMS_URL)
  CNAME               → apex domain, used by GitHub Pages
```

Folder-`index.html` layout means `/privacy` and `/terms` resolve cleanly with no `.html`
extension on GitHub Pages, Cloudflare Pages, or Vercel.

## Status

**Content is final and ready to host** (2026-07-08). Operator named as *Ninad Sadashiv Bhide*,
governing law *New South Wales, Australia*, contact *support@bearlyinvested.app*. Placeholders
and editor note-boxes have been removed. Not lawyer-reviewed — fine to launch from, revisit with
real revenue / a registered entity.

⚠️ **`support@bearlyinvested.app` does not receive mail yet.** The address is published on both
legal pages, so before (or shortly after) going live, set up inbound email for it — e.g. a
Name.com email-forwarding record to your personal inbox, Cloudflare Email Routing, or Resend
inbound. Tracked in the repo `TODO.md`.

## Recommended host: GitHub Pages

1. Push `legal-site/` to a repo (a dedicated `bearlyinvested-site` repo is cleanest, or a
   `gh-pages`/`/docs` setup in an existing one).
2. Repo → **Settings → Pages** → deploy from the branch/folder containing these files.
3. Set the **Custom domain** to `bearlyinvested.app` (the `CNAME` file already declares it).
4. In **Name.com DNS**, point the apex at GitHub Pages:
   - `A  @  185.199.108.153`
   - `A  @  185.199.109.153`
   - `A  @  185.199.110.153`
   - `A  @  185.199.111.153`
   - (leave the existing `api` CNAME → Railway untouched.)
5. Wait for DNS + GitHub's cert, then tick **Enforce HTTPS** (required — `.app` is HTTPS-only
   via HSTS preload).
6. Verify: `https://bearlyinvested.app/privacy` and `/terms` both load over HTTPS.

Alternative hosts (Cloudflare Pages, Vercel) work identically — point them at this folder and
set the apex custom domain. Cloudflare Pages is worth considering since Cloudflare Email Routing
could then cover the `support@` inbound mail in the same dashboard.

## After the pages are live

- Confirm the in-app links open them (`constants/legal.ts` already points at the right URLs).
- Google Cloud Console → **OAuth consent screen**: add the Privacy + Terms URLs, then publish
  **Testing → Production**. No sensitive scopes are requested, so this does not trigger Google's
  full verification review.
- Verify a **non-test** Google account can now sign in end-to-end in the dev client.
