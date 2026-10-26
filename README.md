# settled.social — website for Settled (Sidebar Interactive LLC)

Static site for GitHub Pages. No build step: `index.html` + `assets/`.

| File | Purpose |
|---|---|
| `index.html` | The page (styles and script inline) |
| `404.html` | Branded not-found page |
| `about/`, `support/`, `privacy/`, `terms/` | Company and legal pages (shared styles in `assets/site.css`) |
| `CNAME` | Tells GitHub Pages to serve `settled.social` |
| `.nojekyll` | Skip Jekyll processing |
| `assets/` | Logo, app screens, favicons, social share image (`og-image.png`, 1200×630) |

## Beta sign-up form

Both forms post JSON to the Settled API's `POST /v1/waitlist` (contract in
`settled-app/settled`: `docs/api/openapi.yaml`). Sign-ups land in the app's
Postgres. Approving someone sends them a TestFlight invite
(`php artisan settled:waitlist-invite`, see that repo's
`docs/09-testing-guide.md`). The API allows this origin only through CORS,
so the form only works when served from `settled.social`. The hidden `website`
field is a honeypot; leave it hidden.

## Deploy

1. Create a public GitHub repo (e.g. `settled-website`) and push this folder to `main`.
2. Repo → Settings → Pages → Source: **Deploy from a branch**, Branch: `main` / `(root)`.
3. Custom domain: `settled.social` (already set by the `CNAME` file).
4. Cloudflare DNS for settled.social (Proxy status **DNS only**, grey cloud):

   | Type | Name | Content |
   |---|---|---|
   | A | @ | 185.199.108.153 |
   | A | @ | 185.199.109.153 |
   | A | @ | 185.199.110.153 |
   | A | @ | 185.199.111.153 |
   | AAAA | @ | 2606:50c0:8000::153 |
   | AAAA | @ | 2606:50c0:8001::153 |
   | AAAA | @ | 2606:50c0:8002::153 |
   | AAAA | @ | 2606:50c0:8003::153 |
   | CNAME | www | `<your-github-username>.github.io` |

5. Once GitHub shows the DNS check passing and the certificate is issued, tick **Enforce HTTPS**.

## Replacing assets

`assets/logo.png` was cut from the concept mockup and is low resolution. Drop in a
high-res transparent PNG of the wordmark (≈840px wide) under the same name when you have one.
