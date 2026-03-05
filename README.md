# MediLight Shelf Device

**Virtual LED controller (digital twin) for the MediLight Smart Dispensing System.**

> Connects to backend via WebSocket → receives LED commands → lights up the right shelf bins

![WebSocket](https://img.shields.io/badge/WebSocket-Native-010101?logo=websocket) ![HTML](https://img.shields.io/badge/HTML-Vanilla_JS-E34F26?logo=html5) ![Vercel](https://img.shields.io/badge/Deployed-Vercel-000?logo=vercel)

## What This Does

This is a **digital twin** of the physical ESP32 LED shelf hardware. It simulates what a real pharmacy shelf would look like when the system activates LEDs to guide pharmacists to the correct medication bins.

When a pharmacist confirms an order on the dashboard:
1. Backend broadcasts a WebSocket command with LED addresses
2. This device receives the signal in real time
3. The matching shelf bins glow green with a pulsing animation
4. LEDs auto-deactivate after 30 seconds

The same WebSocket protocol works with real ESP32 hardware — this page just visualizes it in a browser.

## Shelf Layout

```
┌─────────────────────────────────────────────┐
│  SHELF A — General Medications              │
│  ┌─────────────────┐ ┌─────────────────┐   │
│  │ Amoxicillin     │ │ Ibuprofen       │   │
│  └─────────────────┘ └─────────────────┘   │
│  ┌─────────────────┐ ┌─────────────────┐   │
│  │ Aspirin         │ │ Lisinopril      │   │
│  └─────────────────┘ └─────────────────┘   │
├─────────────────────────────────────────────┤
│  SHELF B — Specialty                        │
│  ┌─────────────────┐ ┌─────────────────┐   │
│  │ Metformin       │ │ Omeprazole      │   │
│  └─────────────────┘ └─────────────────┘   │
│  ┌─────────────────┐ ┌─────────────────┐   │
│  │ Cetirizine      │ │ Prednisone      │   │
│  └─────────────────┘ └─────────────────┘   │
├─────────────────────────────────────────────┤
│  SHELF C — Controlled Substances            │
│  ┌─────────────────┐ ┌─────────────────┐   │
│  │ Lorazepam       │ │ Adderall        │   │
│  └─────────────────┘ └─────────────────┘   │
│  ┌─────────────────┐ ┌─────────────────┐   │
│  │ Codeine         │ │ Alprazolam      │   │
│  └─────────────────┘ └─────────────────┘   │
└─────────────────────────────────────────────┘
```

## How to Use

1. Open the shelf device page in a browser
2. Click the **⚙ Settings** gear icon (top right)
3. Enter your backend URL: `https://medilight-backend.onrender.com`
4. Click **Connect** — wait for Render cold start (~30s on free tier)
5. Status badge turns green: **Connected to Backend**
6. Now go to the dashboard, process a prescription, and confirm an order
7. Watch the shelf bins light up in real time!

You can also click **Test All LEDs** to verify the connection — all 12 bins will flash for 5 seconds.

## Tech Details

- **Single HTML file** — no build step, no dependencies
- **Native WebSocket** — connects directly to backend's `ws` server
- **Auto-reconnect** — exponential backoff up to 10 attempts
- **Health check ping** — wakes up Render free tier before attempting WebSocket

## Deploy

**Option 1 — Vercel (recommended):**
Push this repo to GitHub, import to Vercel as a static site. Done.

**Option 2 — Open locally:**
Just open `index.html` in any browser. Click Settings, enter backend URL, connect.

## WebSocket Protocol

The shelf device listens for JSON messages:

```json
{
  "command": "ACTIVATE_LEDS",
  "activation_mode": "BLINK_FAST",
  "duration_seconds": 30,
  "color_hex": "#00FF00",
  "targets": [
    { "led_address": "shelf_A_row_1_pos_1", "item": "Amoxicillin 500mg" },
    { "led_address": "shelf_C_row_2_pos_1", "item": "Codeine 30mg" }
  ]
}
```

To deactivate: `{ "command": "DEACTIVATE_LEDS" }`

This same protocol is what a real ESP32 microcontroller would receive over WiFi.

## Related Repos

- **Dashboard** → [medilight-dashboard](https://github.com/YOUR_USERNAME/medilight-dashboard)
- **Backend API** → [medilight-backend](https://github.com/YOUR_USERNAME/medilight-backend)
- **Project Guide + Test Images** → [medilight-guide](https://github.com/YOUR_USERNAME/medilight-guide)
