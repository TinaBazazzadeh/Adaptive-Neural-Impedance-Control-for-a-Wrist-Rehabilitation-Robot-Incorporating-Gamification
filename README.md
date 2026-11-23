# Adaptive Neural-Impedance Control for a Wrist Rehabilitation Robot Incorporating Gamification

**B.Sc. Project, Control Engineering**
Department of Electrical Engineering, Amirkabir University of Technology (Tehran Polytechnic)

**Author:** Tina Bazazzadeh
**Supervisors:** Dr. Sharifi and Prof. Talebi
**Date:** November 2026

---

## Overview

This thesis presents an adaptive neural-impedance controller for a three-degree-of-freedom wrist rehabilitation robot, combined with a Unity airplane game. Without a force sensor, the robot estimates the patient's interaction torque, wrist stiffness and damping, fatigue, voluntary effort and spasms during exercise. It then adjusts its own stiffness, damping and inertia to give only as much help as the patient needs. The system was evaluated in MATLAB simulation and tested on the real robot.

## Main Components

- **Sensorless torque estimation:** a generalized momentum observer, with the robot model calibrated beforehand using an empty handle (least-squares parameter identification plus RBF compensation of the residual torque).
- **Wrist stiffness and damping:** lock-in detection of the response to small probe signals at a different frequency for each joint, giving the full matrix including the coupling between joints.
- **Fatigue and effort:** a ramp-tracking oscillator observer of the voluntary torque.
- **Spasm handling:** a detection layer with an adaptive notch filter and fast regression of the stiffness increase. During a spasm, a safety layer softens the robot, adds damping and lowers the torque limit.
- **Online learning:** an RBF neural network that adds centres when it sees new inputs and learns each patient during the session without true labels. Its output is blended with the physics-based estimates according to its confidence.
- **Assist-as-needed control:** tracking error, effort, fatigue and the game score are mapped to bounded impedance gains, and stiffness may only rise at a limited rate to keep the variable impedance stable.
- **Gamification:** a Unity airplane game linked to the controller over UDP every 10 ms, with the game score feeding back into the assistance level.

## Key Results

- After warm-up, a normal patient's wrist stiffness was estimated within about 2–4%, and the coupling term within about 3%.
- All spasms were detected with no false alarms, and the robot's torque never hit its limit.
- The robot increased its help as fatigue grew, and the fatigued patient's game score (0.744) stayed close to the normal patient's (0.777).
- The variable-impedance stability conditions were violated in at most 0.5% of samples, and only during spasms.
- With the calibrated model, an eigenvalue check of the full control loop found all 3,888 operating points stable. If the model overestimates the robot's inertia by 20%, about two thirds of those points become unstable.
- Pre-training the network made it more confident and helped with patients similar to its training data, but not with unfamiliar ones. A test on 30 random patients showed this, along with the method's limits for very stiff wrists, very severe spasms and fatigue that starts early in the session.
- On the real robot, the controller was stable and gave way softly when torque was applied to the handle. Some jitter came from delays in the hardware interface.

## Files

| File | Description |
|------|-------------|
| `Thesis_Bazazzadeh_English.pdf` | Full thesis in English (83 pages) |
| `Thesis_Bazazzadeh_English_LaTeX.zip` | LaTeX source of the English thesis (main file, chapters and figures) |

To rebuild the PDF from the source, unzip it and run `pdflatex main.tex` three times.

## Keywords

Wrist rehabilitation robot, adaptive impedance control, online radial basis function network, sensorless torque estimation, gamification
