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
| `index.html` | The landing page: hero, format chips, features, how it works, closing CTA |
| `styles.css` | All styling, including the responsive breakpoints |
| `privacy/index.html` | Privacy policy — this is the URL to give the Play Console |
| `404.html` | Custom not-found page (GitHub Pages serves it automatically) |
| `assets/` | Icons, favicons, social image, and the social image's HTML source |
| `.nojekyll` | Disables Jekyll processing |

## Before launch: review these

The Play Store links and the support address are final and no placeholders remain in the
markup. What still needs a human eye:

| What | Where | Notes |
|---|---|---|
| Privacy policy text | `privacy/index.html` | **Draft** — written from the app's actual behaviour, but review it before publishing |
| Policy date | `privacy/index.html` | Currently 1 January 2026 |
| Data controller identity | `privacy/index.html` | The policy names no company or individual as data controller; add one if you need it for GDPR purposes |

Contact address used throughout: **intervaltimerpro@gmail.com**.

## About the screenshots

The hero uses **real device captures**, copied unchanged from the app repo's
`store-assets/` folder:

| Site file | Source | Shows |
|---|---|---|
| `assets/work.jpg` | `store-assets/work.jpg` | The running timer in a WORK phase (720×1571) |
| `assets/setup.jpg` | `store-assets/setup.jpg` | The setup screen with prep / work / rest / rounds (720×1569) |

They sit inside the CSS phone frames (`.phone`), which supply the bezel, radius and shadow. The
back phone is rotated, dimmed and overlapped by 30px behind the front one — the overlap is kept
small on purpose, because the setup capture fills its full width and a deep overlap slices its
value boxes mid-word.

To swap a screenshot, replace the file and update the `width`/`height` attributes on the `<img>`
so the browser still reserves the right space before the image loads. Keep
`decoding` synchronous on the front phone: it is the largest above-the-fold image, and
`decoding="async"` let it paint blank in headless captures.

The favicons, `apple-touch-icon` and `favicon.ico` are downscaled from
`store-assets/app-icon-512.png` — the best-resolution icon in the tree (the launcher density
buckets top out at 192px, so using those would have meant upscaling).

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
