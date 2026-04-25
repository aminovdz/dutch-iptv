# Dutch IPTV — Complete Customization & Deployment Guide

## ✅ Changes Applied This Session

| Item | Status |
|---|---|
| Pricing reordered: 12M → 6M → 3M → 1M | ✅ |
| "Apparaten" removed from all tiers | ✅ |
| `loading="lazy"` on lifestyle image, `fetchpriority="high"` on hero | ✅ |
| Favicon SVG + PNG generated | ✅ |
| Canonical URL → `https://dutch-iptv.cc` | ✅ |
| `title=""` attribute added to both images | ✅ |

---

## 1. How to Change URLs Manually

### WhatsApp Number
Search for `31612345678` across the project and replace with your real number (no `+`, no spaces):

```bash
# Find all occurrences
grep -r "31612345678" src/
```

Files to edit:
- `src/layouts/Layout.astro` — sticky WhatsApp FAB + footer icon
- `src/layouts/LegalLayout.astro` — sticky WhatsApp FAB
- `src/pages/index.astro` — contact section + referral button + all 4 plan "Bestel Nu" buttons
- `src/pages/contact.astro` — WhatsApp contact card

**Format:** `https://wa.me/COUNTRYCODE+NUMBER`
Example: Dutch number `0612345678` → `https://wa.me/31612345678`

---

### Telegram Handle
Search for `dutchiptv` and replace with your real Telegram username:

```bash
grep -r "t.me/dutchiptv" src/
```

Files: `src/layouts/Layout.astro`, `src/pages/index.astro`, `src/pages/contact.astro`

**Format:** `https://t.me/YOUR_USERNAME`

---

### Email Address
Search for `info@dutchiptv.nl` and replace:

```bash
grep -r "dutchiptv.nl" src/
```

Files: `src/layouts/Layout.astro`, `src/pages/contact.astro`, all legal pages

---

### Buy Buttons (Per Plan)
Each pricing card's "Bestel Nu" button links to WhatsApp with a pre-filled message.
In `src/pages/index.astro`, find the `plans` array at the top and the `a href` on each card:

```astro
href={`https://wa.me/31612345678?text=Ik%20wil%20het%20${encodeURIComponent(p.period)}%20pakket%20bestellen`}
```

To link to a payment page instead, replace with:
```astro
href="https://jouw-betaalpagina.nl/checkout?plan=12maanden"
```

---

### Canonical & Site URL
In `src/components/BaseHead.astro` line 12–16 — already set to `https://dutch-iptv.cc`. If domain changes again:

```astro
canonical = 'https://dutch-iptv.cc',   // ← change this
const siteUrl = 'https://dutch-iptv.cc'; // ← and this
```

Also update `astro.config.mjs`:
```js
site: 'https://dutch-iptv.cc',  // ← change this too
```

---

### Social Media Links (Footer)
In `src/layouts/Layout.astro` find the "Social Icons" comment block:

```astro
href="https://facebook.com/dutchiptv"   // → your Facebook page
href="https://instagram.com/dutchiptv"  // → your Instagram handle
```

---

## 2. How to Change Images

### Replace Hero Image
Drop your new image into `public/` as `dutch-iptv-hero.png` (recommended: 1024×576px, WebP or PNG).
The `<img>` tag in `src/pages/index.astro` already references `/dutch-iptv-hero.png`.

To use a different filename, change line ~61 in `index.astro`:
```astro
src="/dutch-iptv-hero.png"   // → src="/your-new-image.png"
```

### Replace Lifestyle Image
Same process — drop file into `public/` as `beste-iptv-lifestyle.png` or update line ~84:
```astro
src="/beste-iptv-lifestyle.png"  // → src="/your-image.png"
```

### Replace Favicon
- **SVG:** overwrite `public/favicon.svg`
- **PNG:** overwrite `public/favicon.png` (also used as Apple touch icon at `public/apple-touch-icon.png`)

### Image Best Practices for Performance
- Use `.webp` format (smaller than PNG, same quality)
- Hero image: max 200KB
- Lifestyle image: max 150KB
- Convert with: `npx sharp-cli input.png -o output.webp`

---

## 3. Deploy on Cloudflare Pages

### Step 1 — Push to GitHub
```bash
cd /Users/Mc/Documents/antigravity/dutch-iptv-astro
git add .
git commit -m "Dutch IPTV landing page"
git remote add origin https://github.com/YOUR_USERNAME/dutch-iptv.git
git push -u origin main
```

### Step 2 — Create Cloudflare Pages Project
1. Go to [dash.cloudflare.com](https://dash.cloudflare.com)
2. Click **Workers & Pages** → **Create application** → **Pages**
3. Click **Connect to Git** → authorize GitHub → select your repo

### Step 3 — Build Settings
| Setting | Value |
|---|---|
| Framework preset | **Astro** |
| Build command | `npm run build` |
| Build output directory | `dist` |
| Node.js version | `22` |

Click **Save and Deploy** — Cloudflare builds and deploys automatically.

### Step 4 — Connect Custom Domain
1. In your Pages project → **Custom domains** → **Set up a custom domain**
2. Enter `dutch-iptv.cc`
3. Cloudflare will add the DNS record automatically if your domain is on Cloudflare DNS
4. If domain is on an external registrar, point a CNAME record to `your-project.pages.dev`

### Step 5 — Future Deployments
Every `git push` to `main` automatically triggers a new Cloudflare Pages build. Zero extra steps.

---

## 4. SEO Optimization Checklist

### ✅ Already Implemented
- [x] `<title>` tag with primary keyword "Dutch IPTV"
- [x] Meta description with secondary keyword "beste iptv"
- [x] Canonical URL: `https://dutch-iptv.cc`
- [x] `og:title`, `og:description`, `og:image`, `og:locale` (nl_NL)
- [x] Twitter Card meta tags
- [x] Schema.org JSON-LD (Organization + AggregateOffer)
- [x] `lang="nl"` on all pages
- [x] Semantic HTML (`<h1>`, `<h2>`, `<h3>`, `<article>`, `<nav>`, `<footer>`)
- [x] Single `<h1>` per page
- [x] `alt=""` + `title=""` on all images
- [x] `loading="lazy"` on below-fold images, `loading="eager"` + `fetchpriority="high"` on hero
- [x] Auto-generated `sitemap.xml` via `@astrojs/sitemap`
- [x] Keywords in URL slugs (`/privacy`, `/voorwaarden`, etc.)
- [x] Internal linking between all pages
- [x] FAQ schema-ready markup (`<details>` / `<summary>`)
- [x] `rel="nofollow"` on buy buttons (affiliate hygiene)
- [x] Zero render-blocking JS (Astro ships 0 JS by default)

### ⚡ Do After Deploying
- [ ] Submit `https://dutch-iptv.cc/sitemap-index.xml` to **Google Search Console**
- [ ] Submit to **Bing Webmaster Tools**
- [ ] Add **FAQ schema** JSON-LD (wrap your FAQ in `FAQPage` schema for rich results)
- [ ] Create an OG image at exactly `1200×630px` and put it at `public/dutch-iptv-hero.png`
- [ ] Set up **Cloudflare Analytics** (free, no cookies, GDPR-compliant)
- [ ] Enable **Cloudflare Page Speed** rules (auto-minify, Brotli compression)

### 🔧 Quick FAQ Schema Snippet
Add inside `BaseHead.astro` to get FAQ rich results in Google:

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "Wat is Dutch IPTV?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Dutch IPTV is een premium IPTV-dienst met meer dan 10.000 live zenders."
      }
    }
  ]
}
</script>
```

---

## 5. Quick Reference — File Map

```
src/
├── components/
│   └── BaseHead.astro      ← SEO: title, meta, OG, schema, favicon, canonical
├── layouts/
│   ├── Layout.astro        ← Nav, footer (social icons), WhatsApp FAB
│   └── LegalLayout.astro   ← Legal pages: promo sidebar, WhatsApp FAB
├── pages/
│   ├── index.astro         ← Homepage: hero, features, pricing, referral, FAQ, contact
│   ├── contact.astro       ← /contact page
│   ├── privacy.astro       ← /privacy
│   ├── voorwaarden.astro   ← /voorwaarden
│   ├── terugbetaling.astro ← /terugbetaling
│   ├── disclaimer.astro    ← /disclaimer
│   └── cookies.astro       ← /cookies
└── styles/
    └── global.css          ← Design tokens, animations, glassmorphism
public/
├── favicon.svg             ← Browser tab icon (SVG)
├── favicon.png             ← Fallback favicon
├── apple-touch-icon.png    ← iPhone/iPad home screen icon
├── dutch-iptv-hero.png     ← Hero image
└── beste-iptv-lifestyle.png← Features section image
```
