# UrbanPulse

### AI-Powered Mobile Urban Intelligence Platform

UrbanPulse turns moving vehicles into mobile sensing units. It processes camera feeds onboard in real time to detect road conditions and traffic activity, then converts those detections into structured events that appear on a central GIS dashboard.

---

## What It Does

- **Road Intelligence** — detects potholes, speed bumps, and unpaved roads from the onboard front camera
- **Traffic Intelligence** — detects, tracks, and counts vehicles (car, bus, truck, motorcycle) from a side camera, and estimates congestion
- **Central Dashboard** — displays events on a live GIS map with heat maps, live camera panels, an event stream, and evidence images

---

## How It Works

```
Bus Camera → Edge AI (YOLOv8n) → Structured Event → Central API → Dashboard
```

Video never leaves the bus. Only small JSON events (~1.2 KB each) are sent to the central system.

---

## How To Run

Open **three terminals**.

### Terminal 1 — Central Backend

    cd ~/Desktop/fleetbus
    source .venv/bin/activate
    python -m uvicorn app.main:app --app-dir backend --host 127.0.0.1 --port 8000

### Terminal 2 — Edge AI

    cd ~/Desktop/fleetbus
    source .venv/bin/activate
    export AI_DEVICE=cpu
    python -m uvicorn edge.main:app --app-dir backend --host 127.0.0.1 --port 8001

### Terminal 3 — Frontend

    cd ~/Desktop/fleetbus
    npm install
    npm run dev -- --host 127.0.0.1 --port 5173

Open **http://127.0.0.1:5173** in your browser.

---

## Tech Stack

- **Edge AI:** YOLOv8n, OpenCV, ByteTrack
- **Backend:** FastAPI, SQLite
- **Frontend:** React, Vite, Tailwind, Leaflet

---
