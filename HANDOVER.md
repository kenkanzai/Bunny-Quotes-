# Bunny Quotes — Handover

Read this first, before any change to the app.

App file: `index.html` (was `bunny-quotes.html`; opens in Chrome on phone or PC).
App files (v4.3): `index.html`, `manifest.json`, `sw.js`, `icon-192.png`, `icon-512.png`, `icon-maskable-512.png`.
Live link: https://kenkanzai.github.io/Bunny-Quotes-/ (GitHub repo kenkanzai/Bunny-Quotes-, Pages from main / root).
Owner: Christopher.
Last update: 29 October 2026 (v4.3).

---

## 1. What the app is for

Find existing cute bunny GIFs, stickers and pictures from the internet, write a quote **directly on** the GIF or picture, and send it to someone through Messenger, WhatsApp or any message app.

## 2. Owner's rules for this app (do not break)

1. Bunnies come from the internet (existing GIFs, stickers, pictures). **Never draw or create bunnies.**
2. The main source of bunnies is always the internet. **Never** make the user add his own GIFs or pictures to get bunnies. His own photo is an optional extra (Use my photo), added at his request in v4.2.
3. The quote is written **on** the GIF or picture itself. A GIF must stay animated after the quote is added.
4. The quote must **match the meaning** of the bunny (same theme). See section 5.
5. Send must reach Messenger, WhatsApp and any message app (phone share menu).
6. Quotes on the GIF use the plain apostrophe `'`, not the curly one `’` (the Cute font has no curly apostrophe and shows a gap).
7. General working rules:
   - If his message has a question, answer it first and wait for his reply before changing anything.
   - Change only what he asks. Nothing extra.
   - If a request is unclear, ask one line before changing anything.
   - After every change, say what it was checked against. Say so if it could not be tested.
   - Give a direct answer first, then detail.

## 3. Screens

**Today**
- One theme per day: 5–11 am Good morning, 9 pm–5 am Good night, other hours rotate through the other 8 groups by day.
- Shows a GIPHY GIF for that theme with a quote from the same group. Theme name shows after the date.
- Buttons: Another bunny (same theme), Another quote (same group), Edit, Send.
- Today's bunny is cached for the day so opening the app does not use searches.

**Find**
- Search box. A word without "bunny" or "rabbit" gets "bunny " added (e.g. "good morning" → "bunny good morning").
- 20 topic buttons, 3 types: GIFs, Stickers, Photos (Photos need a Pexels key).
- Loads 50 at a time, more load on scroll (Load more button as fallback). No repeats in one list. GIPHY stops at about 500 results per search.
- **Use my photo:** opens camera, gallery or files. His own photo or GIF goes into the same Editor. Own GIFs stay animated. Big photos are reduced to 1600 px on the longest side so they send easily. No quote theme is set for own photos; the last quote group is used.
- Tap a bunny → Editor.

**Editor**
- Live preview with the quote on the bunny.
- Quote: 10 groups, Another quote, or type your own (max 120 characters).
- Font: Cute (Gaegu), Round (Fredoka), Script (Pacifico), Hand (Patrick Hand), Bold (Nunito).
- Color: white, pink, yellow, mint, blue, plum, black. Outline turns dark or white automatically.
- Size: S, M, L. Place: Top, Middle, Bottom. Last style is remembered.
- Save (downloads the file) and Send (opens the share menu; if not available, saves to Downloads with a message).

**Settings**
- GIPHY key and optional Pexels key.

## 4. Keys and limits

- GIPHY beta key: free, **100 searches per hour**. Created on developers.giphy.com under the name "Ken" (platform Web).
- The key is built into the file (`DEFAULT_GIPHY` in the script). Anyone with the file uses the same 100 per hour. If it leaks: delete it on the GIPHY dashboard, create a new one, paste it in Settings or replace `DEFAULT_GIPHY`.
- Pexels key: optional, for Photos only. Not set yet.
- When the hourly limit is reached, the app shows: "You've used this hour's 100 searches."

## 5. Quote ↔ bunny matching

10 quote groups, 8 quotes each (80 total): Good morning, Good night, Love, Miss you, Hi, Cheer up, Thank you, Sorry, Birthday, Funny.

Today uses these GIPHY searches per group:

| Group | Search |
|---|---|
| Good morning | bunny good morning |
| Good night | bunny good night |
| Love | bunny love |
| Miss you | bunny miss you |
| Hi | bunny hi |
| Cheer up | bunny hug |
| Thank you | bunny thank you |
| Sorry | bunny sorry |
| Birthday | bunny birthday |
| Funny | funny bunny |

Find topics → group (by keywords in the search word):

| Topic | Group |
|---|---|
| bunny hi | Hi |
| bunny love, bunny kiss | Love |
| bunny hug, bunny sad | Cheer up |
| bunny good morning | Good morning |
| bunny good night, sleepy bunny | Good night |
| bunny miss you, bunny bye | Miss you |
| bunny thank you | Thank you |
| bunny sorry | Sorry |
| bunny birthday | Birthday |
| bunny dance, bunny happy, bunny eating | Funny |
| cute bunny, kawaii bunny, baby bunny, fluffy rabbit | from the GIF's own title; "Hi" if the title has no clue |

Limit: matching depends on how GIPHY tags its GIFs. A GIF may sometimes not fit; Another bunny gives a new one.

## 6. How the quote is written on a GIF

- Download the GIF (GIPHY `downsized_medium`, else `downsized`, else `original`).
- Read every frame (gifuct-js), draw the frame, draw the quote, rebuild the GIF (gifenc). Max width 480 px. Transparent stickers get a white background.
- Photos (Pexels `large`): draw picture + quote, save as JPG.
- Both libraries are bundled inside the HTML file (no internet needed for them).

## 7. Where things are in the code

| Part | Look for |
|---|---|
| Quotes | `const QUOTES=` |
| Topic buttons | `const TOPICS=` |
| Group → Today search | `const TOPIC_OF=` |
| Search word → group | `const KEYWORDS=` |
| Fonts, colors, sizes | `const FONTS=`, `COLORS`, `SIZES` |
| Built-in GIPHY key | `const DEFAULT_GIPHY=` |
| Quote on screen | `styleOverlay` |
| Quote on the file | `drawQuote`, `makeFile` |
| Send / Save | `sendItem`, `download` |
| Use my photo | `ownBtn`, `ownFile` |

Saved on the device (browser storage): `giphyKey`, `pexelsKey`, `style`, `todayCache`.

## 8. Not tested yet

- Send on a real phone (share menu with WhatsApp / Messenger). Only PC screens have been seen.
- Photos tab (no Pexels key yet).
- Use my photo: camera and gallery on a real phone.
- Writing the quote on a live GIPHY GIF was not run in the build workspace (no internet there). It needs a check on the device.

## 9. Version history

| Version | Change | Owner's response |
|---|---|---|
| v1 | Drawn bunnies with quotes | Rejected: wanted real GIFs from the web |
| v2 | User adds own GIFs/pictures | Rejected: wanted GIFs from the internet |
| v3 | GIPHY search, quote written on GIF, Send/Save | Accepted; key setup done |
| v3.1 | GIPHY key built into the file | — |
| v3.2 | Apostrophe gap fixed ("it' s" → "it's") | — |
| v4 | Refine all: layout, quote style options, 80 quotes in 10 groups, 20 topics, scroll loading | — |
| v4.1 | Quote matches the bunny's theme (Today and Find) | — |
| v4.2 | Use my photo: quote on his own photo or GIF | Requested |
| v4.3 | Installable app (PWA): manifest, service worker, 🐰 icon; 2 lines added to the HTML. App features unchanged | Requested |
