# 🎉 Happy Birthday, Hari! — Interactive Birthday Website

A single-page, animated birthday experience built with plain HTML, CSS, and vanilla JavaScript. No build tools, no dependencies — just open `index.html` in a browser.

## ✨ Features

- **Cover Page** — A warm, animated welcome screen with a hero image and call-to-action buttons.
- **Spin-the-Wheel Gift Reveal** — A roulette-style wheel (with an animated ball) that reveals surprise gifts one at a time, each with a personal message and the giver's identity hidden until "opened."
- **Travel / Europe Page** — A dedicated section for a travel-themed surprise.
- **Birthday Wishes Wall** — Friends' messages are displayed as tappable "bubbles" that burst open to reveal the message. Wishes are fetched live from a connected backend (see below).
- **Friend Submission Link** — A special link (`?wish=1`) that shows *only* the wish-submission form, hiding every other page/section from the DOM so friends can't browse the rest of the site (gifts, wheel, etc.) — they can only leave a message.

## 🗂️ Project Structure

This is a self-contained single file:

```
index.html   → HTML structure, CSS styling, and JavaScript logic all in one file
```

## 🖥️ How to Run

1. Download / clone `index.html`.
2. Open it directly in any modern web browser (double-click, or right-click → "Open with").
3. No server or installation required for local viewing.

For sharing online, upload `index.html` to any static host (GitHub Pages, Netlify, Vercel, etc.).

## 🔗 Sharing the "Leave a Wish" Link

To send friends a link where they can **only** submit a birthday wish (without seeing gifts or other surprises), share the page URL with the query parameter:

```
https://your-site-url/index.html?wish=1
```

When this parameter is present, all other pages are removed from the DOM entirely (not just hidden), so the link is safe to share widely.

## ⚙️ Backend Setup (Wishes Wall)

The wishes wall pulls submitted messages from a **Google Sheet + Google Apps Script Web App**, since a static site (e.g., GitHub Pages) has no built-in database.

1. Create a Google Sheet to store incoming wishes (e.g., columns: `name`, `message`, `timestamp`).
2. Write a Google Apps Script bound to that sheet that:
   - Handles `GET` requests → returns all wishes as JSON.
   - Handles `POST` requests → appends a new wish to the sheet.
3. Deploy the script as a **Web App** (accessible to "Anyone").
4. Copy the deployed Web App URL and paste it into the `WISHES_API_URL` constant near the top of the `<script>` section in `index.html`:

   ```js
   const WISHES_API_URL = "https://script.google.com/macros/s/XXXXXXXX/exec";
   ```

If this URL isn't configured, the wishes wall will show a friendly "not connected yet" message instead of erroring out.

## 🎁 Customizing Gifts

Gift data (giver name, initials, optional photo, and message) is defined in the `giftData` object inside the script. Edit the `message`, `giver`, `initials`, and `photo` fields for each gift number to personalize them. The reveal order is controlled by the `spinOrder` array.

## 🛠️ Tech Stack

- HTML5 & CSS3 (custom animations, gradients, responsive layout)
- Vanilla JavaScript (no frameworks)
- Google Sheets + Apps Script (lightweight serverless backend for wishes)

## 📌 Notes

- Designed to be mobile-friendly and responsive.
- All content is self-contained in one HTML file for easy sharing and hosting.
- Safe-guarded friend link ensures privacy of the main surprise pages.

---

Made with ❤️ to celebrate Hari's birthday.

Wishes Url - > 1786612615112	Vishnu	Happy Birthday to my incredible wife, my partner in everything, and truly my best friend. Thank you for bringing so much love, warmth, and joy into my life every single day. I'm so lucky to walk through life with you. Here’s to celebrating you today and making this year your best one yet!
