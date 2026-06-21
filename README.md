
# 🏎️ F1 AI Analyst

> An automated Formula 1 analysis and content pipeline that turns raw race data into AI-generated insights, delivered across Discord, Telegram, Facebook, and Gmail — without a human touching it on race weekend.

![n8n](https://img.shields.io/badge/automation-n8n-EA4B71?style=flat-square)
![Claude](https://img.shields.io/badge/AI-Claude%20(Sonnet%20%2B%20Haiku)-D97757?style=flat-square)
![Supabase](https://img.shields.io/badge/database-Supabase-3ECF8E?style=flat-square)
![Status](https://img.shields.io/badge/status-active-success?style=flat-square)

---

## Overview

**F1 AI Analyst** is an end-to-end automation pipeline that monitors Formula 1 data, generates AI-powered analysis and commentary, and publishes it automatically across multiple platforms. It pulls live and historical F1 data, runs it through Claude for analysis, stores results in Supabase, and distributes the final content wherever fans are — Discord servers, Telegram channels, a Facebook page, or directly to an inbox.

The pipeline is fully orchestrated in **n8n**, with logic that adapts depending on whether it's a race day or a regular day.

---

## ✨ Features

- **Race-day awareness** — branching workflow logic that behaves differently depending on whether a session/race is happening, vs. quieter days between race weekends.
- **AI-generated analysis** — uses Claude Sonnet for deeper race breakdowns and Claude Haiku for lighter, faster tasks (e.g. summaries, formatting), balancing quality and cost.
- **Live F1 data** — pulled from the Jolpica-F1 API (results, standings, schedules, session data).
- **Persistent storage** — Supabase backs the pipeline with upsert logic to avoid duplicate posts across runs.
- **Multi-platform publishing** — auto-distributes content to:
  - 💬 Discord (webhook)
  - 📨 Telegram (bot)
  - 📘 Facebook Page
  - 📧 Gmail (email digest)

---

## 🏗️ Architecture

```
Jolpica-F1 API
      │
      ▼
 n8n Scheduled Trigger
      │
      ▼
 Race Day? ──No──► Standings/News Digest Branch
      │
     Yes
      │
      ▼
 Live Session/Results Branch
      │
      ▼
 Claude (Sonnet/Haiku) — Analysis & Copy Generation
      │
      ▼
 Supabase — Upsert / Deduplication
      │
      ▼
 ┌─────────────┬─────────────┬─────────────┬─────────────┐
 │   Discord   │  Telegram   │  Facebook   │    Gmail    │
 └─────────────┴─────────────┴─────────────┴─────────────┘
```

---

## 🛠️ Tech Stack

| Layer              | Tool / Service                              |
|---------------------|----------------------------------------------|
| Workflow automation | [n8n](https://n8n.io)                        |
| AI / Analysis       | Anthropic Claude API (Sonnet + Haiku)        |
| F1 Data Source      | [Jolpica-F1 API](https://github.com/jolpica/jolpica-f1) |
| Database / Storage  | [Supabase](https://supabase.com) (Postgres)  |
| Publishing Channels  | Facebook Graph API, Discord Webhooks, Telegram Bot API, Gmail API |

---

## ⚙️ How It Works

1. **Trigger** — n8n runs on a schedule and checks the current F1 calendar.
2. **Branch logic** —
   - **Race day:** pulls live session/results data and generates real-time recaps and reactions.
   - **Non-race day:** pulls standings, news, and upcoming schedule data for digest-style content.
3. **AI processing** — raw data is passed to Claude, which generates the actual analysis, summaries, and platform-ready copy.
4. **Storage** — processed content is upserted into Supabase to prevent duplicate or repeated posts.
5. **Distribution** — final content is pushed out to Discord, Telegram, Facebook, and/or Gmail depending on the content type.

---

## 🚀 Getting Started

### Prerequisites

- An n8n instance (self-hosted or n8n Cloud)
- An Anthropic API key
- A Supabase project (URL + service key)
- Access to the Jolpica-F1 API
- Platform credentials:
  - Discord webhook URL
  - Telegram bot token + chat ID
  - Facebook Page access token
  - Gmail API credentials (OAuth)

### Setup

1. Clone this repository.
2. Import the workflow JSON file into your n8n instance.
3. Add the required credentials in n8n's credential manager (Anthropic, Supabase, Discord, Telegram, Facebook, Gmail).
4. Set the environment variables below.
5. Activate the workflow.

### Environment Variables

| Variable | Description |
|---|---|
| `ANTHROPIC_API_KEY` | Claude API key |
| `SUPABASE_URL` | Supabase project URL |
| `SUPABASE_KEY` | Supabase service role key |
| `DISCORD_WEBHOOK_URL` | Discord channel webhook |
| `TELEGRAM_BOT_TOKEN` | Telegram bot token |
| `TELEGRAM_CHAT_ID` | Target Telegram chat/channel ID |
| `FACEBOOK_PAGE_TOKEN` | Facebook Page access token |
| `GMAIL_CLIENT_ID` / `GMAIL_CLIENT_SECRET` | Gmail OAuth credentials |

---

## 🧪 Engineering Notes

A few real challenges worked through while building this:

- Handling API authentication reliably across multiple external services within n8n.
- Working around n8n's node expression limitations for conditional logic.
- Designing clean branching logic to separate race-day and non-race-day flows.
- Resolving Supabase upsert conflicts to keep data consistent across repeated runs.

---

## 🗺️ Roadmap

- [ ] Add post-race driver/team sentiment analysis
- [ ] Expand to WhatsApp/X (Twitter) publishing
- [ ] Add a lightweight dashboard for run history and content review

---

## 👤 Author

**Muhammad Hashir Iftikhar**
BS Artificial Intelligence,
email muhammadhashiriftikhar@gmail.com

---

## 📄 License

This project is licensed under the MIT License — feel free to adapt for your own use.
