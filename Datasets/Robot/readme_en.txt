Wafer Transfer Robot Dataset Guide

1. Data source and test overview

This dataset contains robot control signals and end-effector vibration
measurements collected while varying the wear conditions of the timing
belt (TB) and harmonic drive (HD) of a wafer transfer robot.
The axis 3 timing belt has a healthy condition and three fault levels,
while the harmonic drive has a healthy condition and two fault levels.
The axis 4 timing belt is healthy in all tests.

Related publication:
Lee, I., Park, H. J., Jang, J.-W., Kim, C.-W., and Choi, J.-H.,
“System-Level Fault Diagnosis for an Industrial Wafer Transfer Robot with
Multi-Component Failure Modes,” Applied Sciences, 13(18), 10243, 2023.
https://doi.org/10.3390/app131810243

Of the 12 possible condition combinations, 11 are provided, excluding
TB Lv3 with HD Lv2. Five recordings of approximately 60 seconds were
collected for each condition, giving 55 raw signal files.
These are fault-level diagnosis data, rather than continuous run-to-failure data.

2. Raw signal filenames and data structure

The original data are in MAT format, with filenames following TTUU_HH_RR.mat.

  TT: axis 3 timing belt condition; 00 healthy, 01/02/03 fault levels 1/2/3
  UU: axis 4 timing belt condition; always 00 (healthy)
  HH: harmonic drive condition; 00 healthy, 01/02 fault levels 1/2
  RR: repeated measurement number, 01–05

Example: 0200_02_04.mat represents axis 3 belt Lv2, a healthy axis 4 belt,
         harmonic drive Lv2, and the fourth recording.

Each raw signal file contains the following variables:

  robot: approximately 60,000 rows × 12 columns; robot control signals recorded for 60 seconds at 1,000 Hz
  vibx, viby, vibz: 1,536,000 rows × 1 column each; vibration signals recorded for 60 seconds at 25,600 Hz
  fs: vibration sampling rate, 25,600 Hz

Rows of robot represent samples in chronological order. Its columns are:

  Columns 1–5: axis 3 current signal, voltage, duty cycle, position tracking error, and actual joint velocity
  Columns 6–10: the same five signals for axis 4
  Column 11: axis 2 duty cycle
  Column 12: GPL program variable used to identify operation segments

The vibration directions are the end-effector travel direction for vibx,
the direction perpendicular to the ground for vibz, and the remaining
orthogonal direction for viby. The current signal in column 1 of robot
is the controller's current-loop torque command and should not be
interpreted as current in amperes without conversion.

3. Use in the textbook

Chapter 6 uses the axis 3 current and velocity signals and the vertical
vibration signal vibz from all 55 files. WPD features extracted from
current and velocity are used to diagnose TB and HD conditions.
Vibration RMS serves as a system performance indicator for modeling
its relationship with component conditions.

  ANN: estimates TB and HD health indices from selected features.
  GPR: models the relationship between TB/HD conditions and RMS calculated directly from each file's raw vibration signal.
  BN: estimates component condition probabilities from features and evaluates system performance and maintenance effects.

4. Files in the folder

  TTUU_HH_RR.mat files: 55 raw signal files
  Features_robot.mat: operation-wise WPD features and condition labels generated in Chapter 6

Features_robot.mat contains the following variables:

  feat_robot: 1,100 rows × 224 columns; rows represent operations and columns represent features
  feat_name: 1 row × 224 columns; feature names
  fs_r: robot control signal sampling rate, 1,000 Hz
  labseg: 1,100 rows × 4 columns; operation-wise condition labels

Twenty operations are used from each file, giving 55 × 20 = 1,100 operations.
The 224 features comprise 14 features from each of eight WPD bands for
both current and velocity: 2 × 8 × 14.
The columns of labseg contain the axis 3 belt condition, axis 4 belt condition,
harmonic drive condition, and repeated measurement number, in that order.
