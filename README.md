# intervalpro.github.io

Landing page for **IntervalPro**, an Android interval timer for HIIT, boxing, circuits,
CrossFit and Hyrox.

**Live:** https://intervalpro.github.io

Plain static HTML + CSS. No build step, no JavaScript, no frameworks, no webfonts, no
third-party requests of any kind. Mobile-first, dark palette lifted from the app itself so
the site and the product look like the same thing.

## How it is published

GitHub Pages, **Deploy from a branch** → `main` → `/ (root)`.

The empty `.nojekyll` file stops GitHub from running the site through Jekyll. Without it,
Jekyll still serves plain HTML fine, but it adds a build step and silently ignores paths
beginning with `_`.

Deploys go live about a minute after a push. HTTPS is automatic on `*.github.io`.

## Files

| Path | Purpose |
|---|---|
| `index.html` | The landing page: hero, features, how it works, pricing, FAQ, CTA |
| `styles.css` | All styling, including the responsive breakpoints |
| `privacy/index.html` | Privacy policy — this is the URL to give the Play Console |
| `404.html` | Custom not-found page (GitHub Pages serves it automatically) |
| `assets/` | Icons, favicons, social image, and the social image's HTML source |
| `.nojekyll` | Disables Jekyll processing |

## Before launch: replace these placeholders

Search the files for `PLACEHOLDER` — each spot is marked with an HTML comment.

| What | Where | Notes |
|---|---|---|
| Play Store URL | `index.html` (4 links) | Currently `…details?id=com.gsi85.intervalpro`, which is correct once the listing is public and 404s until then |
| Support email | `index.html` footer, `privacy/index.html` (×3) | Currently `contact@example.com` |
| Remove-ads price | `index.html` pricing | The text says "One-time"; add the real price if you want it shown |
| Privacy policy text | `privacy/index.html` | **Draft** — written from the app's actual behaviour, but review it before publishing |
| Policy date | `privacy/index.html` | Currently 1 January 2026 |

## About the phone mockups

The two phones in the hero are **hand-built HTML and CSS**, not screenshots. They are
accurate to the app's real palette and layout vocabulary (prep grey `#424242`, work green
`#1B5E20`, rest red `#B71C1C`, start `#2E7D32`), but they are an illustration. Replace them
with real captures when you have them — drop the images in `assets/` and swap the `.phone`
blocks for `<img>` elements with `width`/`height` set and meaningful `alt` text.

## Regenerating the social image

`assets/og-image.png` (1200×630) is rendered from `assets/og-template.html`. Edit the
template, then:

```powershell
& 'C:\Program Files\Google\Chrome\Application\chrome.exe' `
  --headless=new --disable-gpu --hide-scrollbars --force-device-scale-factor=1 `
  --window-size=1200,630 `
  --screenshot="assets\og-image.png" `
  "file:///C:/Dev/intervalpro.github.io/assets/og-template.html"
```

Re-render it whenever the headline or tagline changes — crawlers cache it hard.

## ⚠️ Screenshot gotcha (worth knowing before you "fix" the mobile layout)

Chrome's `--window-size` **cannot go below about 512px** in headless mode. Asking for
`--window-size=390,844` gives you a layout viewport of **512px**, cropped to 390px wide — so
a *correct* mobile layout looks broken (text clipped at the right edge) and sends you off
editing CSS that was never wrong.

Check it yourself: `build/diag.html` prints `innerWidth`. To screenshot a real 390px
viewport, load `build/frame.html?w=390&h=3600` (an iframe at the exact width, so the media
queries see 390px) and crop the image to the iframe rect. `build/` is gitignored.

## Local preview

```powershell
cd C:\Dev\intervalpro.github.io
python -m http.server 8080
```

Then open http://localhost:8080. Opening `index.html` directly from disk also works, since
every path in the site is relative.
