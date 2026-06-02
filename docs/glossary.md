# Glossary — Embedded AI & Aerospace Navigation

> Bilingual reference (English / French). Designed for use in teaching materials.

---

## A

**Allan Variance** *(Variance d'Allan)*  
A time-domain analysis technique used to characterise noise in inertial sensors (gyroscopes, accelerometers). Reveals noise types as a function of averaging time: angle random walk, bias instability, rate random walk.  
→ Module `02-sensor-fusion/imu-dead-reckoning`

**Attitude** *(Attitude / Orientation)*  
The angular orientation of a rigid body in 3D space, typically expressed as Euler angles, a rotation matrix, or a quaternion.

---

## B

**Bayesian Inference** *(Inférence bayésienne)*  
A statistical framework for updating beliefs (probability distributions) in the light of new evidence. Fundamental to all estimation filters: `P(state|measurement) ∝ P(measurement|state) × P(state)`.

**Bias (IMU)** *(Biais)*  
A systematic, slowly-varying offset in IMU measurements. Modelled as a random walk process. Must be estimated and compensated in navigation systems.

---

## C

**CTRV Model** *(Modèle CTRV — Constant Turn Rate & Velocity)*  
A nonlinear motion model commonly used for vehicles and UAVs. State vector includes position, velocity, heading, and turn rate. Requires EKF or UKF.

**Covariance Matrix** *(Matrice de covariance)*  
A symmetric positive semi-definite matrix P encoding the uncertainty of a state estimate. Off-diagonal terms capture correlations between state components.

---

## D

**DAL (Design Assurance Level)** *(Niveau d'assurance de développement)*  
A safety criticality classification (A to E) defined in ARP4754A and DO-178C, where Level A is the most critical (catastrophic failure condition). Drives the rigor of software/hardware development processes.

**Dead Reckoning** *(Navigation à l'estime)*  
Estimating current position by integrating velocity (or acceleration) measurements over time from a known starting point. Accumulates drift without correction from absolute sensors (GPS).

**Digital Twin** *(Jumeau numérique)*  
A real-time virtual replica of a physical system, continuously updated with sensor data. Used for monitoring, prediction, and testing without physical risk.

**DO-178C**  
*Software Considerations in Airborne Systems and Equipment Certification.* The FAA/EASA regulatory standard governing the development of airborne software. Defines five design assurance levels (A–E) and associated verification objectives.

---

## E

**EKF — Extended Kalman Filter** *(Filtre de Kalman étendu)*  
A nonlinear extension of the Kalman Filter. Linearises the system model at each time step using first-order Taylor expansion (Jacobian matrices). Suitable for mildly nonlinear systems.

**Edge AI** *(IA embarquée / IA en périphérie)*  
Execution of AI inference directly on a constrained device (microcontroller, embedded SoC) rather than in the cloud. Key constraints: memory (KB–MB), latency (ms), power (mW).

---

## F

**FilterPy**  
A Python library implementing Kalman filters and related algorithms. Used in Modules 01–04.

**Fusion (sensor)** *(Fusion de capteurs)*  
The process of combining data from multiple sensors with different characteristics (accuracy, rate, modality) to produce a single, more accurate estimate. Architectures: loose coupling, tight coupling, deep coupling.

---

## G

**GNSS — Global Navigation Satellite System** *(Système de navigation par satellite)*  
General term for satellite-based positioning systems: GPS (US), GLONASS (Russia), Galileo (EU), BeiDou (China). Provides absolute position with ~1–10 m accuracy (standalone).

**Gazebo**  
A physics-based 3D robot simulation environment. Tightly integrated with ROS2 for sensor simulation (IMU, camera, LiDAR, GPS).

---

## H

**HDOP — Horizontal Dilution of Precision**  
A measure of GPS accuracy in the horizontal plane based on satellite geometry. Low HDOP = good geometry = better accuracy.

---

## I

**IMU — Inertial Measurement Unit** *(Centrale inertielle)*  
A sensor combining accelerometers (specific force) and gyroscopes (angular rate). Typically 6-DOF (3-axis accel + 3-axis gyro) or 9-DOF (+ magnetometer).

**Innovation** *(Innovation / résidu)*  
In Kalman filtering: the difference between the actual measurement and the predicted measurement: `ν = z - H·x̂⁻`. Drives the update step.

---

## J

**Jacobian Matrix** *(Matrice jacobienne)*  
The matrix of first-order partial derivatives of a vector-valued function. Used in the EKF to linearise nonlinear state transition and measurement functions.

---

## K

**Kalman Gain** *(Gain de Kalman)*  
The weight `K` that determines how much to trust the new measurement vs the prior prediction in the update step: `x̂ = x̂⁻ + K·ν`.

---

## L

**LiDAR — Light Detection and Ranging**  
An active sensor that measures distance using laser pulses. Produces 3D point clouds. Used for obstacle detection, terrain mapping, and SLAM.

**Loose Coupling** *(Couplage lâche)*  
A GPS+IMU fusion architecture where the GPS receiver outputs position and velocity estimates (already processed), which are then fused with IMU data in the navigation filter. Simpler than tight coupling.

---

## M

**MEKF — Multiplicative Extended Kalman Filter**  
An EKF variant that correctly handles attitude (rotation) estimation using quaternion algebra, avoiding singularities present in Euler angle representations.

**MobileNet**  
A family of efficient CNN architectures designed for mobile and embedded deployment. Uses depthwise separable convolutions to reduce computation.

---

## N

**NEES — Normalised Estimation Error Squared**  
A filter consistency metric. Measures whether the filter's covariance is consistent with the actual errors. Expected value = state dimension for a consistent filter.

**NIS — Normalised Innovation Squared**  
A filter consistency metric in the measurement space. Checks whether innovations are consistent with the innovation covariance.

---

## O

**ONNX — Open Neural Network Exchange**  
An open format for representing machine learning models, enabling interoperability between frameworks (PyTorch, TensorFlow, etc.) and deployment runtimes.

**Observability** *(Observabilité)*  
A system property indicating whether the internal state can be reconstructed from output measurements alone. Fundamental to filter design.

---

## P

**Particle Filter** *(Filtre particulaire)*  
A sequential Monte Carlo method that represents the posterior distribution as a set of weighted particles. Handles arbitrary (non-Gaussian) noise distributions and nonlinear models.

**Point Cloud** *(Nuage de points)*  
A set of data points in 3D space, typically produced by LiDAR or stereo cameras. Processed with libraries such as Open3D or PCL.

**PX4**  
An open-source flight control stack for drones and other autonomous vehicles. Uses EKF2 (an EKF implementation) for state estimation.

---

## Q

**Quantisation** *(Quantification)*  
The process of reducing the numerical precision of model weights and activations (e.g. from FP32 to INT8) to decrease model size and inference latency on embedded hardware.

**Quaternion**  
A 4-element representation of 3D rotation: `q = [qw, qx, qy, qz]` with `||q|| = 1`. Avoids gimbal lock (present in Euler angles) and is computationally efficient for propagation.

---

## R

**ROS2 — Robot Operating System 2**  
A modular middleware framework for robotics. Provides a publish/subscribe communication model, sensor drivers, simulation integration, and a large ecosystem of packages.

**RTOS — Real-Time Operating System** *(Système d'exploitation temps réel)*  
An operating system designed to guarantee deterministic response times. Examples: FreeRTOS, Zephyr, VxWorks. Required for safety-critical embedded systems.

---

## S

**SLAM — Simultaneous Localisation and Mapping**  
The problem of building a map of an unknown environment while simultaneously estimating the agent's location within it. Approaches: EKF-SLAM, graph SLAM, visual SLAM.

**State Vector** *(Vecteur d'état)*  
The vector `x` containing all variables needed to describe the system at a given time (e.g. position, velocity, orientation, bias).

---

## T

**Tight Coupling** *(Couplage serré)*  
A GPS+IMU fusion architecture where raw GNSS measurements (pseudoranges, Doppler) are directly fused with IMU data in the navigation filter. More robust than loose coupling in degraded GNSS environments.

**TinyML**  
The field of deploying machine learning models on microcontrollers and other ultra-constrained devices (< 1 MB RAM). Key tools: TensorFlow Lite for Microcontrollers, Edge Impulse.

---

## U

**UKF — Unscented Kalman Filter** *(Filtre de Kalman sans parfum)*  
A nonlinear Kalman filter that propagates a set of carefully chosen sigma points through the nonlinear function, capturing higher-order moments without computing Jacobians.

---

## W

**White Noise** *(Bruit blanc)*  
A random signal with flat power spectral density — uncorrelated samples. In discrete time: `w[k] ~ N(0, Q)`. Process noise in the Kalman filter is modelled as white noise.
