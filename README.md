# fasterai.com

A single-page, zero-dependency asset portal for the **fasterAI.com** domain.
Static HTML, CSS and vanilla JavaScript — no build step, no framework, no runtime.

## Structure

```
.
├── index.html                  # The page (Tailwind via CDN, inline critical CSS)
├── 404.html                    # Any stray path returns to /
├── assets/
│   ├── app.js                  # Assembles the contact mailto: at runtime
│   └── favicon.svg             # Forward-sheared 'F' mark
├── CNAME                       # Custom domain for GitHub Pages
├── robots.txt
├── sitemap.xml
├── .nojekyll                   # Serve files verbatim; skip Jekyll processing
└── .github/workflows/deploy.yml
```

## Local preview

Open `index.html` directly, or serve it so relative paths behave exactly as in production:

```bash
python3 -m http.server 8000
# → http://localhost:8000
```

## Deployment

Pushing to `main` triggers `.github/workflows/deploy.yml`, which uploads the
repository root as a Pages artifact and publishes it. No build stage runs.

One-time setup in the repository:

1. **Settings → Pages → Build and deployment → Source: GitHub Actions.**
2. Point the apex domain at GitHub Pages via DNS `A` records:
   `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   (and `AAAA` records `2606:50c0:8000::153` … `8003::153` for IPv6).
   Add a `CNAME` record for `www` → `<owner>.github.io`.
3. Once DNS resolves, tick **Enforce HTTPS**.

The `CNAME` file in this repository must keep matching the custom domain
configured under Settings → Pages, or Pages will drop the domain on the next deploy.

## Notes

- **Contact address.** The email never appears in the served markup. `assets/app.js`
  reassembles it from three `data-*` fragments and upgrades the anchor to a real
  `mailto:` link with a prefilled subject. With JavaScript disabled, the obfuscated
  form (`… [at] … [dot] …`) remains readable to a human.
- **Motion.** The reveal animation is applied only when JavaScript is present and is
  fully suppressed under `prefers-reduced-motion: reduce`.
- **Contrast.** Body and footer copy use `#A1A1AA` on `#0A0A0A` — roughly 7.7:1,
  clearing WCAG AA and AAA for body text.
