# Scheduled task: refresh-emails

**Cron:** `*/30 * * * *` (every 30 minutes)

**Description:** Refresh emails.json sidecar every 30 min (no HTML rebuild)

**Prompt (paste into Cowork when creating the task):**

---

Pull fresh unread emails and write to emails.json sidecar (not the full HTML).

PROJECT FOLDER: [PATH TO YOUR DASHBOARD FOLDER]
USER: [YOUR NAME] ([YOUR_EMAIL@gmail.com])

STEPS:
1. Use the Gmail connector to search threads with query "is:unread newer_than:2d -in:draft" (pageSize 25).
2. Write the email data to TWO files in [PROJECT FOLDER]:

**emails.js** (this is the file the dashboard actually loads — must be a JavaScript file, not pure JSON, so it can be loaded via `<script src>` and bypass file:// CORS):

```js
window.coworkEmails = {
  "generated_at": "<ISO timestamp now>",
  "accounts": [
    { "id": "acct1", "name": "[YOUR_EMAIL@gmail.com]", "color": "#60a5fa", "short": "[YOUR_HANDLE]" }
    // Add more accounts when connected; slot colors in order: blue, orange, purple, lime, pink, teal, yellow (max 7)
  ],
  "unread_count": <integer total unread>,
  "emails": [
    {
      "account": "acct1",
      "from": "<sender display name>",
      "subject": "<full subject>",
      "preview": "<one-sentence summary or snippet>",
      "time": "<human-friendly relative time e.g. '2h ago', 'Yesterday 3:16 PM', 'Thu 11:31 AM'>"
    }
  ]
};
```

KEY: file must START with `window.coworkEmails = {` and END with `};` (semicolon).

**emails.json** (legacy/debug — same object without the JS wrapper):

```json
{
  "generated_at": "...",
  "accounts": [...],
  ...
}
```

ORDER emails by priority: real humans you know → security/system alerts → receipts/orders → recruiters → job alerts → social → newsletters → promotional. Don't include obvious spam.

DO NOT touch dashboard_today.html or any other file. Only emails.js and emails.json. The da