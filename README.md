# Lab 5: Motor Parameters Identification (TRIK RK370 DC Motor)

This directory contains the code, raw experimental measurements, and final identification analysis for the TRIK RK370 DC motor parameters as part of **CS413 - Robotics**.

## Project Structure
```text
CS413 - Robotics/
└── Lab5/
    ├── motor_identification.qrs  # TRIK Studio Python script used to gather step responses
    ├── report.ipynb             # Jupyter Notebook detailing parameter fitting and simulation comparisons
    ├── results_table.csv        # Summary table of fitted steady-state speed (w_ss) and time constant (T_m) per PWM level
    ├── wss_vs_U.png             # Plot illustrating steady-state speed against actual voltage (including dead zones)
    ├── step_response.png        # Comparison plot of measured vs. simulated motor step responses
    └── motor_data/              # Folder containing 82 raw space-separated experimental log files
Identified Motor Parameters
Based on our offline fitting of the experimental step responses using the linear model $T_m \cdot d\omega/dt + \omega = U / k_e$, we extracted the following key parameters for both drive wheels:

Wheel 1 (Left):

Back-EMF constant ($k_e$): 0.962 V·s/rad
Dead Zone Voltage ($U_{dead}$): ±2.92 V (~`±26.1%` PWM)
Mean electromechanical time constant ($T_m$): 0.217 s
Wheel 2 (Right):

Back-EMF constant ($k_e$): 0.941 V·s/rad
Dead Zone Voltage ($U_{dead}$): ±3.00 V (~`±26.8%` PWM)
Mean electromechanical time constant ($T_m$): 0.272 s
Summary of Model Limitations & Odometry Impact
Linear Model Failures: The linear model fails to capture the prominent dead zone below approximately $\pm 3.0$ V where static friction prevents any movement. It also does not account for minor non-linearities at peak voltages caused by thermal resistance variations and battery voltage sag.
Asymmetry: Wheel 2 has a lower back-EMF constant (leading to a higher top speed) but has a slower transient response ($T_m = 0.272$ s vs. $0.217$ s) than Wheel 1.
Impact on Odometry: Because the motors behave asymmetrically, sending symmetric PWM commands to both wheels will make the robot curve and drift. The left wheel accelerates faster initially, but the right wheel dominates in steady-state cruise. This behavior will require corrective scaling coefficients in Week 6's localization models and active closed-loop tracking (e.g., PI control) in Week 7.