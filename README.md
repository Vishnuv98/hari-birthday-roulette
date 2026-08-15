# Hari's Birthday Website

A single-file interactive birthday experience (`index.html`). No build step, no dependencies — open it in a browser or host it anywhere that serves static files.

## Pages

The site is one HTML file with five "pages" (each a `<div class="page">`), shown one at a time by toggling an `active` class:

| Page ID | What it is |
|---|---|
| `page-cover` | Opening greeting screen with a start button |
| `page-wheel` | Casino/roulette scene — spin to reveal 5 mystery gift bags |
| `page-europe` | Travel-themed scene |
| `page-wishes` | Wall of birthday wishes submitted by friends (pulled from Google Sheets) |
| `page-friend-submit` | A standalone form for friends to submit a wish — reached via `?wish=1` |

## Navigation flow

1. **Cover → Wheel**: the start button calls `enterBirthdayExperience()`, which plays a transition and moves to `page-wheel`.
2. **Wheel → Europe**: after all 5 gift bags are opened, a "Click Next" button appears and calls `goToPage('page-europe')`.
3. **Europe → Wishes**: the "See Wishes" link calls `openWishesDirect()`, switching straight to `page-wishes` and loading the wishes wall.

`page-friend-submit` is separate from this flow. Any link with `?wish=1` in the URL removes every other page from the DOM and shows only the wish form — that's the link to send to friends.

## Configuring the gift bags

Each bag's giver, message, and (optional) photo live in the `giftData` object (search for `const giftData` in the script):

```js
1: {
  giver: "LYNN",
  initials: "L",
  photo: "",   // paste an image URL or base64 data URI here
  message: "Happy birthday, Hari! ..."
}
```

- Bags 1–4 are shown as a mystery ("A Mystery Someone") until revealed, then display the giver's name and message.
- Bag 5 is always the last one spun (see `spinOrder`) and is treated as the "jackpot" — it shows the giver, message, and photo immediately with a side-by-side layout (message on one side, photo on the other) so nothing needs to scroll. That side-by-side layout is controlled by the `with-photo` class, which is currently only applied when `num === 5` in `openMessageOverlay()`. To give another bag a photo too, add its `photo` value in `giftData` and update that condition.
- `cardRevealText` (bag 5 only) is the text shown on the grid card after it's revealed.

## Wishes wall setup

The wishes wall reads from and writes to a Google Sheet via a Google Apps Script Web App. The endpoint is set once, near the top of the script:

```js
const WISHES_API_URL = "https://script.google.com/macros/s/XXXXXXXX/exec";
```

For this to work:
1. The Apps Script deployment's **Who has access** must be set to **Anyone** (not "Anyone with Google account" or "Only myself").
2. Any time the script code changes, it needs a **new deployment version** (Deploy → Manage deployments → Edit → New version) — the `exec` URL always serves whatever was last deployed, not your latest saved edit.
3. You can sanity-check it any time by pasting the `WISHES_API_URL` into a browser tab — it should return a plain JSON array, not a sign-in page or error page.
4. **file://** doesn't work for this — opening the downloaded HTML file directly (double-click) blocks the fetch call. Host it somewhere (GitHub Pages, Netlify, etc.) or use a real link.

The friend-submission form (`page-friend-submit`, fields `#friendName` / `#friendMessage`) sends a GET request to the same URL with `name` and `message` query params to add a new wish.

## Responsive design

The layout has been tested and fixed to work cleanly from small phones up through large desktops — roughly 320px to 1920px wide, and down to short windows like the common 1366×768 or 1280×720 laptop resolutions. No horizontal scrollbars, and every overlay (gift reveal, wishes) fits within the viewport without needing to scroll to reach its buttons.

## Hosting

Since it's a single static file, any static host works: GitHub Pages, Netlify, Vercel, or a plain web server. Just make sure the Apps Script deployment (above) is reachable over HTTPS from wherever you host it.
