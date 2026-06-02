# Embedded AI & Aerospace Navigation — Learning Roadmap

> **A structured, hands-on curriculum from signal theory to real-time embedded perception systems.**  
> Designed for engineers targeting research or teaching positions in aerospace embedded intelligence.

---

## Motivation

This repository documents a complete upskilling path toward **embedded AI and sensor fusion for aerospace systems** — covering the theoretical foundations, algorithmic implementations, and practical deployments expected in research-oriented engineering education (e.g. *Majeure SEN-Aero* profiles).

It is built as a **living curriculum**: each module combines formal theory, annotated notebooks, simulations, and a deployable project.

---

## Who this is for

- Engineers with a background in signal processing or ML who want to specialize in **navigation, estimation, and embedded AI**
- Educators building courses around **perception, sensor fusion, and aerospace systems**
- Anyone preparing for a doctoral or research position at the intersection of **signal, AI, and autonomous systems**

---

## Repository structure

```
embedded-ai-aerospace-roadmap/
│
├── 00-foundations/          ← Probability, linear algebra, signals, state-space
├── 01-kalman-filter/        ← KF theory, 1D/nD implementations, exercises
├── 02-sensor-fusion/        ← GPS+IMU, dead reckoning, multi-rate fusion
├── 03-extended-kalman-filter/ ← EKF, Jacobians, nonlinear models, drone nav
├── 04-particle-filter/      ← PF theory, localization, EKF vs PF comparison
├── 05-robotics-perception/  ← ROS2, OpenCV, LiDAR, SLAM introduction
├── 06-embedded-ai/          ← TinyML, quantization, TFLite, ONNX, benchmarks
├── 07-aerospace-use-cases/  ← Drone estimation, GNSS denoising, Digital Twin
├── 08-capstone-projects/    ← 4 full end-to-end projects
└── docs/                    ← Glossary, bibliography, teaching notes
```

---

## Learning path

```
Phase 1 (months 1–3)   Foundations + Kalman Filter + Sensor Fusion
Phase 2 (months 3–6)   EKF + Particle Filter + Robotics/Perception
Phase 3 (months 6–9)   Embedded AI + Edge Inference
Phase 4 (months 9–12)  Aerospace Use Cases + Capstone Projects
```

See [`ROADMAP.md`](ROADMAP.md) for the detailed week-by-week plan.  
See [`SKILLS_MATRIX.md`](SKILLS_MATRIX.md) for the competency map against target job profiles.

---

## Prerequisites

| Domain | Expected level |
|--------|---------------|
| Python (NumPy, SciPy, Matplotlib) | Intermediate |
| Signal processing (Fourier, filtering) | Intermediate |
| Linear algebra | Intermediate |
| Machine learning (supervised, CNNs) | Intermediate |
| C/C++ basics | Beginner |

---

## Environment setup

### Option A — Conda (recommended)
```bash
conda env create -f environment.yml
conda activate aerospace-ai
```

### Option B — pip
```bash
pip install -r requirements.txt
```

### Option C — Docker
```bash
docker build -t aerospace-ai ./docker
docker run -it -p 8888:8888 aerospace-ai
```

---

## Module overview

### `00-foundations`
Core mathematics required across all modules: probability theory, Bayesian inference, multivariate Gaussians, state-space representations. No aerospace context yet — pure rigour.

### `01-kalman-filter`
Full derivation of the linear Kalman filter. 1D position tracking → multi-dimensional systems → tuning Q and R. Exercises with reference solutions.

### `02-sensor-fusion`
GPS + IMU fusion from scratch. Dead reckoning, drift analysis, covariance propagation. Introduction to multi-rate sensor architectures.

### `03-extended-kalman-filter`
EKF derivation via first-order linearisation. Jacobian computation (analytical and numerical). 2D drone navigation simulation with noise injection.

### `04-particle-filter`
Sequential Monte Carlo methods. Resampling strategies. Localization benchmark: EKF vs PF under non-Gaussian noise.

### `05-robotics-perception`
ROS2 fundamentals, topic architecture, sensor drivers. OpenCV for embedded vision. LiDAR pointcloud processing. Introduction to graph SLAM.

### `06-embedded-ai`
TinyML pipeline: training → quantization → deployment. TensorFlow Lite and ONNX Runtime on ARM targets (Raspberry Pi, STM32, Jetson). Inference latency benchmarks.

### `07-aerospace-use-cases`
Applied domain modules: drone state estimation, satellite attitude control, GNSS signal denoising, Digital Twin with real-time telemetry dashboard.

### `08-capstone-projects`
Four end-to-end projects integrating all prior modules. Each includes a full technical report written as a teaching case study.

---

## Teaching philosophy

Each module is structured for **dual use**:
1. **Self-study** — progressive notebooks with theory → implementation → validation
2. **Course material** — each notebook can be stripped to an exercise version (`_exercises.ipynb`) with guided blanks

Teaching notes and slide outlines are in [`docs/teaching-notes.md`](docs/teaching-notes.md).

---

## Bibliography

See [`docs/bibliography.md`](docs/bibliography.md) for the full reference list.  
Key references:
- Thrun, Burgard, Fox — *Probabilistic Robotics* (MIT Press)
- Simon — *Optimal State Estimation* (Wiley)
- Labbe — *Kalman and Bayesian Filters in Python* (open access)
- Groves — *Principles of GNSS, Inertial, and Multisensor Integrated Navigation Systems*

---

## Aerospace standards (context)

Modules 07 and 08 introduce vocabulary and constraints from:
- **DO-178C** — Software considerations in airborne systems
- **ARP4754A** — Development of civil aircraft and systems
- **DAL (Design Assurance Level)** — Criticality classification

---

## Contributions & roadmap evolution

This repository is under active development. Planned additions:
- [ ] Module 09 — SLAM advanced (factor graphs, g2o)
- [ ] Module 10 — Aerospace certification primer (DO-178C deep dive)
- [ ] Hardware lab — STM32 + MPU-6050 real sensor fusion
- [ ] French versions of teaching notes (for francophone engineering schools)

---

## License

MIT License — see `LICENSE`.  
All datasets used are either synthetic (generated in-repo) or open-source licensed.

---

*Built with rigour. Designed to teach.*
