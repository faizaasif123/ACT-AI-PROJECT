# Seally — Verified Handmade Shops

A single-file, front-end-only web app that acts as a directory of "verified" handmade/small businesses. It's built as one self-contained `index.html` with inline CSS and vanilla JavaScript (no build step, no backend, no framework).

## What it does

- **Directory of shops** — Browse a list of handmade businesses (candles, crochet, baskets, home decor, bath & body, etc.), each with a description, product list, and contact/social links.
- **Verification workflow** — Businesses can "Register" (submit an application), which goes into a **pending** queue. An **Admin** panel (demo passcode `admin123`) can approve (issuing a certificate number like `V-0001`) or reject applications.
- **Shop detail pages** — Each verified shop has its own page showing owner, contact info, socials (Instagram/Facebook/WhatsApp), website, and products.
- **"AI-Powered" Authenticity Checker** — A page where you type in a business name and get a score (0–100%) based on:
  - Whether it matches an existing entry in the local directory
  - A simulated "social media presence" check
  - An AI text analysis step (calls a Gemini API endpoint if a key is configured, otherwise falls back to a simple heuristic/keyword scorer)
  - A "name quality" heuristic (casing, length, keywords like "studio"/"collective")
- **Reporting** — A simple form to report a suspicious listing.

## How it's built

- **Single HTML file**: everything (styles, markup, and logic) lives in one `.html` file.
- **Fonts**: Google Fonts (Fraunces, Work Sans, IBM Plex Mono) loaded via CDN link tags.
- **No framework**: rendering is done by hand — a `currentView` state variable and a `render()` function that swaps `innerHTML` in a `#view-root` container based on the view (`home`, `browse`, `detail`, `authenticity`, `register`, `registered`, `admin`, `report`).
- **Client-side "database"**: business listings and reports are stored in the browser's `localStorage` (keys `seally_businesses` and `seally_reports`). On first load, if nothing is stored, it seeds 5 example businesses.
- **Routing**: not a real router — clicking nav buttons or elements with `data-action="nav"` just updates a JS variable and re-renders.
- **SVG seal**: a "Verified Shop" circular stamp is generated dynamically as inline SVG (`sealSVG()`), reused on the home page hero, shop cards, and detail pages.

## Known issues / things to flag before using this anywhere real

1. **Hardcoded API key in client-side code.** `CONFIG.GEMINI_API_KEY` contains what looks like a live Google Generative Language API key, shipped directly in the HTML/JS that runs in the browser. Anyone who views source or opens dev tools can see and use it. This should be removed and the key rotated/revoked immediately — API calls to third-party services like this need to go through a backend, never be embedded in client-side code.
2. **The "authenticity checker" isn't a real verification tool.** The "social media presence" check (`checkSocialMediaPresence`) doesn't actually contact Instagram, Facebook, or Google — it fabricates a pass/fail based on string length and whether the name contains words like "test" or "123". The Gemini call, when it works, just asks the model to judge a business name in isolation (no real lookup), and there's a keyword-based fallback that behaves the same way. As shipped, this feature can produce a confident-looking "87% verified" score for a business that doesn't exist, and there's a risk it could be trusted as a genuine trust/safety signal when it isn't one.
3. **No real backend/persistence.** Everything lives in `localStorage` on one device/browser — it's not shared between users, isn't an actual database, and would need a real backend (auth, storage, and genuine verification checks) before this could function as an actual trust-and-safety product.
4. **Admin auth is a hardcoded client-side passcode** (`admin123`) checked in JavaScript — trivial to bypass by reading source, so it's demo-only and not real access control.

## Tool Used
- Idea from myself + chagpt
- claude for basic front end
- google gemini api key
- google stich for idea enhncing
- deepseek for updation and chnages




## Running it

Just open the HTML file in a browser — no server or build step required. To reset the seeded data, clear `localStorage` for the page (or open dev tools → Application → Local Storage → delete `seally_businesses` and `seally_reports`).


## Deployed at

https://act-ai-project-omega.vercel.app/
