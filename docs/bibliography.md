# Bibliography

> Annotated references for the full roadmap. Organised by domain. Access links where available.

---

## Estimation & Kalman Filtering

### Essential

**Labbe, R. (2020). *Kalman and Bayesian Filters in Python*.**  
Open access — https://github.com/rlabbe/Kalman-and-Bayesian-Filters-in-Python  
→ *The best starting point. Jupyter notebooks, intuitive explanations, covers KF → EKF → UKF → PF. Read alongside the implementation modules.*

**Simon, D. (2006). *Optimal State Estimation: Kalman, H∞, and Nonlinear Approaches*. Wiley.**  
→ *Rigorous mathematical treatment. Essential for understanding derivations. Chapters 5–7 for EKF.*

**Welch, G. & Bishop, G. (2006). *An Introduction to the Kalman Filter*. UNC Chapel Hill.**  
Free PDF — https://www.cs.unc.edu/~welch/media/pdf/kalman_intro.pdf  
→ *Classic 16-page introduction. Good for course materials.*

---

## Sensor Fusion & Navigation

**Groves, P. D. (2013). *Principles of GNSS, Inertial, and Multisensor Integrated Navigation Systems* (2nd ed.). Artech House.**  
→ *The reference book for GPS+IMU fusion. Dense but comprehensive. Chapter 9 (loosely coupled) is the key chapter for Module 02.*

**Titterton, D. & Weston, J. (2004). *Strapdown Inertial Navigation Technology* (2nd ed.). IET.**  
→ *Deep dive into IMU error models, calibration, navigation equations.*

**Woodman, O. J. (2007). *An Introduction to Inertial Navigation*. University of Cambridge Technical Report.**  
Free PDF — https://www.cl.cam.ac.uk/techreports/UCAM-CL-TR-696.pdf  
→ *Excellent 37-page primer on IMU basics and dead reckoning.*

---

## Robotics & Probabilistic Methods

**Thrun, S., Burgard, W., & Fox, D. (2005). *Probabilistic Robotics*. MIT Press.**  
→ *The bible of probabilistic robotics. Covers Kalman, Particle Filters, SLAM, localization in a unified framework. Modules 01–05 draw heavily from this.*

**Barfoot, T. D. (2017). *State Estimation for Robotics*. Cambridge University Press.**  
Free PDF — http://asrl.utias.utoronto.ca/~tdb/bib/barfoot_ser17.pdf  
→ *Modern treatment including Lie groups for attitude estimation. Module 07 reference.*

**Sola, J., Deray, J., & Atchuthan, D. (2018). *A micro Lie theory for state estimation in robotics*. ArXiv:1812.01537.**  
→ *Concise reference for quaternion-based estimation (satellite attitude, drone 6-DOF).*

---

## Embedded AI & TinyML

**Warden, P. & Situnayake, D. (2019). *TinyML*. O'Reilly.**  
→ *Practical guide to ML on microcontrollers. Essential companion to Module 06.*

**Banbury, C. et al. (2021). *MLPerf Tiny Benchmark*. NeurIPS Datasets and Benchmarks Track.**  
→ *Standard benchmark for edge AI. Use to contextualize your own benchmarks in Module 06.*

**Jacob, B. et al. (2018). *Quantization and Training of Neural Networks for Efficient Integer-Arithmetic-Only Inference*. CVPR.**  
→ *Foundational paper on INT8 quantization. Required reading for Module 06.*

---

## SLAM

**Cadena, C. et al. (2016). *Past, Present, and Future of Simultaneous Localization and Mapping: Toward the Robust-Perception Age*. IEEE TRO.**  
→ *Comprehensive SLAM survey. Good entry point before tackling Module 05 SLAM.*

**Grisetti, G., Kümmerle, R., Stachniss, C., & Burgard, W. (2010). *A Tutorial on Graph-Based SLAM*. IEEE Intelligent Transportation Systems Magazine.**  
→ *Clear explanation of graph SLAM — the dominant modern approach.*

---

## Aerospace Systems

**Britting, K. R. (1971). *Inertial Navigation Systems Analysis*. Wiley.**  
→ *Classical reference on inertial navigation. Historical but foundational.*

**RTCA DO-178C (2011). *Software Considerations in Airborne Systems and Equipment Certification*.**  
→ *The certification standard for airborne software. Read the overview and DAL table for Module 07.*

**SAE ARP4754A (2010). *Guidelines for Development of Civil Aircraft and Systems*.**  
→ *System-level complement to DO-178C. Focus on the development assurance level concept.*

---

## Online Courses (structured learning)

| Course | Platform | Relevant modules |
|--------|----------|-----------------|
| State Estimation and Localization for Self-Driving Cars | Coursera (Univ. Toronto) | 01, 02, 03 |
| Sensor Fusion Engineer Nanodegree | Udacity | 02, 03, 04 |
| Tiny Machine Learning (TinyML) | edX (Harvard/Google) | 06 |
| Self-Driving Cars Specialization | Coursera (Univ. Toronto) | 03, 05, 07 |
| ROS2 for Beginners | Udemy (various) | 05 |

---

## Key ArXiv papers to follow

- **cs.RO** — Robotics: https://arxiv.org/list/cs.RO/recent
- **eess.SP** — Signal Processing: https://arxiv.org/list/eess.SP/recent
- **cs.LG** — Machine Learning (edge AI papers): https://arxiv.org/list/cs.LG/recent

---

## Target conferences for publication

| Conference | Focus | Deadline (approx.) |
|------------|-------|-------------------|
| FUSION (IEEE) | Sensor fusion, estimation | March |
| ICASSP (IEEE) | Signal processing | October |
| EUSIPCO | European signal processing | March |
| IROS | Intelligent robots & systems | March |
| ROSCon | ROS ecosystem | June |
| TAROS | Autonomous robot systems | April |
