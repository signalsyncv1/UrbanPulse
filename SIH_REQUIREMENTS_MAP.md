# Problem Statement Coverage & Prototype Traceability

## UrbanPulse — AI-Powered Mobile Urban Intelligence Platform

UrbanPulse transforms public transport vehicles into **mobile urban sensing units** using Edge AI, Computer Vision, event intelligence, and GIS-based monitoring.

The prototype demonstrates an end-to-end pipeline: onboard video streams are analyzed at the edge, meaningful observations are validated into structured events, and those events are made available through a centralized urban-intelligence platform.

---

## How to Verify

All claims in this document can be verified by running the prototype locally:

1. Start the backend (port 8000), edge AI (port 8001), and frontend (port 5173)
2. Open `http://127.0.0.1:5173`
3. The dashboard shows live road defects, vehicle counts, and traffic events
4. See `README.md` for setup instructions

---

## 1. Coverage Overview

UrbanPulse addresses the problem statement through four functional layers:

```
Mobile Sensing → Edge AI → Event Intelligence → Central Platform
```

The prototype currently demonstrates:

- Road-condition detection (potholes, speed bumps, unpaved roads)
- Vehicle detection, classification, and counting
- Multi-object tracking across frames
- Traffic-density and congestion estimation
- Concurrent multi-camera processing
- Edge-side AI inference
- Event validation and geo-tagged event generation
- Visual evidence capture
- Centralized event aggregation with GIS visualization
- Event review and resolution workflow

---

## 2. Onboard Edge-AI Requirements

| Requirement | Coverage | Demonstration |
|---|---|---|
| Road-condition monitoring | ✅ | Road Intelligence detects potholes, speed bumps, unpaved roads |
| Vehicle detection | ✅ | Vehicles detected from traffic camera stream |
| Vehicle classification | ✅ | Cars, buses, trucks, motorcycles classified |
| Vehicle counting | ✅ | Tracked detections converted to counts |
| Traffic-density monitoring | ✅ | Vehicle observations aggregated into density |
| Congestion identification | ✅ | Traffic classified into congestion levels |
| Multi-object tracking | ✅ | Persistent track IDs across consecutive frames |
| Multi-camera sensing | ✅ | Independent Road + Traffic streams run concurrently |
| Edge processing | ✅ | Inference, filtering, tracking, event generation at sensing layer |
| Confidence-based detection | ✅ | Model confidence used to filter detections |
| False-positive reduction | ✅ | Spatial dedup + temporal cooldown before event creation |
| Event generation | ✅ | Confirmed detections become structured urban events |
| Evidence generation | ✅ | Visual evidence associated with events |
| Geo-temporal context | ✅ | Every event carries location + timestamp |
| Source identification | ✅ | Events remain tied to sensing vehicle + camera source |

---

## 3. Road Intelligence

The Road Intelligence pipeline continuously analyzes the forward-facing road stream.

**Detectable road conditions (current model):**
- Potholes
- Speed bumps
- Unpaved-road conditions

**Processing flow:**

```
Road Camera → Frame Processing → Road AI Inference → Confidence Filtering
→ Validation → Confirmed Observation → Location + Timestamp + Source
→ Evidence Capture → Urban Event
```

A single isolated prediction is never treated as an urban event. Road observations must pass confidence and validation checks before becoming events, providing an additional layer between raw AI output and the central platform.

---

## 4. Traffic Intelligence

The Traffic Intelligence pipeline analyzes a second, independent camera stream.

**Capabilities:**
- Vehicle detection
- Vehicle classification
- Multi-object tracking
- Vehicle counting
- Traffic-density estimation
- Congestion classification

**Processing flow:**

```
Traffic Camera → YOLO Detection → Vehicle Classification → ByteTrack
→ Persistent Track IDs → Vehicle Counts → Traffic Density → Congestion Intelligence
```

Tracking allows UrbanPulse to reason about vehicles across frames rather than treating every frame as an isolated observation.

---

## 5. Edge-First Processing

UrbanPulse follows an edge-first architecture. The sensing layer performs the computationally intensive visual processing **before** information is sent to the central platform.

**Processing at the edge:**
- Video-frame acquisition and preprocessing
- AI inference
- Object tracking
- Confidence filtering
- Validation
- Vehicle counting
- Congestion estimation
- Event creation
- Evidence preparation

**Sent to the central layer:**

Instead of continuous raw video, UrbanPulse transmits structured observations containing:

```
Event Type · Source Vehicle · Camera Source · Timestamp · Location
· Confidence · Traffic/Road Metadata · Visual Evidence · Event Status
```

This supports the problem statement's objective of reducing unnecessary video transmission while retaining meaningful urban intelligence.

---

## 6. Event Intelligence

UrbanPulse introduces an intelligence layer between AI detection and the central dashboard:

```
Raw Detection → Validation → Context Enrichment → Structured Event
```

This prevents every bounding box from becoming an operational alert.

**A generated event includes:**
- Event identifier and category
- Source vehicle and camera
- Timestamp and geographic coordinates
- Confidence score
- Detection metadata
- Visual evidence
- Current status and status history

This provides traceability from the original sensing source to the final operational event.

---

## 7. Centralized Intelligence Platform

The central platform receives intelligence from the sensing layer and unifies it into a single operational interface.

| Requirement | Coverage | Demonstration |
|---|---|---|
| Centralized event aggregation | ✅ | Events collected into one platform |
| Operational dashboard | ✅ | Road, traffic, event, system info presented centrally |
| Real-time event visibility | ✅ | Newly generated events surface to operator |
| GIS visualization | ✅ | Geo-tagged observations displayed spatially |
| Road-condition visualization | ✅ | Road observations represented geographically |
| Traffic intelligence | ✅ | Counts and congestion shown centrally |
| Traffic reports | ✅ | Traffic observations reviewable beyond live feed |
| Event history | ✅ | Previous observations remain accessible |
| Evidence review | ✅ | Operators inspect visual evidence per event |
| Event-status management | ✅ | Events move through operational workflow |
| Resolution tracking | ✅ | Status history provides traceability |

---

## 8. GIS-Based Urban Intelligence

Every supported event carries geographic information. The GIS layer converts individual observations into spatial intelligence.

```
Edge Observation → Latitude + Longitude → Structured Event → GIS Layer
```

**Demonstrated GIS capabilities:**
- Geo-tagged event markers
- Road-condition visualization
- Traffic-condition visualization
- Event inspection
- Spatial grouping of observations
- Infrastructure-condition representation
- Event-density visualization

The same architecture aggregates observations from multiple sensing vehicles as the fleet expands.

---

## 9. Evidence-Backed Event Management

UrbanPulse retains a link between AI observations and their supporting visual evidence.

```
Detection → Validation → Event → Evidence → Operator Review
```

This provides a verifiable record of what the AI observed at the time an event was created.

Evidence-backed events improve traceability, operator verification, incident review, status management, and auditability.

---

## 10. Detection-to-Resolution Workflow

UrbanPulse supports a full operational event lifecycle:

```
Detected → Validated → Event Generated → Notification → Evidence Review
→ Acknowledgement → Action → Resolution
```

This transforms AI observations into actionable information rather than isolated computer-vision predictions.

---

## 11. Traffic Reports and Analytics

Frame-level traffic detections are converted into reviewable operational information:

- Detected vehicle categories
- Vehicle counts
- Traffic-density observations
- Congestion state
- Historical traffic observations
- Location-linked traffic events

This bridges real-time Computer Vision with higher-level traffic intelligence.

---

## 12. Multi-Camera Intelligence

The prototype demonstrates two independent sensing streams operating concurrently:

```
                    SENSING VEHICLE
                          │
              ┌───────────┴───────────┐
              ▼                       ▼
         ROAD CAMERA             TRAFFIC CAMERA
              ▼                       ▼
       Road Intelligence       Traffic Intelligence
         Road Conditions         Vehicle Detection
         Event Validation        ByteTrack Tracking
                                 Traffic Density
                                 Congestion
              └───────────┬───────────┘
                          ▼
                     Event Layer
                          ▼
                  Central Platform
```

The architecture supports multiple sensing functions without coupling all intelligence to a single camera pipeline.

---

## 13. System Architecture

```
┌──────────────────────────────────────────────┐
│             MOBILE SENSING LAYER             │
│        Road Camera       Traffic Camera      │
└───────────────┬──────────────────────────────┘
                ▼
┌──────────────────────────────────────────────┐
│             EDGE AI LAYER                    │
│  Road AI          Vehicle Detection          │
│  OpenCV           ByteTrack                  │
│  Validation       Traffic Analysis           │
└───────────────┬──────────────────────────────┘
                ▼
┌──────────────────────────────────────────────┐
│          EVENT INTELLIGENCE LAYER            │
│  Validation · Context Enrichment             │
│  Geo-Temporal Information · Evidence         │
│  Event Generation                            │
└───────────────┬──────────────────────────────┘
                ▼
┌──────────────────────────────────────────────┐
│        CENTRAL INTELLIGENCE PLATFORM         │
│  Dashboard · Event Management                │
│  GIS · Evidence Review                       │
│  Reports · Resolution Workflow               │
└──────────────────────────────────────────────┘
```

---

## 14. Technology Mapping

| Layer | Technology | Role |
|---|---|---|
| Edge AI | Python | AI and event-processing logic |
| Computer Vision | OpenCV | Video and frame processing |
| Road Intelligence | Custom YOLO model | Road-condition detection |
| Traffic Intelligence | YOLOv8n | Vehicle detection + classification |
| Tracking | ByteTrack | Persistent vehicle tracking |
| ML Runtime | PyTorch / Ultralytics | Model inference |
| Backend | FastAPI | Edge + central APIs |
| API Server | Uvicorn | Backend runtime |
| Frontend | React | Command + monitoring interface |
| Build Tool | Vite | Frontend build |
| GIS | Leaflet | Geographic visualization |
| Storage | Structured event store | Event history + workflow |
| Evidence | Image storage | Verification + audit trail |

---

## 15. End-to-End Prototype Flow

```
Moving Vehicle → Camera Observation → Edge AI → Detection
→ Tracking/Validation → Context Enrichment → Structured Event
→ Evidence → Central Platform → Notification → GIS/Reports
→ Operator Review → Resolution
```

This demonstrates the complete transition from **visual sensing to actionable urban intelligence**.

---

## 16. Scalability

The current prototype demonstrates the architecture on one sensing vehicle with two AI streams. The same model scales horizontally across a public transport fleet.

```
Vehicle 01 ──┐
Vehicle 02 ──┤
Vehicle 03 ──┤
Vehicle 04 ──┼────► Central Urban Intelligence Platform
    ...      │
Vehicle N ───┘
```

Each vehicle performs local AI processing and contributes validated events to the same central system — increasing geographic coverage without requiring equivalent fixed sensing infrastructure.

---

## 17. Extended Intelligence Capabilities

The architecture is designed so additional intelligence modules plug into the same sensing and event framework.

**Supported by the architecture (roadmap):**
- Waterlogging detection
- Zebra-crossing condition monitoring
- Road-divider monitoring
- Traffic-sign condition monitoring
- Vulnerable pedestrian detection
- School-zone safety monitoring
- Automatic Number Plate Recognition
- Traffic-rule violation detection
- Origin-Destination analysis
- Route-delay analytics
- Fleet-wide congestion heatmaps
- Infrastructure deficiency maps

All of these use the same core pipeline:

```
Detection → Validation → Context → Event → GIS → Action
```

---

## 18. Key Differentiators

- **Mobile rather than fixed sensing** — moving vehicles extend coverage along existing transport routes
- **Edge-first processing** — visual intelligence generated close to the sensing source
- **Multi-domain intelligence** — road + traffic pipelines run simultaneously
- **Detection validation** — raw predictions never become events until validated
- **Evidence-backed events** — every event retains supporting visual information
- **Geo-spatial intelligence** — observations become location-aware urban information
- **Full event lifecycle** — detection connects to review, action, and resolution
- **Fleet scalability** — architecture extends from one vehicle to an entire transport fleet

---

## 19. Requirement-to-Solution Summary

| Problem Requirement | UrbanPulse Approach |
|---|---|
| Use public transport as sensing infrastructure | Vehicle-mounted cameras act as mobile sensing inputs |
| Detect road conditions | Custom Road Intelligence pipeline |
| Understand traffic | Detection, classification, tracking, congestion analysis |
| Process information at the edge | Local AI inference and event generation |
| Reduce unnecessary data transmission | Structured events replace continuous video |
| Aggregate fleet intelligence | Central event-ingestion architecture |
| Visualize events geographically | Leaflet-based GIS interface |
| Preserve evidence | Event-linked visual evidence |
| Generate actionable information | Event management, reports, resolution workflow |
| Support scalable deployment | Independent Edge + Central processing layers |

---

## 20. Core Design Principle

UrbanPulse follows a four-stage intelligence model:

```
SENSE → DETECT → VALIDATE → ACT
```

**Sense** — Moving vehicles observe the urban environment.

**Detect** — Edge AI identifies meaningful road and traffic conditions.

**Validate** — Tracking, confidence filtering, temporal checks, and context enrichment convert predictions into reliable observations.

**Act** — Validated events are mapped, reviewed, analyzed, and progressed through an operational workflow.

---

## UrbanPulse

**Turning public transport into a distributed mobile intelligence network for continuous urban awareness.**
