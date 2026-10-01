# 🚔 AI-Powered CCTV Surveillance & Policing Platform

> **Transforming Public Safety with AI-Powered CCTV Surveillance & Policing**

An AI-powered CCTV platform that turns passive cameras into an active policing assistant. The platform is designed to detect people and vehicles, estimate vehicle speed, read number plates, check vehicle records against the VAHAN registry, match faces against wanted and missing-person records, identify suspicious activity, and prepare verified alerts, incident reports, and route information for police operators.

The platform follows a **human-in-the-loop** approach: AI proposes detections and matches, an officer reviews and verifies them, and only then is legal action initiated.

---

<p align="center">

![Status](https://img.shields.io/badge/Status-Development-blue?style=for-the-badge)
![AI Powered](https://img.shields.io/badge/AI-Powered-purple?style=for-the-badge)
![Computer Vision](https://img.shields.io/badge/Computer-Vision-orange?style=for-the-badge)
![ANPR](https://img.shields.io/badge/ANPR-VAHAN-green?style=for-the-badge)
![Policing](https://img.shields.io/badge/Public-Safety-red?style=for-the-badge)
![Human in the Loop](https://img.shields.io/badge/Human-in--the--Loop-blue?style=for-the-badge)

</p>

---

# 📑 Table of Contents

- [Project Overview](#-project-overview)
- [Problem Statement](#-problem-statement)
- [Operational Gaps](#-operational-gaps)
- [Our Solution](#-our-solution)
- [Key Features](#-key-features)
- [System Workflow](#-system-workflow)
- [Development Process](#-development-process)
- [AI Detection Engine](#-ai-detection-engine)
- [Vehicle Speed Estimation](#-vehicle-speed-estimation)
- [ANPR & VAHAN Verification](#-anpr--vahan-verification)
- [Face Detection & Matching](#-face-detection--matching)
- [Suspicious Activity Detection](#-suspicious-activity-detection)
- [Video Quality Engine](#-video-quality-engine)
- [Confidence Tiers](#-confidence-tiers)
- [Registry Data Layer](#-registry-data-layer)
- [Rule Engine & Dispatch](#-rule-engine--dispatch)
- [Human-in-the-Loop Workflow](#-human-in-the-loop-workflow)
- [SCADA / Operator Dashboard](#-scada--operator-dashboard)
- [Core Technical Pillars](#-core-technical-pillars)
- [Technology Stack](#-technology-stack)
- [Project Architecture](#-project-architecture)
- [Deployment Architecture](#-deployment-architecture)
- [Expected Outcomes](#-expected-outcomes)
- [Validation & Pilot](#-validation--pilot)
- [Privacy, Legal Compliance & Audit](#-privacy-legal-compliance--audit)
- [Open Items](#-open-items)
- [Project Screenshots](#-project-screenshots)
- [Project Demo](#-project-demo)
- [License](#-license)

---

# 🚔 Project Overview

The **AI-Powered CCTV Surveillance & Policing Platform** is a centralized public-safety platform designed to transform conventional CCTV infrastructure into an AI-assisted detection and response system.

Traditional CCTV systems primarily record video for later review. This platform is designed to analyze live and offline video, identify relevant people and vehicles, correlate detections with police registries, estimate vehicle speed, detect suspicious activity, and prepare actionable alerts for operator verification.

The platform combines:

- Computer vision
- Deep learning
- Person and vehicle detection
- Object and centroid tracking
- Automatic Number Plate Recognition (ANPR)
- OCR-based number-plate reading
- VAHAN registry verification
- Face detection and embedding matching
- Wanted and missing-person matching
- Vehicle speed estimation
- Suspicious activity rules
- Video quality assessment
- Confidence tiering
- False-alarm reduction
- Incident report generation
- Police station selection
- Route-map generation
- SMS / email dispatch
- Operator review and confirmation
- Detection event logging
- Audit trails

---

# ⚠️ Problem Statement

Police control rooms lack an automated and trustworthy way to turn live CCTV video into **verified, actionable alerts**.

Today, identifying a stolen or blacklisted vehicle, a wanted or missing person, an over-speeding vehicle, or suspicious behaviour can depend on operators continuously watching screens and manually checking separate registries.

This can result in:

- Slow detection
- Missed incidents
- Inconsistent evidence
- Delayed registry verification
- Late dispatch
- False or low-confidence matches
- Increased operator workload

The platform is designed for real Indian road conditions, including:

- Mixed traffic
- Variable lighting
- Night-time conditions
- Rain
- Glare
- Motion blur
- Low-resolution video

A core requirement is that **AI recommendations do not independently trigger legal action**. The platform keeps a human officer in control before any action is taken.

---

# 🔍 Operational Gaps

| Problem Area | Camera-Only / Manual Approach | Platform Response |
|---|---|---|
| Stolen / Blacklisted Vehicles | Plate read manually and checked against registries | ANPR reads the plate and checks VAHAN / relevant vehicle records |
| Wanted / Missing Persons | Officers recognise faces from circulated photographs | Face detection and embedding matching against relevant registries |
| Over-Speeding | Fixed radar available only at selected points | Speed estimated from tracked vehicle movement |
| Suspicious Activity | Depends on an operator noticing the event | Rule-based analysis of tracked paths raises review alerts |
| Poor Video Quality | Weak frames may be misread or discarded | Quality gate and enhancement before detection |
| Alert Dispatch | Phone calls and manual forwarding | Nearest station selection, report generation, route map and SMS/email |
| Evidence & Audit | Information spread across systems | Central detection event log and audit trail |

---

# 🤖 Our Solution

The platform follows a complete pipeline from video capture to officer verification:

```text
CCTV / RTSP / Uploaded Video
            │
            ▼
┌─────────────────────────────┐
│ Video Quality Assessment    │
│ Blur / Brightness / Contrast│
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│ AI Detection & Tracking     │
│ YOLOv8 + Object Tracking    │
└─────────────┬───────────────┘
              │
       ┌──────┼───────────┐
       ▼      ▼           ▼
    Vehicle  Person    Activity
       │      │           │
       ▼      ▼           ▼
    Speed   Face Match   Rules
       │      │           │
       ▼      ▼           ▼
     ANPR   Wanted /    Suspicious
       │    Missing      Activity
       ▼      │
     VAHAN    │
       │      │
       └──────┼──────────────┐
              ▼              ▼
       Registry Verification
              │
              ▼
        Confidence Tiers
              │
              ▼
         Rule Engine
              │
              ▼
       ┌──────┴────────┐
       ▼               ▼
  Officer Review   Event Logging
       │
       ▼
 Verified Alert
       │
       ├── PDF Report
       ├── Route Map
       └── SMS / Email
```

---

# 🚀 Key Features

## 👁️ Person & Vehicle Detection

The platform detects people and vehicles in CCTV streams using AI-based computer vision.

The vehicle detection pipeline is designed for Indian road conditions and includes classes such as:

- Two-wheelers
- Auto-rickshaws
- E-rickshaws
- Cars
- Trucks
- Buses
- Tractors

The documented development approach uses **YOLOv8**, fine-tuned using Indian vehicle data with night and rain augmentation.

---

## 🎯 Object & Centroid Tracking

Tracking is used to maintain the movement identity of detected objects across video frames.

```text
Frame 01 → Vehicle #12
Frame 02 → Vehicle #12
Frame 03 → Vehicle #12
Frame 04 → Vehicle #12
```

Tracking supports:

- Vehicle speed estimation
- Path analysis
- Suspicious activity rules
- Consistent event generation
- Separation of real targets from background traffic

---

## ⚡ Vehicle Speed Estimation

Vehicle speed is estimated from tracked vehicle movement.

The camera is calibrated so that movement between image-space positions can be converted into a speed estimate.

```text
Vehicle Detection
       │
       ▼
Centroid Tracking
       │
       ▼
Position Over Time
       │
       ▼
Camera Calibration
       │
       ▼
Estimated Vehicle Speed
       │
       ▼
Over-Speed Rule
```

The platform measures speed as part of the rule engine rather than relying exclusively on fixed radar points.

---

## 🔢 Automatic Number Plate Recognition

The ANPR pipeline is designed to:

1. Detect the vehicle
2. Detect the number plate
3. Read the plate using OCR
4. Validate the Indian plate format
5. Check the plate against VAHAN
6. Compare the vehicle class with the registry record
7. Produce a verified plate result

```text
Vehicle
   │
   ▼
Number Plate Detection
   │
   ▼
OCR
   │
   ▼
Indian Plate Format Validation
   │
   ▼
VAHAN Registry Check
   │
   ▼
Vehicle Class Verification
   │
   ▼
Registry-backed Result
```

### Important Reliability Rule

An unreadable plate is marked **`unread`** rather than guessed.

The documented production approach removes simulated fallback behaviour and requires the platform to represent uncertainty honestly.

---

# 👤 Face Detection & Matching

The platform detects faces and computes embeddings for comparison against:

- Wanted Persons Registry
- Missing Persons Registry

The matching system uses three confidence tiers:

```text
Face Detection
      │
      ▼
Face Embedding
      │
      ▼
Registry Comparison
      │
      ▼
┌────────────┬────────────┬────────────┐
│  Possible  │   Likely   │  Confirmed │
└────────────┴────────────┴────────────┘
      │             │            │
      ▼             ▼            ▼
    Log        Operator Review   Alert
                               + Review
```

Every confirmed match still requires officer verification before action.

---

# 🧠 Suspicious Activity Detection

The initial suspicious-activity approach is rule-based analysis of tracked paths.

The documented rules include:

- Loitering
- Wrong-way movement
- Abandoned objects
- Sudden crowd formation
- Abnormal movement

```text
Tracked Paths
     │
     ▼
Behaviour Rules
     │
     ├── Loitering
     ├── Wrong-Way Movement
     ├── Abandoned Object
     └── Sudden Crowd
     │
     ▼
Activity Alert
     │
     ▼
Short Evidence Clip
     │
     ▼
Officer Review
```

A learned action model is identified as a later-stage option once suitable labelled examples are available. The final model approach is to be determined with the police team.

---

# 🎥 Video Quality Engine

Real-world CCTV feeds can contain:

- Motion blur
- Low brightness
- Poor contrast
- Rain
- Glare
- Low resolution

The platform uses a quality-first approach.

```text
Input Frame
    │
    ▼
Quality Scoring
    │
    ├── Pass ────────────────► Detection
    │
    ├── Enhance ────────────► Enhancement → Detection
    │
    └── Reject ─────────────► Do Not Trust
```

Quality processing can include:

- Denoising
- Low-light enhancement
- Sharpening

Enhancement is primarily applied to plate and face crops where required.

The original frame is retained as evidence.

---

# 🛡️ Confidence Tiers

The platform uses three confidence levels for matching and alert handling.

| Tier | Meaning | Action |
|---|---|---|
| **Possible** | Weak similarity or poor video quality | Logged and shown to operator for optional review |
| **Likely** | Moderate similarity | Operator review required before an alert is sent |
| **Confirmed** | High similarity at a threshold calibrated on local data | Alert/report prepared, followed by officer verification |

This approach is designed to reduce false alarms and prevent weak matches from being treated as verified incidents.

---

# 🗄️ Registry Data Layer

The platform is designed around multiple registry sources.

| Registry | Used For |
|---|---|
| **Stolen Vehicles** | Plate match and Culprit Vehicle alerts, including FIR reference |
| **Wanted Persons** | Face matching and display of record information |
| **Missing Persons** | Face matching to help locate and safely recover missing people |
| **Blacklisted Vehicles** | Plate matching for vehicles flagged for violations or investigations |
| **Suspect Watch List** | Persons or vehicles under observation, with restricted access |

The development plan includes defining access controls and structuring registry information before operational deployment.

---

# 🚨 Rule Engine & Dispatch

The rule engine combines AI detections, tracking information and registry results.

Example rule categories include:

- Over-speeding
- Culprit / stolen vehicle
- Blacklisted vehicle
- Wanted person
- Missing person
- Suspicious activity

When an actionable event is prepared, the platform can:

```text
Detection
   │
   ▼
Rule Engine
   │
   ▼
Registry / Confidence Verification
   │
   ▼
Officer Review
   │
   ▼
Nearest Police Station
   │
   ├── PDF Incident Report
   ├── Route Map
   ├── SMS Alert
   ├── Email Alert
   └── Audit Entry
```

The platform documentation identifies automatic report generation, route-map generation and SMS/email dispatch as core platform services.

---

# 👮 Human-in-the-Loop Workflow

Human verification is a central design principle.

```text
AI Detection
     │
     ▼
AI Matching / Rule Evaluation
     │
     ▼
Confidence Assessment
     │
     ▼
Operator Dashboard
     │
     ▼
Officer Review
     │
     ├── Reject / Dismiss
     │
     └── Verify
            │
            ▼
      Alert / Report
            │
            ▼
     Dispatch / Follow-up
```

The platform follows the principle:

> **AI recommends → Officer verifies → Legal action follows verification**

This workflow is intended to ensure that automatically detected details are verified before legal action is initiated.

---

# 🖥️ SCADA / Operator Dashboard

The platform provides a central **Streamlit operator dashboard**.

It connects:

- Live webcam streams
- RTSP streams
- Uploaded video clips
- Still images

with:

- AI detection
- Rule-based alerting
- Registry verification
- Police dispatch
- Event logging
- Operator review

### Dashboard Capabilities

- Live video analysis
- Offline video analysis
- Culprit / stolen vehicle alerts
- Wanted-person alerts
- Missing-person alerts
- Over-speed detection
- Suspicious activity alerts
- Automated PDF incident reports
- Nearest police station locator
- Route-map generation
- SMS / email alert dispatch
- Detection event log
- Audit trail
- Review and confirm workflow

---

# 🔄 System Workflow

```text
                  ┌─────────────────────┐
                  │ CCTV / RTSP Feed    │
                  │ Video / Images      │
                  └──────────┬──────────┘
                             │
                             ▼
                  ┌─────────────────────┐
                  │ Video Quality Gate  │
                  │ Pass / Enhance /    │
                  │ Reject              │
                  └──────────┬──────────┘
                             │
                             ▼
                  ┌─────────────────────┐
                  │ YOLOv8 Detection    │
                  │ Person + Vehicle    │
                  └──────────┬──────────┘
                             │
                             ▼
                  ┌─────────────────────┐
                  │ Tracking            │
                  │ Object / Centroid   │
                  └──────────┬──────────┘
                             │
              ┌──────────────┼───────────────┐
              ▼              ▼               ▼
        Vehicle Speed      ANPR          Face Matching
              │              │               │
              ▼              ▼               ▼
        Speed Rules        VAHAN       Wanted / Missing
              │              │               │
              └──────────────┼───────────────┘
                             ▼
                  ┌─────────────────────┐
                  │ Suspicious Activity │
                  │ Rule Engine         │
                  └──────────┬──────────┘
                             │
                             ▼
                  ┌─────────────────────┐
                  │ Confidence Tiers    │
                  └──────────┬──────────┘
                             │
                             ▼
                  ┌─────────────────────┐
                  │ Operator Review     │
                  │ & Verification      │
                  └──────────┬──────────┘
                             │
                             ▼
                  ┌─────────────────────┐
                  │ Alert / Report      │
                  │ Route / Dispatch    │
                  └──────────┬──────────┘
                             │
                             ▼
                  ┌─────────────────────┐
                  │ Audit Trail         │
                  └─────────────────────┘
```

---

# 🧪 Development Process

The platform development is divided into the following phases:

| Phase | Approach | Output |
|---|---|---|
| **1. Data & Registries** | Collect, clean and structure police data and define access controls | Stolen, wanted, missing, blacklisted and watch-list registries |
| **2. Video Quality** | Score blur, brightness and contrast; enhance only where needed | Quality gate and camera quality scorecard |
| **3. Indian Vehicle Detection** | Fine-tune YOLOv8 using Indian road data and night/rain augmentation | Trained model with precision, recall and mAP report |
| **4. ANPR & VAHAN** | Detect plate, OCR, validate format, check VAHAN and compare vehicle class | Verified plate result and registry status |
| **5. Face Matching** | Generate embeddings and compare with wanted/missing registries | Tiered face match with confidence |
| **6. Suspicious Activity** | Apply rule-based analysis to tracked paths | Activity alerts with evidence clips |
| **7. Rule Engine & Dispatch** | Combine detections, select station, generate report/map and dispatch | Alert, report, route map and audit entry |
| **8. Pilot & Validation** | Test at selected junctions and tune thresholds | Pilot accuracy, false-alarm and response-time report |

---

# 🧩 Core Technical Pillars

## 1. Quality-First Detection

AI processing begins with a quality assessment so that weak video reduces trust instead of producing confident but unreliable results.

---

## 2. Intelligent Threat Detection

Detection combined with tracking separates relevant targets from background traffic.

Three-tier confidence banding helps keep weak matches from becoming high-confidence alarms.

---

## 3. Registry-Backed Verification

Plate and face detections are designed to be checked against relevant registry data so that alerts carry supporting evidence for officer review.

---

## 4. Human-in-the-Loop Control

The AI recommends and an officer verifies.

Automatically detected information must be reviewed before legal action.

---

## 5. Edge Processing with Central Control

The development architecture separates video processing from central rules, reporting and dispatch.

```text
Camera / Junction
       │
       ▼
Edge AI Processing
       │
       ▼
Central Rules & Services
       │
       ├── Registry Checks
       ├── Reports
       ├── Route Maps
       └── Dispatch
```

This architecture is intended to reduce bandwidth requirements and support scaling from one junction to many cameras.

---

# 🛠️ Technology Stack

### Artificial Intelligence

- YOLOv8
- Computer Vision
- Object Detection
- Object / Centroid Tracking
- Face Detection
- Face Embeddings
- Suspicious Activity Rules

### Computer Vision

- Video Quality Assessment
- OCR
- Number Plate Detection
- Number Plate Recognition
- Object Tracking
- Centroid Tracking
- Image Enhancement

### Application

- Streamlit
- Live Video Analysis
- Offline Video Analysis
- Operator Dashboard

### Data & Verification

- VAHAN / e-Vahan API integration plan
- Police Registries
- Wanted Persons Registry
- Missing Persons Registry
- Stolen Vehicles Registry
- Blacklisted Vehicles Registry
- Suspect Watch List

### Communication

- SMS
- Email
- Route Maps
- PDF Incident Reports

### Integration

- VAHAN e-Vahan API
- CCTNS
- CCTV / RTSP Streams

---

# 🏗️ Project Architecture

```text
                         ┌──────────────────────┐
                         │ CCTV Cameras         │
                         │ Webcam / RTSP / File │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ Video Quality Engine │
                         │                      │
                         │ Blur                 │
                         │ Brightness           │
                         │ Contrast             │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ AI Detection Engine  │
                         │                      │
                         │ YOLOv8               │
                         │ Person / Vehicle     │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ Tracking Engine      │
                         │                      │
                         │ Object / Centroid    │
                         └──────────┬───────────┘
                                    │
                ┌───────────────────┼───────────────────┐
                ▼                   ▼                   ▼
        ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
        │ Speed Engine │    │ ANPR Engine  │    │ Face Engine  │
        │              │    │              │    │              │
        │ Vehicle      │    │ Plate + OCR  │    │ Embeddings   │
        │ Speed        │    │ + VAHAN      │    │ + Registry   │
        └──────┬───────┘    └──────┬───────┘    └──────┬───────┘
               │                   │                   │
               └───────────────────┼───────────────────┘
                                   ▼
                         ┌──────────────────────┐
                         │ Suspicious Activity  │
                         │ & Rule Engine        │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ Confidence Engine    │
                         │ Possible / Likely /  │
                         │ Confirmed             │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ Operator Dashboard   │
                         │ Human Verification   │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ Dispatch & Reporting │
                         │ PDF / Map / SMS /    │
                         │ Email                │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ Audit Trail          │
                         └──────────────────────┘
```

---

# 🌐 Deployment Architecture

The proposed deployment separates camera-side AI processing from central control and dispatch services.

```text
┌────────────────────────────────────┐
│ CCTV Cameras / Road Junctions      │
│ Webcam / RTSP Streams              │
└──────────────────┬─────────────────┘
                   │
                   ▼
┌────────────────────────────────────┐
│ Edge AI Processing                 │
│                                    │
│ Video Quality                      │
│ YOLOv8 Detection                   │
│ Tracking                           │
│ Speed Estimation                   │
│ ANPR / OCR                         │
│ Face Matching                      │
│ Activity Rules                     │
└──────────────────┬─────────────────┘
                   │
                   ▼
┌────────────────────────────────────┐
│ Central Platform                   │
│                                    │
│ Rule Engine                        │
│ Registry Verification              │
│ Reports                            │
│ Route Maps                         │
│ Dispatch                           │
│ Audit Logs                         │
└──────────────────┬─────────────────┘
                   │
         ┌─────────┼─────────┐
         ▼         ▼         ▼
      Officer    Police    Audit
     Dashboard  Station    Trail
```

The project documentation identifies edge processing with central control as the target architecture for reducing bandwidth and supporting multi-camera scaling.

---

# 📊 Expected Outcomes

The project defines measurable outcomes rather than committing to fixed performance numbers before pilot validation.

| Outcome | Measurement | Expected Direction |
|---|---|---|
| Detection Accuracy | Precision and recall on labelled Indian test footage | Higher than current baseline |
| Plate Read Success | Correct plate reads under low visibility | Higher; unreadable plates flagged honestly |
| Face Match Reliability | False-match and missed-match rates per tier | Lower false matches in Confirmed tier |
| False Alarm Rate | Alerts rejected by officers | Lower |
| Detection-to-Dispatch Time | Time from detection to station alert | Faster than manual process |
| Video Quality Score | Per-camera quality score vs baseline | Improved on weak cameras |
| Officer Workload | Manual screen watching and registry lookups | Lower |
| Connectivity Loss | Camera outages and recovery behaviour | Edge buffering and delayed synchronization |

---

# 🧪 Validation & Pilot

The pilot phase is designed to run the platform on selected junctions and compare AI-generated alerts against officer-confirmed outcomes.

The validation process includes:

1. Deploy on selected junctions
2. Measure detection results
3. Compare alerts with officer-confirmed outcomes
4. Tune detection and matching thresholds
5. Measure false-alarm rates
6. Measure detection-to-dispatch time
7. Evaluate video quality improvements
8. Validate registry integrations
9. Plan production integration with VAHAN e-Vahan API and CCTNS

The resulting pilot report is expected to include:

- Accuracy
- False-alarm rate
- Response time

---

# 🔐 Privacy, Legal Compliance & Audit

Privacy, legal compliance and auditability are identified as core project challenges.

The platform therefore includes:

- Human verification before legal action
- Confidence-based alert handling
- Original-frame retention as evidence
- Detection event logging
- Audit trails
- Registry access controls
- Restricted access to suspect watch-list information
- Explicit handling of unreadable plate results

The project documentation also identifies the need to confirm the access and approval path for VAHAN e-Vahan API and CCTNS integration before production deployment.

---

# 📦 Platform Services

The documented platform provides or targets the following service capabilities:

```text
AI-Powered CCTV Surveillance Platform
│
├── Live Video Analysis
├── Offline Video Analysis
├── Person Detection
├── Vehicle Detection
├── Object / Centroid Tracking
├── Vehicle Speed Estimation
├── ANPR + OCR
├── VAHAN Verification
├── Wanted Person Matching
├── Missing Person Matching
├── Suspicious Activity Detection
├── Video Quality Assessment
├── Confidence Tiering
├── False Alarm Reduction
├── PDF Incident Reports
├── Police Station Locator
├── Route Map Generation
├── SMS / Email Dispatch
├── Operator Review
├── Detection Event Log
└── Audit Trail
```

---

# ⚠️ Open Items

The source project documentation identifies the following items for confirmation:

- Final model approach for suspicious activity detection
- Original camera-quality notes and transcribed values
- Duplicated rows and overlapping frame ranges in source notes
- Correct RTO name
- Vehicle-class mismatch in the sample report
- Station-distance logic when camera and station share the same location
- Access and approval path for VAHAN e-Vahan API
- Access and approval path for CCTNS integration

These items should be resolved during implementation and pilot validation rather than assumed as finalized system requirements.

---

# 🖼️ Project Screenshots

<!--
## 🖥️ Operator Dashboard

<p align="center">
  <img src="images/operator-dashboard.png" alt="AI CCTV Policing Operator Dashboard" width="900">
</p>

---

## 🔢 ANPR & VAHAN Verification

<p align="center">
  <img src="images/anpr-vahan.png" alt="ANPR and VAHAN Verification" width="900">
</p>

---

## 🚨 Alert & Dispatch

<p align="center">
  <img src="images/alert-dispatch.png" alt="Police Alert and Dispatch Workflow" width="900">
</p>

---

## 📊 Analytics & Audit Trail

<p align="center">
  <img src="images/audit-dashboard.png" alt="Detection Events and Audit Trail" width="900">
</p>

---
-->

## 🚗 Vehicle Detection & Tracking

<p align="center">
  <img src="images/1.png" alt="AI Vehicle Detection and Tracking" width="900">
</p>

---
## 👤 Face Matching

<p align="center">
  <img src="images/2.png" alt="Wanted and Missing Person Face Matching" width="900">
</p>

---

# 🎥 Project Demo

<p align="center">

<a href="#">
  <img src="images/cctv-policing-demo.png"
       alt="AI-Powered CCTV Surveillance and Policing Platform Demo"
       width="900">
</a>

</p>

<p align="center">

▶️ **Click the thumbnail to watch the project demo**

</p>

---

# 🔧 Integration Roadmap

The project documentation identifies the following future integration path:

```text
Current Platform
      │
      ▼
Pilot Junction Deployment
      │
      ▼
Officer-Confirmed Validation
      │
      ▼
Threshold & Accuracy Tuning
      │
      ▼
VAHAN e-Vahan API Integration
      │
      ▼
CCTNS Integration
      │
      ▼
Multi-Camera Deployment
      │
      ▼
Centralized Public-Safety Operations
```

---

# 📄 License

This repository's licensing terms should be defined by the project owner before public distribution.

If this repository contains proprietary code, trained models, police data, registry information, or operational CCTV footage, access and redistribution should be controlled according to the applicable project, organizational, legal and data-governance requirements.

---

<p align="center">

### 🚔 AI-Assisted Public Safety Through Intelligent CCTV

**AI • Computer Vision • ANPR • Face Matching • Tracking • Registry Verification • Human-in-the-Loop • Smart Dispatch**

</p>
