RK4 Rotor Dataset Guide

1. Data source and test overview

This dataset contains vibration measurements from the RK4 rotor test rig
provided with the textbook. It covers seven conditions: healthy, two levels
of unbalance, two levels of shaft misalignment, and bearing inner-race and
outer-race faults. These are data for condition-specific anomaly detection
and fault diagnosis, rather than run-to-failure lifetime data.

The nominal shaft speed is 1,500 rpm (25 Hz), and the sampling rate is 12,800 Hz.
For each condition, twenty one-second measurement signals were concatenated and stored.
The 3.5 g and 4.5 g values for unbalance denote added masses and should be
distinguished from g as a unit of vibration acceleration.

Related publication:
Lee, H. J., Jang, J. W., Park, H. J., Noh, K. R., Choi, J. H., and Yoon, H. S. (2026).
“Practical approach for integrated fault diagnosis in rotating machinery using
vibration signals,” Proceedings of the Institution of Mechanical Engineers,
Part O: Journal of Risk and Reliability, 240(4), 1399–1414.

2. Filenames and data structure

The raw signal file provided with the textbook is RK4_raw.mat in MATLAB MAT format.
Its variables and measurement conditions are:

  Nor: healthy
  Unb35: unbalance, added mass of 3.5 g
  Unb45: unbalance, added mass of 4.5 g
  Mis06: shaft misalignment of 0.6 mm
  Mis08: shaft misalignment of 0.8 mm
  Bin: bearing inner-race fault
  Bout: bearing outer-race fault

Each variable is an array with 256,000 rows and one column.

Rows 1–12,800 contain the first measurement signal, and rows 12,801–25,600
contain the second. A total of twenty measurement signals are stored in this way.

3. Use in the textbook

Chapter 4: Raw signals from all seven conditions are used to examine shaft
           rotation components and bearing fault-related components and to
           extract frequency-domain features. Mahalanobis distance-based
           anomaly detection uses healthy-condition features as a reference.
           Velocity RMS is also calculated from vibration acceleration to
           compare vibration levels across conditions.

Chapter 5: Time-domain features calculated from the raw signals and
           frequency-domain features saved in Chapter 4 are used for feature
           selection and ANN classification of the seven conditions.
           Empirical fault diagnosis using frequency-domain features is also performed.

Checking theoretical fault frequencies against measured spectra

The initially used bearing fault frequencies did not match the main peaks
in the measured spectra. The textbook code therefore includes a procedure
to compare theoretical and measured values and identify frequency candidates
for feature extraction.

The bearing geometry entered in the current Chapter 4 code consists of a
pitch diameter of 25 mm, a ball diameter of 5 mm, eleven balls, and a contact
angle of 40 degrees. At a shaft frequency of 25 Hz, the calculated theoretical
frequencies are approximately 116.4 Hz for BPFO and 158.6 Hz for BPFI.

At a shaft frequency of 25 Hz, the theoretical values are BPFO 116.4 Hz and
BPFI 158.6 Hz. However, analysis of the measured spectra identified
BPFO 95 Hz and BPFI 150 Hz as frequency candidates.
The BPFI candidate at 150 Hz coincides with the sixth shaft harmonic,
so it cannot be conclusively identified as a bearing fault frequency.

4. Files in the folder

  RK4_raw.mat: raw signal arrays for the seven conditions
  RK4_feat_domain.mat: frequency-domain features extracted and saved in Chapter 4

The variables in RK4_feat_domain.mat are FNor, FUnb35, FUnb45, FMis06,
FMis08, FBin, and FBout. Each has twenty rows and nine columns.

  Rows: twenty measurement signals for each condition
  Columns: the following nine features extracted from each measurement signal

  Columns 1–3: peak amplitudes at shaft 1X, 2X, and 3X
  Columns 4–6: peak amplitudes at BPFI−1X, BPFI, and BPFI+1X
  Columns 7–9: peak amplitudes at BPFO, 2BPFO, and 3BPFO

BPFI and BPFO use the values identified from the measured spectra above.
The inner-race features are the center component and its two sidebands,
rather than the first three BPFI harmonics.
