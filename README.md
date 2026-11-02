# My Solutions: website package

Static HTML/CSS/vanilla JS site, exactly five pages: `index.html`, `managed-wifi-hotels.html`, `labor-camp-wifi.html`, `managed-wifi-enterprise.html`, `wifi-cctv-amc.html`. No build step or dependencies.

## Before going live
1. **Logo:** the official `Solutions By Surfway logo.png` was not available when this was built. `assets/logo/surfway-logo.png` is a text stand-in (blue SOLUTIONS, red SURFWAY, slogan). Copy the official PNG into `assets/logo/` and change the `src` of the two `<img>` logo tags (header and footer) in each page. Also replace the `og:image` URL with a PNG/JPG (about 1200x630) for social sharing.
2. **Quote form:** set `FORM_ENDPOINT` at the top of `assets/js/main.js` (see below). Until then the form validates and offers to send the details via WhatsApp; it never claims delivery.
3. **Arabic:** have a native speaker review the Arabic copy before launch.
4. **Facts:** confirm the project-experience wording, SIRA wording, and the pricing terms with the business.

## Preview locally
`cd solutions-by-surfway-website && python3 -m http.server 8080`, then open http://localhost:8080.

## Deploy to Netlify
Drag the folder onto https://app.netlify.com/drop, or connect a Git repo (build command empty, publish directory `.`).

## Connect solutionsbysurfway.com and HTTPS
Netlify: Site configuration > Domain management > Add a domain > `solutionsbysurfway.com`. Either set the registrar's nameservers to Netlify's, or add the A/ALIAS and CNAME records Netlify shows. Then Domain management > HTTPS > Verify DNS and Provision certificate (Let's Encrypt, automatic). Enable "Force HTTPS". Canonical URLs already use `https://solutionsbysurfway.com/`.

## English / Arabic switch
Client-side, no extra pages. English is the default. Every translatable element carries a `data-ar="..."` attribute holding the Arabic text; its English text is the element's content. `assets/js/main.js` swaps them, sets `<html lang dir>` (`ar`/`rtl`), updates the title and meta description, and stores the choice in `localStorage` so it persists across all five pages. Because both languages share one URL, there are no hreflang tags (they require separate URLs), and crawlers that do not run JavaScript see English only; the Arabic keywords are in a meta keywords tag. If Arabic search traffic becomes a priority, add language-specific URLs and hreflang later.
To edit copy: change the English text and its `data-ar` together. The pages are generated from `source/build.py` (optional): edit there and run `python3 source/build.py` from the folder above, or edit the HTML directly.

## Update pricing and services
Prices live in `pkgs()` in `source/build.py` and appear in the generated HTML in `index.html` and `managed-wifi-enterprise.html` (search for `AED`), and in the quote form's service dropdown (`SVC_OPT`, on every page). The contract terms list is in the same function, in English and Arabic.

## Quote form backend
In `assets/js/main.js` set `FORM_ENDPOINT: "https://..."`. The form POSTs JSON (all field values). Works with Formspree, a Zapier/Make webhook, or a CRM endpoint that accepts JSON. For Netlify Forms you need `data-netlify` markup and a form-encoded POST, which would require a small change.

## WhatsApp and click-to-call
WhatsApp: `https://wa.me/971554238436?text=<URL-encoded message>`. Each page's links carry `data-wa-en` / `data-wa-ar` messages, and the script builds the encoded URL in the active language. Call: `tel:+971554238436`. Both work without a backend. To change the number, search and replace `971554238436` / `+971 55 423 8436`.

## Sitemap
`sitemap.xml` lists the five URLs. When you add, rename or remove a page, edit the `<url>` entries and update `lastmod`; `robots.txt` already points to it. Submit it in Google Search Console.

## Assets and licences
No stock photos or third-party assets are used. Icons are inline SVG and the fonts are system fonts. No client or vendor logos are used.


## Dedicated page URLs
- Essential Wi-Fi: `/packages/`
- Business Wi-Fi: `/packages/business-wifi/`
- Professional Wi-Fi: `/packages/professional-wifi/`
- RADIUS: `/radius/`
- SIRA: `/sira/` (lowercase URL)
- About: `/about/`
- Contact: `/contact/`

The obsolete `/packages/managed-wifi-enterprise.html` URL redirects to `/packages/`.
Netlify `_redirects` is included for lowercase/canonical route handling.
