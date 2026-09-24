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
Bus Camera → Edge AI → Structured Event → Central API → Dashboard
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

- **Edge AI:** YOLO, OpenCV, ByteTrack
- **Backend:** FastAPI, SQLite
- **Frontend:** React, Vite, Tailwind, Leaflet

---

## Prototype Snapshot 

<img width="1911" height="861" alt="Dashboard" src="https://github.com/user-attachments/assets/156e4cb6-4bee-4c17-ba31-398c02792aac" />

<img width="803" height="733" alt="Notification" src="https://github.com/user-attachments/assets/f73f5028-1732-456b-896d-c16dd88f1f19" />

## Demo Video

<video src="https://github.com/user-attachments/assets/https://github.com/user-attachments/assets/bbedce01-b2c8-43ea-9c87-d7fb852fe0eb" controls width="800"></video>
