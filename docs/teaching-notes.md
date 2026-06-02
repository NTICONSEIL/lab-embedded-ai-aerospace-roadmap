# Teaching Notes

> Guidance for using this repository as course material.  
> Each module is designed for dual use: self-study and structured teaching.

---

## Philosophy

Each module follows the same pedagogical structure:

```
1. Motivation       — Why does this matter? What problem does it solve?
2. Theory           — Mathematical derivation, rigorous but accessible
3. Implementation   — From-scratch Python, no black boxes
4. Validation       — How do we know it works? (visualisation, consistency tests)
5. Application      — Realistic scenario, aerospace context where possible
6. Exercises        — Guided (with blanks) + reference solutions
```

This structure maps to **Bloom's taxonomy** levels 1–5 (remember → evaluate), stopping just short of level 6 (create), which is addressed in capstone projects.

---

## Module-by-module teaching notes

---

### `00-foundations`

**Audience:** Students with basic probability and linear algebra (L3/M1 level).  
**Estimated duration:** 6–8 hours of contact time + 4 hours TP.

**Key pedagogical choices:**
- Introduce Bayesian inference *before* any filter. Students who understand `P(state|z) ∝ P(z|state)·P(state)` will find the Kalman update step obvious, not magical.
- Use coin-flip and Gaussian examples before moving to navigation.
- State-space models should be introduced with a physical example (spring-mass or RC circuit) before abstracting to matrix form.

**Common student mistakes:**
- Confusing the covariance matrix P with measurement noise R
- Not understanding why P must be symmetric positive definite
- Treating A·x = 0 solutions as physical states

**TP suggestion:** Bayesian height estimation — a student estimates their own height from noisy ruler measurements, computing posterior manually, then in Python.

---

### `01-kalman-filter`

**Audience:** Students who have completed `00-foundations` or equivalent.  
**Estimated duration:** 8–10 hours contact + 6 hours TP.

**Key pedagogical choices:**
- Derive the Kalman gain from the MMSE criterion *once*, rigorously. Students remember a derivation they've worked through more than a formula they've been given.
- Start with 1D (scalar) before matrices. The insight comes in 1D; matrices are just bookkeeping.
- Spend time on Q and R tuning. This is where students (and practitioners) struggle most.

**Analogy for Q vs R:**  
> "Q is how much you trust your model. R is how much you trust your sensor. If your model is a drunk driver (high Q), listen to the GPS more."

**Consistency tests (NIS/NEES):**  
Introduce these early. Students who never learn to *validate* filters produce overconfident or diverging systems in practice.

**TP suggestion:** Track a 1D ball bouncing on a table. Students tune Q and R manually, then observe filter behaviour.

---

### `02-sensor-fusion`

**Audience:** Students who have completed Module 01.  
**Estimated duration:** 8 hours contact + 6 hours TP.

**Key pedagogical choices:**
- Start with IMU error characterisation (Allan Variance). Students who know *why* IMUs drift will be more motivated to fix it.
- Dead reckoning first, fusion second. The horror of integrating IMU noise for 30 seconds is the best motivation for adding GPS.
- Loose coupling before tight coupling. Keep it achievable.

**TP suggestion:** Simulate a 2D car trip. GPS drops out for 10 seconds. Watch dead reckoning drift. Then fuse and watch recovery.

---

### `03-extended-kalman-filter`

**Audience:** Students who have completed Modules 01–02.  
**Estimated duration:** 10–12 hours contact + 8 hours TP.

**Key pedagogical choices:**
- Spend a full session on Jacobian computation. This is the most error-prone step.
- Numerical Jacobians are a useful debugging tool — teach them alongside analytical ones.
- The drone-navigation-2d project should be the module *goal*, introduced at the beginning to motivate every step.

**Common student mistakes:**
- Computing Jacobians at the predicted state (correct) vs the updated state (wrong)
- Forgetting to re-linearise at each time step
- Using Euler angles in the state vector (leads to singularities) — introduce quaternions early as a better alternative

**TP suggestion:** Track a car executing a U-turn (constant turn rate model). Compare KF (diverges) vs EKF (handles nonlinearity).

---

### `04-particle-filter`

**Audience:** Students who have completed Modules 01–03.  
**Estimated duration:** 6 hours contact + 4 hours TP.

**Key pedagogical choices:**
- Motivate with a case where EKF fails: multi-modal distribution (robot waking up in an unknown room).
- The resampling step is counter-intuitive — demonstrate particle degeneracy first without resampling, then show the fix.
- The EKF vs PF benchmark is a high-value TP: students see the trade-off directly.

---

### `05-robotics-perception`

**Audience:** Students who have completed Modules 01–04.  
**Estimated duration:** 12 hours contact + 8 hours TP.

**Key pedagogical choices:**
- ROS2 is a tool, not a goal. Frame it as: "how do engineers wire together sensors and algorithms in production?" 
- Keep early ROS2 sessions simple (publisher/subscriber). The goal is the architecture concept, not the API.
- SLAM should be treated as a *motivating problem*, not a fully implemented solution. Students should understand the problem formulation deeply before touching code.

**TP suggestion:** Publish simulated IMU data on a ROS2 topic, subscribe and run EKF node, visualise with RViz. Connects Modules 03 and 05 directly.

---

### `06-embedded-ai`

**Audience:** Students with ML background + Modules 01–05.  
**Estimated duration:** 10 hours contact + 8 hours TP.

**Key pedagogical choices:**
- Start with constraints: show a table of what fits on a Cortex-M4 (256 KB RAM, no FPU). Students need to feel the constraint before learning techniques to respect it.
- Walk through the full pipeline: train (Colab) → convert (TFLite) → benchmark (Raspberry Pi) → deploy (STM32).
- Quantisation should be taught with a before/after comparison: accuracy drop vs latency gain.

**TP suggestion:** Train a keyword-spotting model in 30 minutes on Colab. Deploy on Raspberry Pi. Measure latency. Quantise to INT8. Measure again.

---

### `07-aerospace-use-cases`

**Audience:** Students who have completed Modules 01–06.  
**Estimated duration:** 8 hours contact + seminar format.

**Key pedagogical choices:**
- This module is more *domain context* than new algorithms. The goal is to connect what students know to real aerospace engineering problems.
- Invite an industry speaker if possible (Airbus, Safran, Thales, Dassault) for the Digital Twin session.
- DO-178C should be taught as a *vocabulary* session, not a deep dive. Goal: students can participate in a conversation about software criticality.

---

### `08-capstone-projects`

**Format:** 4–6 week projects, teams of 2–3.  
**Assessment:** Technical report (40%) + demonstration (40%) + peer review (20%).

**Teaching notes for each project:**

**Project 01 — GPS+IMU navigation:**  
Guide students to implement loose coupling first, then challenge them to implement tight coupling as an extension. The comparison is the learning.

**Project 02 — Drone EKF with ROS2:**  
Run in simulation first (Gazebo). If hardware is available, a real quadrotor running PX4 is an exceptional motivation tool.

**Project 03 — Edge AI anomaly detection:**  
Choose an application students find meaningful: structural health monitoring, predictive maintenance on an engine, fall detection for an aircraft technician.

**Project 04 — Digital Twin:**  
This project explicitly connects to the emerging research agenda of the school. Encourage students to link to real industrial use cases from the aerospace sector.

---

## Assessment criteria (general)

| Criterion | Weight | What to look for |
|-----------|--------|-----------------|
| Theoretical correctness | 25% | Derivations, mathematical rigour |
| Implementation quality | 25% | Clean code, documented, reproducible |
| Validation methodology | 20% | Consistency tests, benchmarks, edge cases |
| Technical communication | 15% | Clear writing, figures, teaching-ready |
| Innovation / depth | 15% | Going beyond the baseline |

---

## Notes on teaching in English

This curriculum is designed to be taught in either French or English (as required for the SEN-Aero profile).

Guidelines:
- All notebooks are written in English (code, comments, markdown)
- Exercises can be given in French or English depending on audience
- The glossary provides FR/EN equivalents for all key terms
- Slide notes in `teaching-notes.md` are in French for the instructor's convenience

---

## Recommended class schedule (example — 30h module)

| Session | Duration | Content |
|---------|----------|---------|
| 1 | 2h | Bayesian inference + state-space models |
| 2 | 2h | Kalman Filter derivation |
| 3 | 3h TP | KF implementation (1D → 2D) |
| 4 | 2h | IMU modelling + dead reckoning |
| 5 | 3h TP | GPS+IMU fusion simulator |
| 6 | 2h | EKF derivation + Jacobians |
| 7 | 3h TP | EKF drone navigation |
| 8 | 2h | Particle Filter |
| 9 | 2h | Embedded AI constraints + quantisation |
| 10 | 3h TP | TFLite deployment + benchmark |
| 11 | 2h | Aerospace applications + DO-178C |
| 12 | 4h | Capstone project kickoff + coaching |
