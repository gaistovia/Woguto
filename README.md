# Woguto — Landing Site

Static conversion landing page (mobile-first) — no build step, no framework.
This is the same site that was previously packaged for Blogger; all Blogger-specific
markup (`b:skin`, `b:section`, XML namespaces) has been removed and replaced with
plain, standard HTML/CSS/JS so it deploys anywhere, including Vercel.

## Project structure

```
.
├── index.html      Page markup
├── styles.css      All CSS (design tokens, layout, components)
├── script.js       All JS (mock data, rendering, routing, interactions)
├── vercel.json      Vercel config (clean URLs)
└── .gitignore
```

No `package.json`, no build step. Vercel serves these three files directly as a
static site.

## Preview locally

Any static file server works, for example:

```bash
npx serve .
# or
python3 -m http.server 8080
```

Then open the printed local URL in a browser.

## Deploy with Git + Vercel

### 1. Push to Git

```bash
git init
git add .
git commit -m "Initial commit: Woguto landing site"
git branch -M main
git remote add origin <YOUR_GIT_REMOTE_URL>
git push -u origin main
```

(Create the empty repo on GitHub/GitLab/Bitbucket first, then use its URL as
`<YOUR_GIT_REMOTE_URL>`.)

### 2. Import into Vercel

**Option A — Dashboard:**
1. Go to [vercel.com/new](https://vercel.com/new).
2. Import the Git repository you just pushed.
3. Framework Preset: choose **"Other"** (or leave as detected — there's no
   build step, so Vercel will just serve the static files).
4. Click **Deploy**. Done — no environment variables or build settings needed.

**Option B — CLI:**
```bash
npm i -g vercel
vercel        # first deploy, follow the prompts
vercel --prod # promote to production
```

Every subsequent `git push` to your connected branch triggers an automatic
Vercel deployment.

## How to customize

| What | Where |
|---|---|
| Real signup/app URL | `script.js` → search `APP_URL` near the top. Change the one line and every "Fungua Account – TZS 16,000/=" / "SAMBAZA ULIPWE" button uses it. |
| Open Graph / social preview image + domain | `index.html` → search `YOUR-DOMAIN.com` (4 tags: `og:image`, `og:url`, `twitter:image`). Replace with your real deployed domain once you have one. The image itself is `og-image.png` in this folder — regenerate it if the branding changes. |
| Campaign / product / idea data | `script.js` → search `MOCK DATA`. One JS object `MOCK_DATA` with `campaigns`, `products`, `ideas` arrays. Copy an existing item's shape to add more. |
| Colors / fonts | `styles.css` → search `TOKENS` at the top (`--green-deep`, `--brown-deep`, `--font-display`, `--font-accent`). |
| Logo images | `index.html` → search `LOGO_ICON_BASE64` (header) and `LOGO_FULL_BASE64` (footer). Both are embedded as base64 `data:image` URIs — replace the string after `base64,` with a new export if you get a new logo. |
| Mobile money network logos (M-Pesa, HaloPesa, Mixx by Yas, Airtel Money) | `script.js` → search `NETWORKS` near "LIVE TRANSACTIONS FEED". |
| Items per page in the opportunity grid | `script.js` → search `PAGE_SIZE = 9` and change the number. |
| Masked phone number format | `script.js` → function `maskedPhone()`. |

## Important — mock/demo data still in place

Two things in this build simulate live activity for demo purposes and are
**not connected to a real backend**:

1. **Activity ticker** (top of page, under the header) — rotates fabricated
   "so-and-so just joined" messages. Function: `nextTickerMessage()`.
2. **Live Transactions Feed** ("Miamala ya Hivi Karibuni") — rotates
   fabricated mobile-money payout rows. Function: `renderTxRow()`.

Both are clearly marked in `script.js` with `IMPORTANT` comments. **Before
this goes live for real visitors, wire both to your real signup/payout data
(or remove them)** — showing fabricated activity as if real, to real users,
crosses from persuasive design into deception.

## What changed from the Blogger version

- Removed `<?xml version="1.0"?>` declaration and Blogger namespaces
  (`xmlns:b`, `xmlns:data`, `xmlns:expr`) from `<html>`.
- Converted `<b:skin><![CDATA[...]]></b:skin>` back into a plain external
  `styles.css` file.
- Converted the inline `<script>//<![CDATA[ ... //]]></script>` block into a
  plain external `script.js` file (no CDATA wrapper needed outside Blogger's
  XML parser).
- Removed the empty `<b:section id='moxera-shell' .../>` placeholder that
  Blogger's theme validator required.
- Renamed the internal code variable `MOXERA_APP_URL` → `APP_URL` (this is
  just an internal variable name — it was never shown to visitors).

Everything else — layout, copy, mock data, interactions, embedded photos and
logos — is unchanged.
