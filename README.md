# Daily Dashboard

A single-page browser dashboard that shows your day at a glance: weather, stocks, unread email, calendar, news on topics you track, reminders, notes, and project summaries. Live data refreshes in the browser, and a full rebuild runs once a day through Claude Cowork.

## What It Does

- Stocks: refresh every 5 minutes via Yahoo Finance (free, no API key)
- Weather: refresh every 10 minutes via Open-Meteo (no API key), with three cities you can flip between
- Email: refresh every 30 minutes via a Cowork-side Gmail pull, color-coded by account
- Calendar, news, reminders, and project summaries: rebuilt once a day by Cowork
- Notes: auto-saved in the browser and persistent across sessions
- News topics: fully configurable
- Daily extras: ticker, riddle, and a daily prompt

## Requirements

- The Claude Cowork desktop app
- An Anthropic plan with enough usage for daily Cowork runs
- A connected Gmail account and Google Calendar (Cowork Settings, then Connectors)
- A modern browser (Chrome, Edge, Firefox, or Safari)

## Setup

1. Clone or download this repository.
2. Copy dashboard_template.html into your dashboard folder and rename it to dashboard_today.html.
3. Copy emails.example.js to emails.js (the page loads email through a script tag, which avoids the browser blocking local file requests).
4. Connect Gmail and Google Calendar in Cowork.
5. Open dashboard_today.html in your browser.

## How It Works

Live tiles (stocks, weather, email) refresh on their own timers from the browser. Once a day, Cowork runs the tasks in scheduled-tasks/ to rebuild the calendar, news, reminders, and project summaries, and writes the results back into the page.
