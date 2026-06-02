# Roadmap — Embedded AI & Aerospace Navigation

> Detailed 12–18 month learning plan. Each phase has clear goals, deliverables, and checkpoints.

---

## Overview

| Phase | Duration | Focus | Key deliverable |
|-------|----------|-------|----------------|
| Phase 1 | Months 1–3 | Foundations + Kalman Filter + Sensor Fusion | EKF GPS+IMU simulator |
| Phase 2 | Months 3–6 | EKF + Particle Filter + Robotics/Perception | ROS2 drone simulation |
| Phase 3 | Months 6–9 | Embedded AI + Edge Inference | Edge anomaly detector |
| Phase 4 | Months 9–12 | Aerospace Use Cases + Capstone | Digital Twin dashboard |
| Phase 5 (optional) | Months 12–18 | Research track + publications | Conference paper |

---

## Phase 1 — Probabilistic Foundations & Kalman Filter
**Goal:** Become credible on state estimation and navigation theory.

### Month 1 — Mathematical Foundations (`00-foundations`)

**Week 1 — Probability & Bayesian inference**
- [ ] Conditional probability, Bayes' theorem
- [ ] Gaussian distributions, multivariate Gaussians
- [ ] Prior / likelihood / posterior — intuition and computation
- [ ] 📓 Notebook: `00-foundations/probabilities-estimation/01_bayes_intro.ipynb`

**Week 2 — Linear algebra for estimation**
- [ ] Matrix operations review: inverse, transpose, determinant
- [ ] Eigenvalues and eigenvectors — geometric interpretation
- [ ] Positive definite matrices — why covariance matrices are PD
- [ ] 📓 Notebook: `00-foundations/linear-algebra/01_matrix_review.ipynb`

**Week 3 — Signal theory refresher**
- [ ] Discrete-time signals, sampling (Nyquist)
- [ ] Noise models: white noise, colored noise, drift
- [ ] Power Spectral Density, Allan Variance (IMU noise model)
- [ ] 📓 Notebook: `00-foundations/signals-and-noise/01_noise_models.ipynb`

**Week 4 — State-space models**
- [ ] State-space representation: x, A, B, C, D matrices
- [ ] Discretisation of continuous models
- [ ] Observability and controllability — intuition
- [ ] 📓 Notebook: `00-foundations/state-space-models/01_state_space_intro.ipynb`

---

### Month 2 — Kalman Filter (`01-kalman-filter`)

**Week 5 — KF derivation**
- [ ] Prediction step: state propagation + covariance propagation
- [ ] Update step: Kalman gain, innovation, covariance update
- [ ] Full derivation from first principles (MMSE estimator)
- [ ] 📓 Notebook: `01-kalman-filter/theory/01_kf_derivation.ipynb`

**Week 6 — 1D Kalman Filter implementation**
- [ ] Position tracking with noisy sensor
- [ ] Tuning Q (process noise) and R (measurement noise)
- [ ] Visualising convergence of the covariance
- [ ] 📓 Notebook: `01-kalman-filter/notebooks/01_kf_1d_tracking.ipynb`

**Week 7 — Multi-dimensional KF**
- [ ] Position + velocity tracking (constant velocity model)
- [ ] 2D tracking with radar-like measurements
- [ ] Consistency analysis: NIS, NEES
- [ ] 📓 Notebook: `01-kalman-filter/notebooks/02_kf_2d_tracking.ipynb`

**Week 8 — Exercises and edge cases**
- [ ] Filter divergence: causes and fixes
- [ ] Adaptive noise covariance
- [ ] 📝 Exercises with reference solutions
- [ ] ✅ Checkpoint: can implement KF from scratch without reference

---

### Month 3 — Sensor Fusion (`02-sensor-fusion`)

**Week 9 — IMU modelling**
- [ ] Accelerometer + gyroscope error models
- [ ] Bias, scale factor, random walk
- [ ] Allan Variance analysis on real IMU data
- [ ] 📓 Notebook: `02-sensor-fusion/imu-dead-reckoning/01_imu_model.ipynb`

**Week 10 — Dead reckoning**
- [ ] Inertial navigation equations
- [ ] Drift accumulation analysis
- [ ] Dead reckoning in 2D with simulated IMU
- [ ] 📓 Notebook: `02-sensor-fusion/imu-dead-reckoning/02_dead_reckoning_2d.ipynb`

**Week 11 — GPS + IMU fusion (loose coupling)**
- [ ] GPS measurement model, HDOP/VDOP
- [ ] Loosely coupled GPS/IMU KF architecture
- [ ] Simulation: noisy GPS + drifting IMU → fused trajectory
- [ ] 📓 Notebook: `02-sensor-fusion/gps-imu-fusion/01_loose_coupling.ipynb`

**Week 12 — Covariance analysis + multi-rate sensors**
- [ ] Cross-correlation between sensors
- [ ] Handling sensors with different update rates
- [ ] 🚀 **Phase 1 deliverable:** complete GPS+IMU simulator with KF fusion
- [ ] 📓 Notebook: `02-sensor-fusion/multi-rate-sensors/01_multirate_fusion.ipynb`

---

## Phase 2 — EKF, Particle Filter & Robotics Perception
**Goal:** Handle nonlinear systems and build a simulated autonomous navigation stack.

### Month 4 — Extended Kalman Filter (`03-extended-kalman-filter`)

**Week 13 — EKF derivation**
- [ ] Linearisation via first-order Taylor expansion
- [ ] Jacobian matrices: F (state transition), H (measurement)
- [ ] EKF prediction + update equations
- [ ] 📓 Notebook: `03-extended-kalman-filter/theory/01_ekf_derivation.ipynb`

**Week 14 — Jacobian computation**
- [ ] Analytical Jacobians for common motion models
- [ ] Numerical Jacobians via finite differences
- [ ] 📓 Notebook: `03-extended-kalman-filter/jacobians/01_jacobian_computation.ipynb`

**Week 15 — Nonlinear models**
- [ ] CTRV model (Constant Turn Rate & Velocity) — used in automotive/drone
- [ ] Bearings-only tracking
- [ ] 📓 Notebook: `03-extended-kalman-filter/nonlinear-models/01_ctrv_model.ipynb`

**Week 16 — 2D drone navigation with EKF**
- [ ] Full 2D simulation: GPS + IMU + magnetometer
- [ ] EKF-based state estimator
- [ ] Trajectory reconstruction and error analysis
- [ ] 🚀 **Module deliverable:** `drone-navigation-2d` complete simulation

---

### Month 5 — Particle Filter (`04-particle-filter`)

**Week 17 — PF theory**
- [ ] Monte Carlo principle, importance sampling
- [ ] Sequential Importance Resampling (SIR)
- [ ] Particle degeneracy and resampling strategies
- [ ] 📓 Notebook: `04-particle-filter/theory/01_particle_filter_intro.ipynb`

**Week 18 — Localization with particle filter**
- [ ] Robot localization in a known map
- [ ] Likelihood field sensor model
- [ ] 📓 Notebook: `04-particle-filter/localization/01_pf_localization.ipynb`

**Week 19–20 — EKF vs PF benchmark**
- [ ] Same scenario, both filters
- [ ] Non-Gaussian noise injection
- [ ] RMSE comparison, computational cost
- [ ] 📓 Notebook: `04-particle-filter/comparison-ekf-pf/01_benchmark.ipynb`

---

### Month 6 — Robotics & Perception (`05-robotics-perception`)

**Week 21–22 — ROS2 fundamentals**
- [ ] ROS2 architecture: nodes, topics, services, actions
- [ ] Writing a Python publisher/subscriber
- [ ] Sensor driver simulation (IMU, GPS topics)
- [ ] 📓 Tutorial: `05-robotics-perception/ros2-introduction/`

**Week 23 — Computer vision for perception**
- [ ] Camera calibration, intrinsic/extrinsic parameters
- [ ] Feature detection (ORB, SIFT), optical flow
- [ ] 📓 Notebook: `05-robotics-perception/opencv-perception/01_camera_calibration.ipynb`

**Week 24 — LiDAR basics + SLAM introduction**
- [ ] Point cloud processing with Open3D
- [ ] SLAM problem formulation: loop closure, graph optimisation
- [ ] 🚀 **Phase 2 deliverable:** ROS2 drone simulation with EKF localisation

---

## Phase 3 — Embedded AI
**Goal:** Deploy real-time AI inference on constrained hardware.

### Month 7 — TinyML & Quantization (`06-embedded-ai`)

**Week 25–26 — TinyML pipeline**
- [ ] Constraints of edge AI: memory, latency, power
- [ ] Model design for edge: MobileNet, SqueezeNet, custom CNNs
- [ ] Post-training quantisation (INT8, FP16)
- [ ] 📓 Notebook: `06-embedded-ai/tinyml/01_tinyml_intro.ipynb`

**Week 27 — TensorFlow Lite**
- [ ] TFLite converter, flatbuffer format
- [ ] Deploying on Raspberry Pi
- [ ] Latency profiling
- [ ] 📓 Notebook: `06-embedded-ai/tensorflow-lite/01_tflite_deployment.ipynb`

**Week 28 — ONNX Runtime**
- [ ] ONNX format and ecosystem
- [ ] PyTorch → ONNX → ARM deployment
- [ ] Cross-framework benchmarks
- [ ] 📓 Notebook: `06-embedded-ai/onnx-runtime/01_onnx_deployment.ipynb`

---

### Month 8–9 — Edge Inference Benchmarks

**Week 29–36**
- [ ] Systematic benchmark: Raspberry Pi 4, Jetson Nano, STM32
- [ ] Metrics: latency, throughput, memory footprint, energy
- [ ] Pruning and knowledge distillation
- [ ] 🚀 **Phase 3 deliverable:** edge anomaly detector (vibration / IMU data)

---

## Phase 4 — Aerospace Use Cases & Capstone
**Goal:** Integrate all skills into aerospace-context demonstrators.

### Month 9–10 (`07-aerospace-use-cases`)

- [ ] **Drone state estimation** — full 6-DOF EKF with quaternion attitude
- [ ] **Satellite attitude estimation** — MEKF with star tracker + gyro
- [ ] **GNSS denoising** — neural denoising of GPS signals
- [ ] **Digital Twin** — real-time telemetry dashboard (Kafka + InfluxDB + Grafana)

### Month 11–12 (`08-capstone-projects`)

| Project | Description | Skills integrated |
|---------|-------------|-------------------|
| `project-01` | GPS+IMU navigation system | KF, EKF, sensor fusion |
| `project-02` | Drone EKF with ROS2 | EKF, ROS2, perception |
| `project-03` | Edge AI anomaly detection | TinyML, embedded inference |
| `project-04` | Digital Twin aerospace | Fusion, dashboard, telemetry |

Each capstone includes:
- Full technical report (written as a teaching case study)
- Presentation slides
- Reproducible demo

---

## Phase 5 — Research Track (optional, months 12–18)

For those targeting a doctoral position or a research-facing role:

- [ ] Identify a research question at the intersection of your capstone work
- [ ] Survey literature (Google Scholar, ArXiv cs.RO, eess.SP)
- [ ] Submit to a conference: **FUSION**, **ICASSP**, **EUSIPCO**, **TAROS**, **ROSCon**
- [ ] Contact a research lab for collaboration (ERENA, ISAE-SUPAERO, ONERA)
- [ ] Structure a CIFRE thesis proposal with an industrial partner

---

## Checkpoints summary

| Milestone | Target date | Deliverable |
|-----------|-------------|-------------|
| M1 ✅ | End month 3 | GPS+IMU KF simulator (Python) |
| M2 ✅ | End month 6 | ROS2 drone + EKF localisation |
| M3 ✅ | End month 9 | Edge AI inference on ARM |
| M4 ✅ | End month 12 | 4 capstone projects + reports |
| M5 ✅ | End month 18 | Conference submission or CIFRE proposal |
