# 🚀 TrainFlowVision: Full-Stack Active Learning for Computer Vision

> **Note:** This is a public showcase repository. The production source code is private because the project contains custom ML workflow logic, training orchestration, and deployment architecture. Code access can be provided privately for serious technical review.

<div align="center">
  
[![Angular](https://img.shields.io/badge/Angular-DD0031?style=for-the-badge&logo=angular&logoColor=white)](https://angular.io/)
[![FastAPI](https://img.shields.io/badge/FastAPI-005571?style=for-the-badge&logo=fastapi)](https://fastapi.tiangolo.com/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-%23EE4C2C.svg?style=for-the-badge&logo=PyTorch&logoColor=white)](https://pytorch.org/)
[![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)

**TrainFlowVision** is an end-to-end MLOps and Active Learning platform designed to bridge the gap between raw data and production-ready edge AI models.

[Features](#-key-features) • [Architecture](#%EF%B8%8F-architecture--mlops-pipeline) • [Deep Dives](#-documentation-deep-dives) • [Roadmap](#-roadmap--future-vision)

</div>

---

## 💡 The Problem We Solve
Training a computer vision model (like **YOLOv8** or **YOLO26**) is easy. Managing the messy lifecycle of data, however, is incredibly hard. 

Most models fail in production because they don't have a reliable feedback loop. **TrainFlowVision** solves this by providing a unified platform where you can:
1. **Generate pseudo-labels** using heavy "Teacher" AI models.
2. **Quality-gate** the data through a lightning-fast, human-in-the-loop Angular dashboard.
3. **Automatically retrain** lightweight "Student" models.
4. **Deploy** optimized `.onnx` models directly to drones and edge devices.

Whether you are doing object detection, segmentation, or drone-based telemetry mapping, TrainFlowVision treats your **datasets and models like code**—versioned, tracked, and restorable.

---

## ✨ Key Features

### 🛠️ Interactive Annotation & Review
- **Human-in-the-Loop:** Fast, hotkey-driven canvas for bounding box and polygon editing with full Undo/Redo support.
- **Ensemble Review:** Compare predictions from multiple models side-by-side to catch false negatives.
- **Visual Confidence:** Clearly separates trusted human annotations from unverified machine predictions.

### 🧠 Advanced MLOps & Experiment Tracking
- **Neural History:** Treats models like Git branches. Track mAP, Precision, Recall, and Loss across every training run.
- **Model Registry:** PostgreSQL backed registry tying every weight file to the exact dataset version and hyperparameter config.
- **Safe Rollbacks:** Instantly revert to a previous model state if a new training epoch degrades detection quality.
- **Hardware-Aware:** Real-time CUDA/CPU utilization streaming and dynamic compute fallbacks.

### 🚁 Edge & Drone Ready
- **ONNX Export:** One-click conversion from PyTorch to highly optimized `.onnx` artifacts.
- **Isolated Edge Client:** A completely decoupled client utilizing pure `onnxruntime` and `numpy`, ready for deployment on Jetson Nano or Raspberry Pi.
- **Geotag Metadata:** Native support for uploading and parsing drone telemetry JSON sidecars (GPS coordinates, altitude, pitch) alongside images for spatial dataset generation.

---

## 🏗️ Architecture & MLOps Pipeline

TrainFlowVision is built on a strict microservice boundary, separating the reactive UI, the async orchestration API, and the heavy PyTorch execution context.

```mermaid
graph TD
    subgraph Frontend [Angular 18 Client]
        UI[Interactive Review Dashboard]
        State[RxJS / Signals State Management]
    end

    subgraph Backend [FastAPI Orchestrator]
        API[Async API Gateway]
        Queue[Active Learning Queue]
    end

    subgraph Data [Persistence]
        DB[(PostgreSQL Model Registry)]
        Storage[(File Storage / Datasets)]
    end

    subgraph ML_Engine [PyTorch / Ultralytics]
        Train[Asynchronous Model Training]
        Infer[Teacher / Student Inference]
    end

    subgraph Edge [Edge Deployment]
        ONNX[Exported .onnx Model]
        Jetson[Drone / IoT Inference Scripts]
    end

    UI <-->|REST / WebSockets| API
    State --> UI
    API --> DB
    API --> Storage
    API --> Train
    API --> Infer
    Train --> ONNX
    ONNX --> Jetson
```

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

## 📚 Documentation Deep Dives

For a detailed look at the internal mechanics of the project, please explore the documentation:
- [Hiring Manager Brief](docs/HIRING_MANAGER_BRIEF.md)
- [Technical Deep Dive (Active Learning Loop)](docs/TECHNICAL_DEEP_DIVE.md)
- [Extended Features List](docs/FEATURES.md)
- [Architecture Details](docs/ARCHITECTURE.md)

---

## 🛣️ Roadmap & Future Vision

TrainFlowVision is continuously expanding to support enterprise-grade computer vision deployments.

- [x] **PyTorch to ONNX Pipeline** (Completed)
- [x] **Geotagged Drone Metadata Support** (Completed)
- [ ] **Jetson Edge Client Integration** (In Progress)
- [ ] **Live RTSP Video Stream Inference**
- [ ] **Autonomous MAVLink Drone Integration**
- [ ] **Cloud Provider Agnostic Workers (RunPod/AWS)**

---

## 👨‍💻 For Hiring Managers & Technical Recruiters

If you are evaluating my profile for a **Full-Stack**, **Machine Learning Engineer**, or **MLOps** role, this repository serves as a comprehensive portfolio of my engineering capabilities. It was built from the ground up to demonstrate production-ready software design:

- **End-to-End System Architecture:** Proves the ability to design, build, and deploy a complex, multi-tiered application (UI, REST API, Database, ML Engine, Edge Client).
- **Frontend Mastery (Angular 18):** Demonstrates deep understanding of reactive programming (RxJS), modern state management (Signals), and high-performance DOM updates required for interactive HTML5 canvas rendering (Bounding boxes & Polygons).
- **Backend & Async Orchestration (FastAPI):** Showcases ability to handle complex concurrency. The backend safely orchestrates background PyTorch training threads without blocking the main API event loop, ensuring the UI remains responsive under heavy GPU load.
- **Database Design (PostgreSQL):** Highlights relational database modeling. Every model weight file is tied directly to the exact dataset version and hyperparameter configuration used to train it, ensuring perfect auditability.
- **Applied AI & MLOps:** Moves beyond basic "Jupyter Notebook data science" by implementing real-world MLOps patterns: Teacher-Student distillation, active learning pipelines, neural history tracking, and PyTorch-to-ONNX optimizations for edge hardware (Jetson Nano/Drones).

I build systems that solve real business problems—handling the messy reality of data collection, continuous human feedback, and hardware-constrained deployments.

---
<div align="center">
<i>Engineered for the future of computer vision and edge AI.</i>
</div>
