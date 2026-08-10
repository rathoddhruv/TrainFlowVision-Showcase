# 🔬 Technical Deep Dive

TrainFlowVision is a full-stack active learning platform engineered to address the complexities of real-world computer vision deployments.

## The Active Learning Lifecycle

A real ML workflow needs robust infrastructure surrounding the training script. TrainFlowVision focuses on the complete feedback loop:

```
Upload data ➡️ Review predictions ➡️ Fix annotations ➡️ Train or refine the model ➡️ Track metrics ➡️ Compare history ➡️ Restore older models ➡️ Improve through active learning
```

### 1. Teacher-Student Workflow
TrainFlowVision supports a teacher-student workflow to make model training more flexible and future-ready:
* **Teacher Model**: A large or highly accurate model used to generate initial predictions or label suggestions for review.
* **Student Model**: A smaller, faster model trained using the human-reviewed labels generated from the teacher predictions.
* **Why it matters**: Large models improve labeling quality, but small models are needed for fast inference on edge devices or drones. 
* **The Rule**: The Teacher does not directly train the Student. The Teacher predicts, the Human reviews, and the Reviewed labels train the Student.

### 2. Annotation Review & Ensemble Mode
Real-world annotations are rarely perfect on the first try. The annotation review UI allows users to quickly correct close detections (move, resize, relabel) instead of deleting and redrawing everything. 

An optional **Ensemble Review Mode** allows comparing Teacher and Student predictions on the same image simultaneously. Visual rules (e.g. dashed vs solid borders) quickly highlight what the larger Teacher model detects and what the smaller Student model misses.

### 3. Neural History & Traceability
Neural History turns training into a traceable experiment workflow instead of a black box. It tracks:
- Dataset version & source
- Training configuration & device used
- Precision, Recall, mAP50, mAP50-95, Loss values
- Best/Last model paths
- Rollback availability

### 4. Edge Deployment & Metadata
- **ONNX Export**: Ensures the final Student model is packaged in a lightweight, high-performance format.
- **Geotag Sidecars**: Metadata JSON sidecars containing GPS telemetry (latitude, longitude, altitude) are natively parsed and persisted alongside images, preparing the data for drone-based spatial mapping workloads.
