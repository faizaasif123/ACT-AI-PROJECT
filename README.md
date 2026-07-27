# Seally — Verified Handmade Shops

**A trust layer for small handmade businesses.** Seally is a directory where independent makers (candle-pourers, crocheters, basket weavers, potters, soap-makers, and other handmade sellers) can get a verified badge before they're listed publicly, so buyers can shop from strangers online with more confidence.

---

## a) What it does & the problem it solves

Small handmade businesses mostly sell through Instagram DMs, WhatsApp, and word of mouth. There's no formal storefront, no reviews platform, and no easy way for a first-time customer to tell a real, established maker apart from a fake page set up to collect advance payment and disappear.

**The problem:** Buyers have no fast, trustworthy way to check "is this handmade shop legit?" before sending money. Sellers, meanwhile, have no way to prove legitimacy beyond their own Instagram follower count.

**Who it's for:**
- **Buyers** who want to shop small/handmade but are wary of being scammed by a page with no track record.
- **Sellers/makers** who are legitimate and want a badge of trust that sets them apart from copycats and scam pages using similar names or photos.

**How Seally solves it:**
1. A maker submits their business, contact details, and social handles.
2. An admin reviews the submission (checking it against the actual social pages/contact info) before approving it.
3. Approved businesses get a **certificate number** (e.g. `V-0004`) and a **verified seal**, and appear in the public directory with their products, contact info, and social links.
4. Anyone — buyer or not — can look up a business name in the **Authenticity Checker** to see if it's verified, pending, or unknown, and can **report** a listing that seems suspicious.

---

## b) Live deployed URL

> **⚠️ Not yet deployed to a public URL.**
> This build was authored and tested inside a sandboxed environment that can't push to public hosting, so there is no live link to share yet. The screenshots in section (f) were captured by running the actual `index.html` file in a real browser (Chromium via Playwright), so what you see below is exactly what the deployed app looks like — it just isn't hosted publicly yet.
>
> **Fastest ways to get a real live URL (2 minutes, no build step needed since it's a single HTML file):**
> - **Netlify Drop** — go to https://app.netlify.com/drop and drag `index.html` onto the page. You get a live `https://your-app.netlify.app` URL instantly.
> - **GitHub Pages** — push the file to a GitHub repo, then in **Settings → Pages** set the source to the `main` branch. Your app will be live at `https://<username>.github.io/<repo>/`.
> - **Vercel** — `vercel deploy` from the folder containing `index.html` (via the Vercel CLI or by dragging the folder into vercel.com/new).
>
> Once deployed, replace this section with the real link, e.g.:
> `🔗 https://seally.netlify.app` — *(placeholder — swap in your actual URL after deploying)*

---

## c) Features list

**Public / buyer-facing**
- **Home page** — hero pitch, live stats (verified shop count, pending applications, number of craft categories), a 3-step "how verification works" explainer, and featured verified shops.
- **Browse Shops** — full directory of verified businesses with free-text search (by name, craft, or city) and category filter chips (Candles, Crochet & Knitting, Custom Baskets, Home Decor, Bath & Body, Other Handmade).
- **Shop detail page** — full description, product list with prices, owner name, email, phone, WhatsApp/Instagram/Facebook links, website, certificate number, and verification date.
- **Authenticity Checker** — type in any business name and get a composite trust score (see AI section below).
- **Report a listing** — a form to flag a suspicious or impersonating shop, with an optional contact field for follow-up.
- **Verified seal** — a custom circular SVG "stamp" (generated in code, not an image asset) shown on cards, detail pages, and the hero section.

**Seller-facing**
- **Register your business** — a full application form: business name, category, city, owner name, description, email/phone, WhatsApp, Instagram, Facebook, website, and up to 5 products (name, price, short description), with the ability to add/remove product rows dynamically.
- **Application confirmation screen** after submitting, explaining the shop is now pending review.

**Admin-facing**
- **Passcode-gated admin panel** (demo passcode) with three tabs: **Pending**, **Verified**, **Rejected**.
- Each pending application shows all submitted details (owner, city, email, phone, social handles to check, description) with **Approve & stamp verified** (auto-issues the next certificate number) or **Reject** actions.

**Cross-cutting**
- Fully responsive layout (collapses to single-column on mobile).
- Toast notifications for actions (sign-in, verify, reject, report submitted, etc.).
- All data persists in the browser via `localStorage`, seeded with 5 example businesses on first load so the directory isn't empty out of the box.

---

## d) The AI feature

**Feature name:** AI-Powered Store Authenticity Checker (the "Check Store" tab).

**What it does:** given any typed-in business name, it returns an overall trust score (0–100%) and a status of **Verified / Partial Match / Suspicious**, built from four weighted sub-checks:

| Check | Weight | What it looks at |
|---|---|---|
| Store Registration | 30% | Does the name match an existing entry in Seally's own directory, and is that entry verified or still pending? |
| Social Media Presence | 25% | A presence signal across Instagram/Facebook/Website/Google |
| AI Business Analysis | 25% | An AI model's read on the business name itself (see prompt below) |
| Name Quality | 20% | Rule-based heuristics: capitalization, length, punctuation, presence of "business-sounding" words like *studio*, *collective*, *craft*, *artisan* |

The result panel shows the score, a pass/warn/fail breakdown per check, the AI model's stated reasoning plus any red/green flags it identified, and a recommendations list (e.g. "not registered — consider applying").

**The exact system/user prompt sent to the AI model:**

```
You are an expert in business verification. Analyze this business name: "<STORE NAME ENTERED BY USER>"
Evaluate: authenticity, professionalism, red flags, quality.
Respond with JSON: {"score": 0-100, "risk_level": "low/medium/high", "reasoning": "...", "red_flags": [], "green_flags": [], "confidence": 0-100}
```

The response is parsed as JSON and folded into the overall score.

**⚠️ Important honesty note about this feature (please read before demoing it as a security tool):**
As currently implemented, only the "AI Business Analysis" quarter of the score is a genuine model call — and even that only judges the *name string in isolation* (it has no way to actually look the business up). The "Social Media Presence" check does **not** contact Instagram, Facebook, or Google at all; it's a simulated result based on string length and whether the name contains words like `"test"` or `"123"`. And if the AI call fails or no API key is configured, the app silently falls back to the same kind of keyword/heuristic scoring for the "AI" check too. In its current state this feature is best understood as a **proof-of-concept UI for what a real verification score could look like**, not a working fraud-detection system — a real version would need genuine API-based lookups (with rate limiting and consent) or human review behind each of these checks.

---

## e) Tools, services, and AI models used to build it

**Building this app:**
- **Claude (Anthropic)** — used to write, structure, and review the HTML/CSS/JS in this project, and to produce this README.
- Plain **HTML5 / CSS3 / vanilla JavaScript** — no framework, no build tool, no bundler.
- **Google Fonts** (Fraunces, Work Sans, IBM Plex Mono) via CDN `<link>` tags.
- Browser **`localStorage`** as the only data store (no backend, no database).

**Inside the app itself (the AI feature):**
- **Google Gemini** (`gemini-2.0-flash-lite`, called via the Generative Language REST API) — powers the "AI Business Analysis" portion of the Authenticity Checker, with a heuristic fallback if the API call fails or no key is present.

**Used to produce this documentation:**
- **Playwright + headless Chromium** — used to actually load `index.html` in a real browser and capture the screenshots below, so they reflect the real running app rather than mockups.

---

## f) Screenshots

**1. Home page** — hero, live stats, and featured verified shops
![Home](screenshots/01_home.png)

**2. Browse Shops** — searchable, filterable directory of verified businesses
![Browse](screenshots/02_browse.png)

**3. Shop detail page** — products, contact info, and social links for one verified shop
![Shop Detail](screenshots/03_detail.png)

**4. Authenticity Checker** — a completed check showing the composite score and per-check breakdown
![Authenticity Checker](screenshots/04_authenticity.png)

**5. Register your business** — the seller application form
![Register](screenshots/05_register.png)

**6. Admin panel** — verification queue after signing in
![Admin](screenshots/06_admin.png)

---

## g) How to run the project

No build step, no dependencies, no server required — it's a single HTML file.

**Option 1 — just open it:**
1. Download `index.html`.
2. Double-click it (or drag it into your browser). Done.

**Option 2 — run it on a local server** (recommended if you plan to test the AI checker, since some browsers restrict `fetch()` calls from `file://` pages):
```bash
# from the folder containing index.html
python3 -m http.server 8000
# then open http://localhost:8000 in your browser
```

**To use the AI Authenticity Checker with a real Gemini response** instead of the offline fallback:
1. Get a free API key from Google AI Studio: https://aistudio.google.com/apikey
2. Open `index.html` in a text editor and find:
   ```js
   const CONFIG = {
       GEMINI_API_KEY: 'YOUR_API_KEY_HERE',
       ENABLE_SOCIAL_VERIFICATION: true,
   };
   ```
3. Replace the placeholder with your own key.
4. **Do not commit or ship a real key in client-side code for anything beyond local testing/demo purposes** — anyone who views page source can read and use it. A production version of this feature should proxy the AI call through a backend so the key is never exposed to the browser.

**Admin login (demo):**
- Go to the **Admin** tab → passcode `admin123`.
- This is a hardcoded client-side check for demo purposes only and is not real access control.

**Resetting the seed data:**
- Open DevTools → Application → Local Storage → delete the `seally_businesses` and `seally_reports` keys, then refresh.
