# kawali.app

Marketing and support site for Kawali, the iPhone app that turns Filipino cooking videos into recipes you can edit and a grocery list. Static HTML with no build step and no third-party requests, served by GitHub Pages at **https://kawali.app**. The app's API lives separately at `api.kawali.app` (DigitalOcean); this site only uses the apex domain.

## About Kawali

Kawali is a recipe app built for Filipino food. Paste a cooking video from YouTube, TikTok, Instagram, Facebook or Pinterest, or share it to Kawali from those apps, and Kawali reads the recipe from the description, the linked recipe page and the narration (English, Tagalog or Taglish). The cook reviews and edits it, saves it, and adds the ingredients to a grocery checklist.

- **Filipino terminology kept.** Ingredient names stay as the source says them (patis, toyo, gata, siling labuyo), with the English beside them only as an explanation. The knowledge base behind this has 160+ ingredients, 30+ cooking terms, 23 dish families and 9 regions.
- **Nothing invented.** An amount, time or serving count the source doesn't state is left blank and flagged, never guessed. A serving count may be estimated from the main ingredient's weight, shown as "~5" until the cook confirms it.
- **Silent videos.** When a video has no written recipe or usable narration, Kawali watches it for the steps and notifies the cook when they're added.
- **Other ways in.** Import Photo (up to four pages, read on the device with Vision), Snap Photo, Paste Text and Make Your Own.
- **Groceries.** Choose what to add, sorted by aisle, the same ingredient added up across recipes when units match, English names on demand, Clear Completed, Print Grocery List.
- **Privacy.** No ads, analytics or tracking. Recipes and groceries live on the device; links and the text read from photos go to Kawali's server (and OpenAI) to be read. The full picture is in `privacy.html`.

Requires iOS 26.2 or later, on iPhone and iPad. Made by CaLa Studios LLC. Questions go to support@calastudios.app.

## Pages

| Path | File | Purpose |
| --- | --- | --- |
| `/` | `index.html` | Landing page |
| `/support` | `support.html` | FAQ and contact; the App Store listing's Support URL |
| `/privacy` | `privacy.html` | Privacy policy; the App Store listing's Privacy Policy URL |
| `/404` | `404.html` | Not-found page |

GitHub Pages serves `support.html` at `/support` (and `/support.html`), so the extensionless URLs work as-is. Terms of Use link to Apple's [Standard EULA](https://www.apple.com/legal/internet-services/itunes/dev/stdeula/); there is no terms page here.

`privacy.html` adds an `embedded` class to `<html>` when it detects it is inside an iframe and hides its header and footer, so the policy alone can be embedded elsewhere.

## Layout

- `assets/css/site.css` – the one stylesheet, shared by every page. Light only, to match the app. Brand colors are the designer's hex values from the app's asset catalog: Chile Rojo `#AE431E` (everything tappable), Terracota `#D06224` (brand mark, big numerals), Olive `#8A8635` (notices; `#6F6B28` for text), Sunset `#EAC891` (hairlines).
- `assets/js/site.js` – mobile nav, scroll reveal, footer year. No dependencies.
- `assets/img/` – app icon sizes, the pan mark (`pan.svg`) and wordmark (`wordmark.svg`, used as a CSS mask) from the app's `Brand` assets, the six App Store screenshots (`shot-*.webp`) and three raw device screens (`screen-*.webp`: home, recipe, groceries) used in the phone frames, the four ingredient sketches from the App Store artwork (`sketch-*.webp`, transparent, tinted through a CSS mask), Apple's App Store badge (for launch, see below) and `og-image.jpg` for link previews.
- `assets/fonts/` – Bitter Medium, Medium Italic, SemiBold and Bold, the app's typeface, subset to Latin and self-hosted as WOFF2 (SIL OFL, license alongside). Body copy uses the system font, as in the app.
- The step, grocery and card icons are the app's own glyphs (`Assets.xcassets/Glyphs`), inlined with `currentColor`. The platform marks are the same Simple Icons (CC0) the app uses.
- `sitemap.xml`, `robots.txt`, `site.webmanifest` – the usual metadata.
- `CNAME` – `kawali.app`, so the custom domain survives redeploys.
- `.nojekyll` – tells Pages to publish the files as they are.

The Home screenshot shows the app's sample recipes, whose photos are from Wikimedia Commons under CC BY and CC BY-SA; the landing page credits them under the screenshots (the same list as `docs/SAMPLE_PHOTO_CREDITS.md` in the app).

## Updating

Edit the HTML, commit to `main`, push. Pages redeploys in about a minute.

Feature copy follows the build that is live in the App Store, not what is in development. The stats in the "A kitchen that speaks Filipino" section come from `backend/kawali_api/data/filipino_terms.json` (ingredients, cookingTerms, dishFamilies) and the `FilipinoRegion` enum; update them when those grow past the next round number. The privacy policy describes the server as it runs today (import cache kept 30 days, notification subscriptions at most 3 days, OpenAI as the interpreter and transcriber); change the policy and its effective date before a change to any of that ships.

To regenerate images: screenshots come from the App Store set (1290×2796 PNG) resized to 720 wide and encoded with `cwebp -q 82`; the `screen-*.webp` phone screens are raw device screenshots (1320×2868) at 600 wide. `og-image.jpg` is a 1200×630 HTML composition rendered with headless Chrome.

### When the App Store listing goes live

1. Replace each `<span class="store-soon" data-store>…</span>` (two in `index.html`) with the official badge:
   ```html
   <a class="badge-link" href="https://apps.apple.com/app/idAPP_ID"><img src="/assets/img/app-store-badge.svg" alt="Download on the App Store" width="162" height="54"></a>
   ```
2. Point every "Get the app" button (`href="#download"` / `href="/#download"`) at the same App Store URL, and change the CTA's "coming soon" line.
3. Add `<meta name="apple-itunes-app" content="app-id=APP_ID">` to `index.html` for Safari's Smart App Banner, and `downloadUrl`, `installUrl` and `offers` to its JSON-LD.
4. Add an "App Store" link to each page's footer.
