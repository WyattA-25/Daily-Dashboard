# Scheduled task: morning-dashboard

**Cron:** `0 7 * * *` (every day at 7:00 AM local time)

**Description:** Regenerate the Morning Dashboard daily at 7:00 AM

**Prompt (paste into Cowork when creating the task):**

---

Regenerate the Morning Dashboard for today.

PROJECT FOLDER: [PATH TO YOUR DASHBOARD FOLDER — e.g. C:\Users\YOUR_USER\Documents\Claude\Projects\Daily Dashboard]
USER: [YOUR NAME] ([YOUR_EMAIL@gmail.com])

STEP 1 — Read the existing dashboard_today.html
Preserve ALL CSS, JS structure, and layout. The JS handles weather (auto-detected via Open-Meteo), stocks (auto-refreshed via Yahoo Finance), and emails (fetched from emails.json sidecar). DO NOT remove or replace those scripts. Only update data values inside the HTML.

STEP 2 — Pull live data:
- Stocks: SKIP server-side fetching. Browser auto-refreshes from Yahoo Finance every 5 min. The static `stocks` array in JS is just a fallback for when Yahoo is unreachable — you can update it or leave it.
- Weather: SKIP. Browser fetches Open-Meteo every 10 min.
- Email: Pull via the Gmail MCP connector (search_threads with query "is:unread newer_than:2d -in:draft", pageSize 25). Write the result to emails.json (see STEP 4 for format). Also update the static email items in the HTML as a first-paint fallback.
- Calendar: List events for today, tomorrow, and the rest of the week.
- News: WebSearch for last 36 hours on each tracked topic (data-topic values match these CSS classes):
    * "ai"       — AI / Automation tools, new models, agents
    * "tech"     — Tech industry / startups
    * "compeng"  — Computer / Engineering hardware
    * "3dprint"  — 3D printing tools and materials
    * "camping"  — Camping / outdoors gear
    * "politics" — US politics headlines
    * "gaming"   — Gaming releases and industry. EXCLUDE esports / tournaments / competitive events. Focus on game releases, console news, studio news.
  Aim for 1–2 news items per topic. Each news item uses class `news-item topic-<key>` with `data-topic="<key>"` and matching `tag-<key>` span.

STEP 3 — Update the HTML (only data, not structure):
- Greeting + date
- Banner ("Live: Generated [date]…")
- Quick Stats counts (unread emails, events today, news items, reminders)
- "One Thing to Think About" — fresh reflection, vary topic daily
- Riddle of the Day — new genuine riddle, answer hidden by default
- Reminders based on calendar RSVPs needing action, upcoming appointments
- Email items (fallback rendering — JS overwrites with emails.json data on load)
- Calendar events
- News items
- Right sidebar today's schedule + next-up
- Project cards if user has added named projects

STEP 4 — Write emails.json sidecar:
Also write [PROJECT FOLDER]\emails.json with this exact shape:

```json
{
  "generated_at": "<ISO timestamp now>",
  "accounts": [
    { "id": "acct1", "name": "[YOUR_EMAIL@gmail.com]", "color": "#60a5fa", "short": "[YOUR_HANDLE]" }
  ],
  "unread_count": <integer>,
  "emails": [
    { "account": "acct1", "from": "...", "subject": "...", "preview": "...", "time": "..." }
  ]
}
```

Account colors in slot order: #60a5fa (blue), #fb923c (orange), #a78bfa (purple), #84cc16 (lime), #ec4899 (pink), #14b8a6 (teal), #facc15 (yellow). Add additional accounts to the array as the user connects them. Order emails: real people → security → receipts → recruiters → jobs → social → newsletters → promotional.

STEP 5 — Save:
- Write to dashboard_YYYY-MM-DD.html (today's date) and overwrite dashboard_today.html with same content
- Also write emails.json
- Present the file to the user

TONE: Plain, direct. Empty states for missing data, never fabricate.
