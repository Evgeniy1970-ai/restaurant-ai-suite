# 🍽️ Restaurant AI Suite

**7-agent AI automation system for restaurant business operations**  
Built with n8n · Supabase · OpenAI · Telegram · Netlify · Vapi.ai

---

## 📋 Overview

Restaurant AI Suite is a modular, production-grade automation platform designed to streamline daily restaurant operations. It replaces repetitive manual tasks with intelligent AI agents that work 24/7 — monitoring reputation, managing reservations, recommending menu items, tracking inventory, analyzing food photography, reporting analytics, and handling voice calls from guests.

The system is built around a **modular architecture**: each agent operates independently and can be added, removed, or customized based on the specific needs of any restaurant business.

---

## 🏗️ Architecture

```
restaurant-ai-suite/
├── agents/               # n8n workflow files (importable)
│   ├── Agent1_Reputation_Sentiment_PUBLIC.json
│   ├── Agent2_Reservations_Reminders_PUBLIC.json
│   ├── Agent2_Reminder_Scheduler_PUBLIC.json
│   ├── Agent3_Weather_Menu_PUBLIC.json
│   ├── Agent4_Inventory_Manager_PUBLIC.json
│   ├── Agent5_AI_Menu_Photographer_PUBLIC.json
│   ├── Agent6_Analytics_Reporter_PUBLIC.json
│   └── Agent7_Voice_Agent_PUBLIC.json
├── dashboard/            # Web-based control panel
│   ├── index_PUBLIC.html
│   ├── netlify.toml
│   └── README.md
└── README.md
```

**Tech Stack:**

| Component | Technology |
|-----------|-----------|
| Automation | n8n v2.6+ |
| Database | Supabase (PostgreSQL) |
| AI Models | OpenAI GPT-4o-mini / GPT-4o / DALL-E 3 |
| Alerts | Telegram Bot |
| Email | Gmail (OAuth2) |
| Reviews | SerpAPI (Google Maps, TripAdvisor, Yelp) |
| Weather | OpenWeatherMap API |
| Voice Agent | Vapi.ai |
| Dashboard | HTML/JS hosted on Netlify |

---

## 🤖 Agents

### Agent 1 — Reputation & Sentiment Manager
Monitors restaurant reviews across Google Maps, TripAdvisor and Yelp every 6 hours. Uses GPT-4o-mini to analyze sentiment, assign scores and generate summaries. All reviews are stored in Supabase with full audit trail.

**Trigger:** Schedule (every 6h) + Webhook  
**Output:** Sentiment scores, AI summaries saved to database

---

### Agent 2 — Reservations & Reminder Bot
A Telegram-based booking assistant that guides guests through a step-by-step reservation flow. Collects name, date, time, party size, email and phone. Sends Gmail confirmation on booking and automatic reminder 24 hours before the visit.

**Trigger:** Telegram messages + Schedule (daily at 10:00)  
**Output:** Reservation saved to Supabase, confirmation email, reminder email

---

### Agent 3 — Weather Menu Promoter
Fetches current weather data and selects the most suitable dishes from the menu based on weather conditions. Uses GPT-4o-mini to match weather tags with menu items and sends daily recommendations to the owner via Telegram.

**Trigger:** Schedule (daily at 08:00) + Webhook  
**Output:** Weather-based menu recommendations via Telegram

---

### Agent 4 — Inventory Manager
Reads current stock levels from the database and uses GPT-4o-mini to identify low-stock items. Calculates recommended order quantities for 7 days of operation. Sends alerts to the owner and logs all checks to the database.

**Trigger:** Schedule (daily at 08:00) + Webhook  
**Output:** Inventory alerts via Telegram, logs saved to Supabase

---

### Agent 5 — AI Menu Photographer
A dual-mode photo agent. **Analyze mode** accepts a dish photo URL and uses GPT-4o Vision to evaluate lighting, composition, appetizing appeal and background — returning a quality score out of 10. **Generate mode** takes a dish name and description, creates a professional DALL-E 3 prompt via GPT-4o-mini, generates a high-quality menu photo and stores it in Supabase Storage.

**Trigger:** Webhook (POST request)  
**Output:** Photo analysis scores or AI-generated menu photo stored in Supabase

---

### Agent 6 — Analytics Reporter
Generates a daily business intelligence report by querying three data sources in parallel: review sentiment stats, reservation occupancy rates, and agent activity logs. A GPT-4o-mini narrative summary is sent to the owner via Telegram, while the raw JSON data is stored in Supabase for dashboard visualization.

**Trigger:** Schedule (daily at 09:00) + Webhook  
**Output:** AI narrative report via Telegram, analytics data saved to Supabase

---

### Agent 7 — Voice Agent
Handles inbound phone calls 24/7 via Vapi.ai. A voice assistant named Sofia greets callers, detects their intent (reservation, menu question, or general info), and routes each call to the appropriate branch. Reservations are saved directly to Supabase, the owner receives a Telegram alert, and every call is logged for analytics.

**Trigger:** Vapi.ai webhook (end-of-call-report)  
**Output:** Reservation saved to Supabase, owner notified via Telegram, call logged

---

## 🧩 Modular Design

The system is intentionally designed as a **plug-and-play platform**. Each agent is a self-contained n8n workflow that can be activated or deactivated independently.

**Currently implemented (v2.0):**
- ✅ Agent 1 — Reputation & Sentiment
- ✅ Agent 2 — Reservations & Reminders
- ✅ Agent 3 — Weather Menu Promoter
- ✅ Agent 4 — Inventory Manager
- ✅ Agent 5 — AI Menu Photographer
- ✅ Agent 6 — Analytics Reporter
- ✅ Agent 7 — Voice Agent

**Planned extensions (available on request):**
- 📊 **Agent 8 — Revenue Analytics** — Weekly and monthly performance reports with AI-generated insights and recommendations
- 📱 **Agent 9 — Social Media Poster** — Automatically generates and schedules social media posts based on daily specials, events and weather
- ⭐ **Agent 10 — Review Response Bot** — Automatically drafts personalized responses to new reviews across all platforms

New agents can be added to any existing installation without modifying current workflows.

---

## 🖥️ Dashboard

A lightweight HTML/JS control panel hosted on Netlify. No frameworks, no build step — just clean, fast code.

**Features:**
- Live KPI strip (active agents, reviews, reservations, avg rating)
- Agent cards with run statistics and last execution time
- Live event feed from Supabase
- Manual trigger buttons for each agent
- Settings panel (Supabase connection, webhook URLs)

**Demo:** [[[neolithai.netlify.app](https://neolithai.netlify.app)](https://fancy-figolla-504d1b.netlify.app/)](https://fancy-figolla-504d1b.netlify.app/)

---

## 🚀 Setup

### Prerequisites
- n8n v2.6+ (self-hosted or cloud)
- Supabase project
- OpenAI API key
- Telegram Bot token
- SerpAPI key
- OpenWeatherMap API key
- Gmail OAuth2 credentials
- Vapi.ai account (Agent 7)

### Installation

1. Clone this repository
2. Import JSON files from `/agents` folder into your n8n instance
3. Configure credentials in n8n (replace all `YOUR_*` placeholders)
4. Deploy `/dashboard/index_PUBLIC.html` to Netlify or any static host
5. Enter your Supabase URL, anon key and restaurant ID in dashboard Settings

### Database
The system uses 9 Supabase tables: `restaurant`, `reviews`, `guests`, `reservations`, `menu_items`, `inventory`, `photos`, `agent_logs`, `audit_log`.

---

## 🔒 Security Notes

All workflow files in this repository have been sanitized — API keys, credential IDs, webhook URLs and instance identifiers have been replaced with `YOUR_*` placeholders. Never commit real credentials to a public repository.

---

## 👤 Author

**Yevhenii Nohin** — AI Automation Engineer  
Building intelligent workflows with n8n, OpenAI and Supabase

- 🌐 [neolithai.netlify.app](https://neolithai.netlify.app)
- 💼 [Upwork Profile](https://www.upwork.com)
- 🐙 [GitHub](https://github.com/Evgeniy1970-ai)

---

## 📄 License

MIT License — feel free to use this as a reference or starting point for your own restaurant automation project.
