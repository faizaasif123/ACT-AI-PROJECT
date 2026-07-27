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
