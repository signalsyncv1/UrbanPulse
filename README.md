# UrbanPulse

**AI-Powered Mobile Urban Intelligence Platform Using Public Transport Fleet**

Team **Signal Sync** · Problem **SIH 26124** (Bharat Electronics Limited)

## What It Does

Transforms public transport buses into mobile urban sensing units:

- **Edge AI** — YOLOv8n detects potholes, unpaved roads, speed bumps, vehicles (car/bus/truck/motorcycle)
- **Central backend** — FastAPI + SQLite aggregates fleet events
- **GIS dashboard** — React + Leaflet visualizes events on a live Chennai map with heat maps

**Key result:** 148,000× bandwidth reduction vs. raw video streaming.

## Quick Start

```bash
# Terminal 1 — Backend (port 8000)
cd ~/Desktop/fleetbus && source .venv/bin/activate
python -m uvicorn app.main:app --app-dir backend --host 127.0.0.1 --port 8000

# Terminal 2 — Edge AI (port 8001)
cd ~/Desktop/fleetbus && source .venv/bin/activate
export AI_DEVICE=cpu
python -m uvicorn edge.main:app --app-dir backend --host 127.0.0.1 --port 8001

# Terminal 3 — Frontend (port 5173)
cd ~/Desktop/fleetbus
npm install
npm run dev -- --host 127.0.0.1 --port 5173
Open http://127.0.0.1:5173

Architecture
text
BUS EDGE (inference)  →  CENTRAL BACKEND  →  DASHBOARD
  YOLOv8n                 FastAPI + SQLite     React + Leaflet
  OpenCV                  /api/events          GIS + heat maps
  ByteTrack               /api/incidents/anpr
                          /api/metrics/bandwidth
Tech Stack
Layer	Technology
Edge AI	Ultralytics YOLOv8n, OpenCV, ByteTrack
Backend	FastAPI, Uvicorn, SQLite
Frontend	React 18, Vite, Tailwind, Leaflet
Model	Trained road defect detector (pothole, unpaved_road, speed_bump)
Verified Metrics
Inference: ~100 ms/frame (CPU-only)

Edge FPS: 3–6 (Intel i3-10110U)

Bandwidth reduction: 148,000×

Events captured: 500+ live

Documentation
SIH_REQUIREMENTS_MAP.md — PS requirement coverage

PITCH_SCRIPT.md — 7-min demo script

START_HERE.md — setup guide

License
Prototype for SIH 2026.

text

**Save.**
