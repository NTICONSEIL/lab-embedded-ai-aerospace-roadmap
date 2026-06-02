# Skills Matrix — SEN-Aero Target Profile

> Self-assessment grid aligned with the competency profile expected for a *Enseignant-Chercheur*  
> position in embedded systems and aerospace AI (ESME SEN-Aero typology).

---

## How to use this matrix

For each skill:
- **Level 0** — No knowledge
- **Level 1** — Theoretical awareness (can explain the concept)
- **Level 2** — Can implement from scratch or teach the topic
- **Level 3** — Can produce research-grade work, benchmark, or innovate

Update this file at the end of each phase with your honest self-assessment.

---

## Domain 1 — Signal Processing & Estimation

| Skill | Required level | Start | Phase 1 | Phase 2 | Phase 3 | Phase 4 |
|-------|---------------|-------|---------|---------|---------|---------|
| Discrete-time signal theory | 2 | | | | | |
| Noise models (white, colored, drift) | 2 | | | | | |
| Allan Variance (IMU characterisation) | 2 | | | | | |
| State-space representation | 3 | | | | | |
| Bayesian inference | 3 | | | | | |
| Multivariate Gaussian distributions | 3 | | | | | |
| Kalman Filter (linear) | 3 | | | | | |
| Extended Kalman Filter (EKF) | 3 | | | | | |
| Unscented Kalman Filter (UKF) | 2 | | | | | |
| Particle Filter | 2 | | | | | |
| Filter consistency analysis (NIS, NEES) | 2 | | | | | |

---

## Domain 2 — Sensor Fusion

| Skill | Required level | Start | Phase 1 | Phase 2 | Phase 3 | Phase 4 |
|-------|---------------|-------|---------|---------|---------|---------|
| IMU error modelling (bias, drift, noise) | 3 | | | | | |
| Dead reckoning (inertial navigation) | 3 | | | | | |
| GPS/GNSS measurement model | 2 | | | | | |
| Loose coupling GPS+IMU | 3 | | | | | |
| Tight coupling GPS+IMU | 2 | | | | | |
| Multi-rate sensor fusion | 2 | | | | | |
| Camera-IMU fusion (visual-inertial) | 1 | | | | | |
| LiDAR-IMU fusion | 1 | | | | | |
| Radar integration | 1 | | | | | |
| Covariance analysis & observability | 2 | | | | | |

---

## Domain 3 — Robotics & Perception

| Skill | Required level | Start | Phase 1 | Phase 2 | Phase 3 | Phase 4 |
|-------|---------------|-------|---------|---------|---------|---------|
| ROS2 architecture | 2 | | | | | |
| ROS2 Python nodes (pub/sub/service) | 2 | | | | | |
| Camera calibration | 2 | | | | | |
| Feature detection & matching | 2 | | | | | |
| Optical flow | 2 | | | | | |
| LiDAR point cloud processing | 2 | | | | | |
| SLAM (graph-based, EKF-SLAM) | 2 | | | | | |
| Gazebo simulation | 2 | | | | | |
| Autonomous navigation (Nav2) | 1 | | | | | |

---

## Domain 4 — Embedded AI

| Skill | Required level | Start | Phase 1 | Phase 2 | Phase 3 | Phase 4 |
|-------|---------------|-------|---------|---------|---------|---------|
| Model design for edge (MobileNet family) | 2 | | | | | |
| Post-training quantisation (INT8, FP16) | 3 | | | | | |
| Quantisation-aware training (QAT) | 2 | | | | | |
| Knowledge distillation | 2 | | | | | |
| Pruning | 2 | | | | | |
| TensorFlow Lite (conversion + deployment) | 3 | | | | | |
| ONNX Runtime on ARM | 3 | | | | | |
| Latency / throughput benchmarking | 3 | | | | | |
| STM32 / microcontroller deployment | 2 | | | | | |
| Jetson Nano/Orin (CUDA edge inference) | 2 | | | | | |

---

## Domain 5 — Aerospace Systems

| Skill | Required level | Start | Phase 1 | Phase 2 | Phase 3 | Phase 4 |
|-------|---------------|-------|---------|---------|---------|---------|
| Rigid body dynamics (6-DOF) | 2 | | | | | |
| Quaternion attitude representation | 3 | | | | | |
| Drone/UAV kinematics | 2 | | | | | |
| MAVLink protocol | 1 | | | | | |
| PX4 / ArduPilot architecture | 1 | | | | | |
| Digital Twin concept & implementation | 2 | | | | | |
| Real-time telemetry dashboards | 2 | | | | | |
| DO-178C (software certification) — awareness | 1 | | | | | |
| ARP4754A (system development) — awareness | 1 | | | | | |
| DAL (Design Assurance Level) vocabulary | 1 | | | | | |

---

## Domain 6 — Research & Teaching

| Skill | Required level | Start | Phase 1 | Phase 2 | Phase 3 | Phase 4 |
|-------|---------------|-------|---------|---------|---------|---------|
| Scientific writing (English) | 3 | | | | | |
| Literature survey methodology | 2 | | | | | |
| Conference paper structure | 2 | | | | | |
| Course design (objectives → assessment) | 3 | | | | | |
| TP / lab session design | 3 | | | | | |
| Supervision of student projects | 2 | | | | | |
| Industrial partnership development | 2 | | | | | |
| CNU Section 61 profile awareness | 1 | | | | | |

---

## Gap analysis — current vs target

> *Fill in at the start of the roadmap. Re-evaluate at each phase checkpoint.*

### Strengths to leverage
- Strong signal processing background (ENSEIRB-MATMECA)
- ML/DL pipeline experience (CNN, LSTM, time series)
- Pedagogical maturity (course design, TPs, student supervision)
- Systems architecture (distributed, real-time pipelines)

### Priority gaps to close (Phase 1)
1. Bayesian estimation → Kalman Filter derivation from scratch
2. IMU error modelling and Allan Variance
3. GPS measurement model

### Priority gaps to close (Phase 2)
1. EKF with Jacobian computation
2. ROS2 practical usage
3. Particle Filter for localization

### Priority gaps to close (Phase 3)
1. TFLite and ONNX deployment on ARM hardware
2. Quantization-aware training
3. STM32 C++ inference

### Priority gaps to close (Phase 4)
1. Quaternion attitude estimation (MEKF)
2. DO-178C vocabulary
3. Research writing and conference submission

---

## Module → skills traceability

| Module | Skills covered |
|--------|---------------|
| `00-foundations` | Bayesian inference, state-space, noise models |
| `01-kalman-filter` | KF linear, NIS/NEES, Q/R tuning |
| `02-sensor-fusion` | IMU model, dead reckoning, GPS fusion, multi-rate |
| `03-EKF` | EKF, Jacobians, CTRV model |
| `04-particle-filter` | PF, resampling, EKF vs PF comparison |
| `05-robotics` | ROS2, camera, LiDAR, SLAM |
| `06-embedded-ai` | Quantization, TFLite, ONNX, ARM deployment |
| `07-aerospace` | 6-DOF, quaternions, Digital Twin, DO-178C |
| `08-capstone` | All domains integrated |
