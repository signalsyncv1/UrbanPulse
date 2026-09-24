# UrbanPulse 

**AI-Powered Mobile Urban Intelligence Platform Using Public Transport Fleet**

## What It Does

UrbanPulse transforms public transport buses into **mobile urban sensing units** using the cameras already mounted on them. Instead of streaming raw video to the cloud, each bus runs **onboard edge AI** that detects road and traffic hazards locally and transmits only compact geo-tagged events.

- **Edge AI** — YOLOv8n detects potholes, unpaved roads, speed bumps, and vehicles (car/bus/truck/motorcycle) on CPU-only hardware
- **Central backend** — FastAPI + SQLite aggregates validated events from the fleet
- **GIS dashboard** — React + Leaflet visualizes events on a live Chennai map with density heat maps

**Key result:** ~150,000× bandwidth reduction versus raw video streaming.

---

## Current Status

**Working now (live prototype):**

- 1 live sensing bus (`BUS-104`) running two independent camera pipelines concurrently
- Road camera: YOLOv8n detecting potholes, unpaved roads, speed bumps
- Traffic camera: YOLOv8n (COCO) detecting and counting vehicles by class
- Real-time event pipeline: edge → central → dashboard
- GIS map with event pins, heat maps, and a live edge status panel
- ANPR ingest endpoint (`/api/incidents/anpr`) with 3 seeded demo events
- Bandwidth proof endpoint returning the measured reduction

**Roadmap (honest gaps):**

- Additional road defect classes — waterlogging, missing zebra crossings, damaged signboards
- Multi-bus fleet simulation (currently 1 live + 999 projected)
- Real ANPR plate OCR (endpoint exists, edge integration pending)
- Jetson Orin Nano deployment for 4-camera inference at 60+ FPS
- PostgreSQL + PostGIS migration for production scale

---

## Quick Start

Terminal 1 — Central Backend (port 8000):

    cd ~/Desktop/fleetbus && source .venv/bin/activate
    python -m uvicorn app.main:app --app-dir backend --host 127.0.0.1 --port 8000

Terminal 2 — Edge AI Worker (port 8001):

    cd ~/Desktop/fleetbus && source .venv/bin/activate
    export AI_DEVICE=cpu
    python -m uvicorn edge.main:app --app-dir backend --host 127.0.0.1 --port 8001

Terminal 3 — Frontend (port 5173):

    cd ~/Desktop/fleetbus
    npm install
    npm run dev -- --host 127.0.0.1 --port 5173

Open `http://127.0.0.1:5173`

---

## Architecture

**Bus Edge (onboard)** — YOLOv8n inference, OpenCV preprocessing, ByteTrack tracking, spatial dedup.

↓ transmits structured JSON events (1.2 KB each)

**Central Backend** — FastAPI + SQLite. Endpoints: `/api/events`, `/api/incidents/anpr`, `/api/metrics/bandwidth`.

↓ serves event data + MJPEG streams

**Dashboard** — React + Leaflet. GIS map with heatmaps, live camera panels, event stream, incident center.

---

## Repository Structure

- **backend/app/** — FastAPI central event service (port 8000)
- **backend/edge/** — Onboard edge AI worker (port 8001)
- **backend/models/** — Trained model weights
- **backend/videos/** — Demo video sources
- **src/** — React frontend (Vite + Tailwind + Leaflet)
- **src/components/** — UI components
- **src/pages/** — Route-level pages
- **src/context/** — Global state (UrbanPulseContext)
- **src/services/** — API clients
- **docs/** — Setup and validation documentation
- **scripts/** — Start/stop shell scripts
- **SIH_REQUIREMENTS_MAP.md** — Line-by-line PS coverage

---

## Tech Stack

| Layer | Technology | Purpose |
|---|---|---|
| Edge AI | Ultralytics YOLOv8n | Road + vehicle detection |
| Edge processing | OpenCV | Video decode, resize, annotation |
| Tracking | ByteTrack | Persistent vehicle IDs |
| Backend | FastAPI + Uvicorn | Async REST + MJPEG streaming |
| Storage | SQLite (WAL) | Event store |
| Frontend | React 18 + Vite + Tailwind | Operator dashboard |
| GIS | Leaflet | 5 tile providers |

---

## Verified Metrics

| Metric | Value |
|---|---|
| Edge inference latency | ~100 ms/frame |
| Edge FPS | 3–6 (CPU-only) |
| Events captured (live session) | 500+ |
| Average event size | ~1.2 KB |
| Raw-video counterfactual | 201.6 GB / bus / day |
| Actual edge upload | ~1.2 MB / bus / day |
| **Bandwidth reduction** | **~150,000×** |

All metrics queryable at runtime:

- `GET http://127.0.0.1:8000/api/metrics/bandwidth`
- `GET http://127.0.0.1:8000/api/events`
- `GET http://127.0.0.1:8001/api/status`

---
