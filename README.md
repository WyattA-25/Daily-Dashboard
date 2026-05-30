# Cowork Morning Dashboard

A single-page browser dashboard that gives you your day at a glance — weather, stocks, unread email, calendar, news on tracked topics, reminders, notes, and project summaries. It auto-refreshes the live bits from your browser and gets a full rebuild from Cowork once a day.

![Dashboard screenshot placeholder — add your own]()

## What it does

- **Stocks** — refresh every 5 minutes via Yahoo Finance (no API key)
- **Weather** — refresh every 10 minutes via Open-Meteo (no API key). 3 cities, flip between them with arrows.
- **Email** — refreshes every 30 minutes via a Cowork-side Gmail pull. Color-coded by account, up to 7 accounts.
- **Calendar / news / reminders / project summaries** — rebuilt once a day by Cowork at 7:00 AM
- **Notes** — auto-save to browser localStorage, persistent across sessions, "+ Add a note" creates new notepads per project
- **News topics** — fully configurable. Default set: AI/Automation, Tech, Computer/Engineering, 3D Printing, Camping, Politics, Gaming
- **Ticker, riddle, "one thing to think about"** — refresh daily

## Requirements

- [Cowork](https://claude.com) desktop app installed
- An Anthropic plan with enough usage for daily Cowork runs (Pro is fine; Max 5x is comfortable)
- A connected Gmail account (Cowork Settings → Connectors)
- A connected Google Calendar account
- Chrome, Edge, Firefox, or Safari to view the dashboard

The Outlook / work-email side is optional — leave it unconnected and those slots stay empty.

## Setup — 10 minutes

### 1. Clone or download this repo
```bash
git clone https://github.com/YOUR_USERNAME/cowork-morning-dashboard.git
```
Or just download the ZIP and unpack it somewhere.

### 2. Pick a folder for your dashboard
Anywhere works. A common choice:
```
~/Documents/Claude/Projects/Daily Dashboard
```
Copy `dashboard_template.html` from this repo into that folder and rename it to `dashboard_today.html`. Copy `emails.example.json` and rename it to `emails.json`.

### 3. Open Cowork → create a new project
- Name it whatever (e.g. "Morning Dashboard")
- Point it at the folder you picked in step 2
- Paste the contents of `cowork-project-instructions.md` into the project's custom instructions field
- Fill in the bracketed placeholders (your name, location, email, news topics, stocks, etc.) — they're all listed in the instructions

### 4. Connect Gmail and Google Calendar
In Cowork Settings → Connectors, sign in to:
- Gmail (the address you want triaged)
- Google Calendar (the calendar you want to see)

If you have a work Outlook account, connect that too and tell Cowork to add it to the dashboard.

### 5. Create the two scheduled tasks
In a new chat inside the project, paste these prompts one at a time:

**A. Daily 7 AM rebuild:**
```
Create a scheduled task that runs every day at 7:00 AM. Use the prompt in scheduled-tasks/morning-dashboard.md verbatim.
```

**B. Email refresh every 30 minutes:**
```
Create a scheduled task that runs every 30 minutes. Use the prompt in scheduled-tasks/refresh-emails.md verbatim.
```

Cowork will create both tasks. Click "Run now" on each one so it pre-approves the Gmail / Calendar / WebSearch tools — otherwise the first auto-run might pause waiting for permission.

### 6. Edit the 3 cities for weather
Open `dashboard_today.html` in a text editor. Search for `var CITIES = [`. You'll find:
```js
var CITIES = [
  { name: 'New York, NY',     lat: 40.71, lon: -74.01 },
  { name: 'Chicago, IL',      lat: 41.88, lon: -87.63 },
  { name: 'Los Angeles, CA',  lat: 34.05, lon: -118.24 }
];
```
Replace with your own. The page starts on the middle city; ‹ › arrows cycle.

### 7. Open the dashboard
Double-click `dashboard_today.html`. It opens in your default browser. Bookmark it.

That's it. The first 7 AM run will populate your real data; in the meantime the page works fine with the empty/example states.

## Customization

All these are conversational — just tell Cowork in chat:

| Want to | Say this |
|---|---|
| Add a news topic | "Add cycling to my tracked topics" |
| Remove a topic | "Drop politics from the dashboard news" |
| Add a stock to the ticker | "Add SOFI to the stock ticker" |
| Connect another email | "Connect my work Outlook to the dashboard" |
| Change weather cities | "Change my weather cities to Seattle, Denver, Miami" |
| Change refresh frequency | "Make the email refresh every hour instead of 30 minutes" |
| Add a recurring reminder | "Remind me each Friday morning to send the weekly invoice" |

Cowork updates the project instructions and the scheduled task prompts accordingly. The next 7 AM run reflects the change.

## File layout

```
your-dashboard-folder/
├── dashboard_today.html       # the file you open in your browser
├── dashboard_2026-MM-DD.html  # archived daily snapshots
├── emails.json                # 30-min email feed (auto-updated)
└── (Cowork-managed)
```

## What costs money / what's free

| Service | Cost | Why |
|---|---|---|
| Open-Meteo weather | Free, no key | Public weather API |
| Yahoo Finance stocks | Free, no key | Unofficial chart endpoint |
| Anthropic Claude (Cowork runs) | Your plan's usage | Email pulls, 7 AM rebuild, news searches |
| Gmail / Google Calendar | Free | Your accounts |

Daily token cost on Pro / Max 5x:
- 7 AM rebuild only: ~0.5–1% Pro, ~0.1% Max 5x
- 7 AM + every-30-min email: ~3–6% Pro, ~0.7–1.5% Max 5x

## Privacy

- All your personal data (emails, calendar, location) stays on your machine.
- `emails.json` is plaintext in your project folder. Don't commit it to git (the included `.gitignore` excludes it).
- Browser-side fetches go directly from your machine to Open-Meteo and Yahoo Finance. No proxy server in between.
- Cowork sees your email/calendar through the connectors you authorize. Same trust model as letting any other app read those.

## Troubleshooting

**Weather stuck on "Loading…"** → make sure you opened the file in a real browser (Chrome/Edge/etc.), not a sandboxed preview pane. Check F12 → Console for network errors.

**Ticker shows old prices** → Yahoo Finance occasionally adds CORS restrictions. The page silently falls back to last known prices. If this becomes persistent, tell Cowork "switch the stock ticker to a different source."

**Add Note button doesn't open a prompt** → the inline input bar appears above the button instead of using `window.prompt()`. Click the button → type the name → Enter or click Add.

**Scheduled tasks never fire** → they only run while the Cowork app is open. Closed for 4 hours = one catch-up run when you reopen, not 8 backlogged runs.

**Notes disappeared** → localStorage is per-origin. If you renamed the file or moved it, the browser sees it as a different "site" and gives it fresh storage. Notes survive reloads, restarts, and reboots — just not file moves.

## Credits

Built collaboratively in Cowork. Weather via [Open-Meteo](https://open-meteo.com). Stock data via [Yahoo Finance](https://finance.yahoo.com). Reverse-geocoding via [BigDataCloud](https://www.bigdatacloud.com/).
