ZeMA Hydraulic System Dataset Guide

1. Data source and test overview

This multichannel sensor dataset was collected from a hydraulic test rig at
ZeMA, Germany, while varying the conditions of the cooler, valve, pump,
and accumulator. A total of 2,205 load cycles, each lasting 60 seconds,
were recorded. Faults in multiple components may occur simultaneously.

Official dataset information and downloads:
https://archive.ics.uci.edu/dataset/447/condition+monitoring+of+hydraulic+systems

Dataset source: Helwig, N., Pignanelli, E., and Schütze, A. (2015).
Condition monitoring of hydraulic systems, UCI Machine Learning Repository.
https://doi.org/10.24432/C5CW21

Test rig and measurement methods: Helwig, N., Pignanelli, E., and Schütze, A.,
“Condition Monitoring of a Complex Hydraulic System Using Multivariate Statistics,”
IEEE I2MTC, 2015. https://doi.org/10.1109/I2MTC.2015.7151267

2. Original filenames and data structure

The original files are TXT files containing tab-delimited numerical arrays.
Each channel has its own file, such as PS1.txt or FS1.txt. Rows represent
the 2,205 measurement cycles, and columns represent samples collected
during the 60 seconds of one cycle.

  PS1–PS6: pressure (bar), 100 Hz, 2,205 rows × 6,000 columns each
  EPS1: motor power (W), 100 Hz, 2,205 rows × 6,000 columns
  FS1–FS2: flow rate (L/min), 10 Hz, 2,205 rows × 600 columns each
  TS1–TS4: temperature (°C), 1 Hz, 2,205 rows × 60 columns each
  VS1: vibration (mm/s), 1 Hz, 2,205 rows × 60 columns
  CE: cooling efficiency (%), CP: cooling power (kW), SE: efficiency factor (%)
      Each is sampled at 1 Hz with 2,205 rows × 60 columns; CE and CP are virtual sensors.

profile.txt contains cycle-wise condition labels in an array of 2,205 rows
and five columns. Matching rows in the sensor files and profile.txt refer
to the same cycle. The columns are:

  Column 1, cooler (%): 100 full efficiency, 20 reduced efficiency, 3 close to total failure
  Column 2, valve (%): 100 optimal switching, 90 small switching lag, 80 severe switching lag, 73 close to total failure
  Column 3, internal pump leakage: 0 none, 1 weak leakage, 2 severe leakage
  Column 4, accumulator (bar): 130 optimal pressure, 115 slightly reduced, 100 severely reduced, 90 close to total failure
  Column 5, stability flag: 0 stable, 1 steady-state conditions may not yet have been reached

The valve percentage describes its switching condition, rather than its opening percentage.

3. Use in the textbook

The complete original dataset was saved in MAT format while preserving
the structure of the TXT files. The exercises use 14 sensors and profile;
CE, CP, and SE are not used as inputs.

Chapter 4: An autoencoder detects cooler anomalies. The 741 cycles with
           100% efficiency are used for training, validation, and testing,
           while the 732 cycles with 20% efficiency are used for anomaly evaluation.
           The 3% condition is excluded from evaluation. Here, healthy refers
           only to the cooler condition.

Chapter 5: The ANN uses 56 features—mean, standard deviation, skewness,
           and kurtosis from each of the 14 sensors—to diagnose the healthy/faulty
           states of all four components simultaneously. The healthy values
           for the cooler, valve, pump, and accumulator are 100, 100, 0,
           and 130, respectively; all remaining levels are grouped as faulty.
           The CNN uses sensor signals to classify the condition levels of
           a selected component. Selecting cooler, valve, pump, or accumulator
           through target_name produces diagnosis results for that component's
           condition levels.

The current Chapter 4 and 5 exercises do not exclude data based on
the stability flag in column 5 of profile.

4. Files in the folder

  PS1.mat–PS6.mat, EPS1.mat: pressure and motor power
  FS1.mat–FS2.mat: flow rate
  TS1.mat–TS4.mat: temperature
  VS1.mat: vibration
  CE.mat, CP.mat, SE.mat: cooling efficiency, cooling power, and efficiency factor
  profile.mat: cycle-wise condition labels
  documentation.txt: Original provider documentation with sample counts for each component condition level and results from previous studies.

