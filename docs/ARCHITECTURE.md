# 🏗️ Architecture & MLOps Pipeline

TrainFlowVision is designed as a modular full-stack ML platform, not just a single training script. The architecture separates Frontend, Backend, database, ML orchestration, dataset management, model history, and future deployment concerns.

## Data Flow
```mermaid
sequenceDiagram
    participant U as User (Angular FE)
    participant A as API (FastAPI)
    participant DB as PostgreSQL
    participant ML as PyTorch Engine

    U->>A: Upload Images & Telemetry (JSON)
    A->>DB: Save Dataset Version
    A->>ML: Run Teacher Model Inference
    ML-->>A: Pseudo-labels
    A-->>U: Load Review Dashboard
    U->>U: Human-in-the-loop Fixes (Bounding Boxes)
    U->>A: Submit Reviewed Data
    A->>DB: Update Trusted Dataset
    A->>ML: Start Async Student Model Training
    ML->>DB: Stream Live GPU Logs / mAP Metrics
    ML-->>A: Output .onnx Edge Model
```

## Microservice Boundaries
- **Frontend (Angular 18)**: Manages state using RxJS and Signals. Handles complex HTML5 canvas rendering for interactive bounding box/polygon editing.
- **Backend (FastAPI)**: Safely orchestrates background PyTorch training threads without blocking the main API event loop. 
- **Database (PostgreSQL)**: Complex SQLAlchemy schemas govern the relationship between uploaded data, human reviews, training configurations, and emitted weights. Ensures relational integrity and auditability.
- **Edge Deployment (Standalone)**: A completely decoupled `edge/` client utilizing pure `onnxruntime` and `numpy`.
