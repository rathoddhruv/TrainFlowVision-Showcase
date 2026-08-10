# ✨ Core Features

### 🛠️ Interactive Annotation & Review
- **Human-in-the-Loop:** Fast, hotkey-driven canvas for bounding box and polygon editing with full Undo/Redo support.
- **Ensemble Review:** Compare predictions from multiple models (e.g. Teacher vs Student) side-by-side to catch false negatives. Visual borders differentiate model predictions vs human corrections.
- **Visual Confidence:** Clearly separates trusted human annotations from unverified machine predictions.

### 🧠 Advanced MLOps & Experiment Tracking
- **Neural History:** Treats models like Git branches. Track mAP, Precision, Recall, and Loss across every training run. Turns training into a traceable experiment workflow instead of a black box.
- **Model Registry:** PostgreSQL backed registry tying every weight file to the exact dataset version and hyperparameter config. Group models by Recommended, Experimental, and Legacy states (e.g. YOLO26, YOLO12).
- **Safe Rollbacks:** Instantly revert to a previous model state if a new training epoch degrades detection quality.
- **Hardware-Aware:** Real-time CUDA/CPU utilization streaming and dynamic compute fallbacks. Safe Auto Batch logic for large models.

### 🚁 Edge & Drone Ready
- **ONNX Export:** One-click conversion from PyTorch to highly optimized `.onnx` artifacts.
- **Isolated Edge Client:** A completely decoupled client utilizing pure `onnxruntime-gpu`, ready for deployment on Jetson Nano or Raspberry Pi.
- **Geotag Metadata:** Native support for uploading and parsing drone telemetry JSON sidecars (GPS coordinates, altitude, pitch) alongside images for spatial dataset generation.

### 🔮 Teacher-Student Active Learning Workflow
- **Preview / Teacher Model**: Use a large, highly accurate model to generate initial predictions or label suggestions.
- **Student / Training Model**: Train a smaller, faster model using the human-reviewed labels derived from the teacher.
- **Workflow**: Large model predicts → Human corrects → Corrections feed into smaller model training → Refined smaller model is deployed to the edge.
