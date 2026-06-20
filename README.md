# Restaurant AI Suite — Dashboard

Command Center dashboard for all 5 Restaurant AI agents.

## Stack
- Pure HTML/JS (no build step) — deploy directly to Netlify
- Supabase REST API (anon key, client-side)
- n8n webhooks via ngrok or production URL

## Quick Deploy to Netlify

1. Drag the `dashboard/` folder to [netlify.com/drop](https://app.netlify.com/drop)
   — OR —
   Push to GitHub and connect repo in Netlify.

## First-time Setup

After deploying, open the app → **Settings** tab and fill in:

| Field | Value |
|-------|-------|
| Supabase URL | `https://vyaebzrydyppsvpmlwcx.supabase.co` |
| Supabase Anon Key | from your Supabase project → Settings → API |
| Restaurant ID | `e12f1eae-f48d-4c5f-ac9b-b9fd0f84e754` |
| Webhook Base URL | your current ngrok URL |
| Agent webhook paths | `/webhook/reputation`, `/webhook/reservations`, etc. |

Settings are saved to **localStorage** (per browser, not committed).

## Features

- **Dashboard** — KPI strip + all 5 agent cards with live stats
- **Live Feed** — real-time event stream (webhook triggers, DB syncs)
- **Agent Detail views** — per-agent data tables (Reviews, Reservations, etc.)
- **Settings** — configure Supabase + all webhook paths + test/trigger each agent
- **Auto-refresh** every 2 minutes when connected

## Supabase Tables Expected

| Agent | Table | Key columns |
|-------|-------|-------------|
| Agent 1 | `reviews` | `restaurant_id`, `rating`, `sentiment_score`, `source`, `review_text`, `created_at` |
| Agent 2 | `reservations` | `restaurant_id`, `guest_name`, `reservation_date`, `party_size`, `status`, `phone` |
| Agent 3 | `promotions` | `restaurant_id`, `weather_condition`, `message`, `sent_at` |
| Agent 4 | `inventory_events` | `restaurant_id`, `item`, `action`, `created_at` |
| Agent 5 | `photo_jobs` | `restaurant_id`, `dish_name`, `image_url`, `created_at` |

## Files

```
dashboard/
├── index.html      # entire app (single file, no dependencies except Google Fonts)
└── netlify.toml    # headers + redirect rules
```
