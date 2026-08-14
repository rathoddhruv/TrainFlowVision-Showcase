# 🚀 TrainFlowVision: Full-Stack Active Learning for Computer Vision

> **Note:** This is a public showcase repository. The production source code is private because the project contains custom ML workflow logic, training orchestration, and deployment architecture. Code access can be provided privately for serious technical review.

<div align="center">
  
[![Angular](https://img.shields.io/badge/Angular_18-DD0031?style=for-the-badge&logo=angular&logoColor=white)](https://angular.io/)
[![FastAPI](https://img.shields.io/badge/FastAPI-005571?style=for-the-badge&logo=fastapi)](https://fastapi.tiangolo.com/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-%23EE4C2C.svg?style=for-the-badge&logo=PyTorch&logoColor=white)](https://pytorch.org/)
[![TensorRT](https://img.shields.io/badge/TensorRT-76B900?style=for-the-badge&logo=nvidia&logoColor=white)](https://developer.nvidia.com/tensorrt)
[![Docker](https://img.shields.io/badge/Docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white)](https://opencv.org/)

**TrainFlowVision is an end-to-end computer vision MLOps platform for training, reviewing, refining, versioning, and safely deploying YOLO models across desktop, edge devices, and drone simulation.**

[Features](#-key-features) • [Architecture](#%EF%B8%8F-architecture--mlops-pipeline) • [Edge Deployment](#-edge-ai--tensorrt) • [Deep Dives](#-documentation-deep-dives)

</div>

---

## ⏱️ The 30-Second Summary

- **What it is:** A platform that ingests drone video feeds, extracts meaningful frames, allows humans to correct model mistakes, and automatically retrains and safely promotes the AI model.
- **Why it matters:** It demonstrates the ability to architect complex systems bridging UI/UX, robust backend pipelines, custom ML training loops, and robotics simulation.
- **The Tech Stack:** Angular, FastAPI, PostgreSQL, YOLO, PyTorch, FFmpeg/CUDA, PX4 Gazebo, and MAVSDK.

---

## 💡 The Problem We Solve
Training a computer vision model is easy. Managing the messy lifecycle of data, however, is incredibly hard. 

Most models fail in production because they don't have a reliable feedback loop. **TrainFlowVision** connects full-stack software, machine learning operations, edge AI deployment, and robotics simulation into one cohesive workflow. Models improve through continuous human-in-the-loop review, rigorous corrections, automated evaluation, strict versioning, and safe deployment.

---

## 📸 Platform Screenshots

<details open>
<summary><b>1. Annotation Review Dashboard</b></summary>
<br>
<img src="screenshots/review.png" alt="Annotation Review Dashboard" width="800"/>
</details>

<details open>
<summary><b>2. Neural History, Metrics & Dashboard</b></summary>
<br>
<img src="screenshots/dashboard.png" alt="Neural History and Dashboard" width="800"/>
</details>

---

## 1. End-to-End Active Learning Pipeline

```mermaid
flowchart TD
    subgraph Data Intake
        I1[Manual Image Upload]
        I2[Video Upload]
        I3[Simulation Downward Camera]
        I4[Future Jetson Camera Stream]
    end

    subgraph Backend Processing
        P1[Frame Processing Backend]
        P2[Smart Frame Extraction]
        P3[Similar Frame Grouping]
        P4[Representative Keyframes]
    end

    subgraph Human-in-the-Loop
        H1[Review Queue]
        H2[Human Correction]
    end

    subgraph Database & MLOps
        D1[(ReviewCorrection DB)]
        D2[Refinement Dataset Builder]
        D3[YOLO Fine-Tuning]
        D4[Evaluation Old vs New]
        D5{Promotion Guard}
        D6[(Neural History)]
        D7[Restore / Rollback]
    end
    
    subgraph Deployment
        E1[Edge Export]
        E2[Jetson / Drone Simulation Loop]
    end

    I1 & I2 & I3 & I4 --> P1
    P1 --> P2
    P2 --> P3
    P3 --> P4
    P4 --> H1
    H1 --> H2
    H2 --> D1
    D1 --> D2
    D2 --> D3
    D3 --> D4
    D4 --> D5
    D5 -- Pass --> D6
    D5 -- Fail --> D7
    D6 --> E1
    E1 --> E2
    E2 -- "Collect New Edge Cases" --> I3
```

---

## 2. Architecture Diagram

```mermaid
flowchart LR
    subgraph Frontend
        FE(Angular UI)
    end

    subgraph Backend API
        BE(FastAPI)
        AL(Active Learning Services)
        VB(Video Processing Backend)
    end

    subgraph Storage
        DB[(PostgreSQL)]
        FS[(Model Storage)]
    end

    subgraph ML Pipeline
        ML(YOLO Training/Inference)
    end

    subgraph Robotics & Edge
        SIM(PX4/Gazebo Simulation)
        JET(Jetson Edge Client)
        HW(Future Holybro X650)
    end

    FE <--> BE
    BE <--> AL
    BE <--> VB
    BE <--> DB
    BE <--> FS
    AL <--> ML
    ML <--> FS
    SIM --> VB
    JET --> VB
    HW -.-> JET
```

---

## 3. Drone Simulation Workflow

```mermaid
flowchart TD
    A[PX4 SITL] <--> B[Gazebo World]
    B --> C[x500_mono_cam_down]
    C --> D[GStreamer UDP Feed]
    D --> E[OpenCV Capture]
    E --> F[YOLO Detector]
    F --> G[Target Alignment]
    G --> H[Safe Command Limiter]
    H --> I[State Machine]
    I --> J[MAVSDK Control]
    J <--> A
    I -- Emergency/End --> K[Hold / Land]
```

---

## 4. Review Issue Types

The issue type defines *what* mistake the model made. Geometry types dynamically adapt to the active model task (`detect` = bbox, `segment` = polygon, `obb` = rotated box).

```mermaid
pie title Correction Issue Types
    "false_positive" : 20
    "false_negative" : 15
    "misclassification" : 25
    "bad_box" : 10
    "bad_mask" : 10
    "low_confidence" : 15
    "domain_gap" : 5
```

*(Note: If the model predicts "Sunflower" in Gazebo, it doesn't mean a sunflower exists in the virtual world. It means the drone's dandelion marker is being misclassified due to a domain gap. This is exactly what the active-learning cycle solves.)*

---

## 5. Active Learning Intake

A raw drone video can contain thousands of near-identical frames. TrainFlowVision is designed so that humans **never review redundant frames**. 

Instead, the pipeline:
1. Probes the video file.
2. Uses smart frame extraction (Auto, 0.5 FPS, 1 FPS, 2 FPS).
3. Groups visually similar frames via hashing and IoU overlap.
4. Selects a single **representative keyframe** per group.
5. Lets the human correct only the keyframes.
6. Preserves the represented frame count to retain data weight.
7. Builds a clean refinement dataset explicitly from human-reviewed or safely propagated corrections.

**Example Intake Summary:**
> **Video Uploaded:** `garden_flight.mp4`  
> **Total Frames:** `4,800`  
> **Extracted Frames:** `320`  
> **Grouped Similar Frames:** `296`  
> **Review Keyframes:** `24`  
> **Human Review Saved:** `4,776 frames avoided`  
> **Processing Backend:** `FFmpeg CUDA (if available), otherwise CPU fallback`

---

## 6. Fine-Tuning and Promotion Guard

A model is never blindly auto-promoted simply because it performed well in a synthetic simulation. It must pass strict evaluation checks:
- Simulated marker-hover frames at various altitudes (5m, 4m, 3m, 2m, 1.5m).
- **Real-world dandelion validation images** (when available).
- Sunflower/Dandelion confusion edge cases.

**If real-world validation data is missing:**
- The training pipeline is permitted to run.
- Simulation evaluation metrics are saved.
- **Auto-promotion is blocked.**
- The `promotion_status` is explicitly set to `needs_real_world_validation`.

This enforces safety by ensuring models do not dangerously overfit to Gazebo graphics before flying on a real drone.

---

## 7. Project Status

We believe in technical honesty. Simulation flight is not equal to real drone flight, and hardware integration is treated with strict safety protocols.

✅ **Completed:**
- Angular frontend review workflow & FastAPI backend APIs.
- PostgreSQL metadata and ReviewCorrection persistence.
- YOLO model training, inference, and lineage tracking (Neural History).
- Pluggable video processing backend with smart frame extraction (Auto, 0.5 FPS, 1 FPS, 2 FPS).
- Similar frame grouping to prevent Review UI flooding.
- Human-reviewed refinement dataset builder and fine-tuning engine.
- Strict Evaluation and Promotion Guard preventing unsafe model deployment.
- PX4 Gazebo SITL simulation in WSL2 with downward camera feeds and MAVSDK control.

🔄 **In Progress:**
- High-altitude model detection improvements in simulation.
- Real-world validation image collection.
- Autonomous visual tracking logic.

📅 **Planned:**
- Live field testing on NVIDIA Jetson hardware.
- Physical integration with the Holybro X650 + Pixhawk 6X (tethered safety tests first).
- Real camera mount vibration validation and emergency geofence testing.

---

## 8. Safety and Honesty

- **Simulation != Reality**: Simulation flight is not equal to real drone flight.
- **Physical Safety First**: Real drone work requires tested manual overrides, strict geofencing, emergency stop integration, physical vibration testing, and Pixhawk safety validation.
- **Edge Inference**: The Jetson should run inference and upload learning frames. Heavy GPU training runs on a desktop/server, *not* on the drone during flight.
- **Live Learning Scope**: Near-live active learning is supported through frame upload and review. The drone can send low-confidence or confusing frames back to the backend. Human review and retraining happen safely offboard. The refined model is then versioned and exported back to the Jetson.

---

## 👨‍💻 For Hiring Managers & Technical Recruiters

If you are evaluating my profile for a **Full-Stack**, **Machine Learning Engineer**, or **MLOps** role, this repository serves as a comprehensive portfolio of my engineering capabilities. It was built from the ground up to demonstrate production-ready software design across the entire stack:

- **End-to-End System Architecture:** Proves the ability to design, build, and deploy a complex, multi-tiered application (UI, REST API, Database, ML Engine, Edge Client, Robotics Simulator).
- **Frontend Mastery (Angular 18):** Demonstrates deep understanding of reactive programming (RxJS), modern state management (Signals), and high-performance DOM updates required for interactive HTML5 canvas rendering.
- **Backend & Async Orchestration (FastAPI):** Showcases ability to handle complex concurrency. The backend safely orchestrates background PyTorch training threads without blocking the main API event loop, ensuring the UI remains responsive under heavy GPU load.
- **Database Design & MLOps Safety (PostgreSQL):** Highlights relational database modeling. Every model weight file is tied directly to the exact dataset version, human corrections, and hyperparameter configuration used to train it. Built-in promotion guards block unsafe auto-promotions.
- **Applied AI & Edge Deployments:** Moves beyond basic "Jupyter Notebook data science" by implementing real-world MLOps patterns: active learning pipelines, neural history tracking, and compiling custom TensorRT `.engine` models for bare-metal NVIDIA Jetson Orin NX hardware.
- **Advanced Systems Engineering:** Demonstrates complex problem-solving by implementing visual hashing to deduplicate thousands of drone video frames to prevent flooding the Review UI, and seamlessly falling back between FFmpeg CUDA GPU processing and standard OpenCV CPU.

I build systems that solve real business problems—handling the messy reality of data collection, continuous human feedback, hardware-constrained deployments, and simulation safety.

---
<div align="center">
<i>Engineered for the future of computer vision and edge AI.</i>
</div>
