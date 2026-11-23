TGLS Dataset User Guide

1. Source and experimental context

TGLS stands for turbine generator lubrication system. Multichannel measurements
from the tank, pump, filter, cooler and bearing path support component health
diagnosis and analysis of bearing-side response.

Related publication:
Lee, D., Lee, I., Kim, Y., Joo, S. C., and Choi, J.-H. (2024).
Sensor set optimization by functional model and Bayesian network for fault
 diagnosis of turbine generator lubrication system.
Engineering Applications of Artificial Intelligence, 138, 109416.
https://doi.org/10.1016/j.engappai.2024.109416

This guide documents the practice dataset and its current label convention.
Consult the provider for usage and redistribution conditions; this README does
not grant a new license.

2. Files and array structure

  data.mat: variable data, shape 17400 × 15 × 81, single (float32)
    Axes: time samples, sensor channels, test conditions.
  code.mat: variable code, shape 81 × 4, uint8
    Rows: test conditions. Columns: component severity labels.
  4가지 고장 모드.png: diagram of the four fault modes.
  14개 센서 위치.png: lubrication path and sensor locations.

The location diagram marks 14 channels; data includes a fifteenth channel, Vib. All condition and sensor numbers below are one based, as in MATLAB.
Subtract one when indexing directly in Python. code(k,:) describes data(:,:,k).
No NaN or infinite values were found in the current data array.

The publication describes approximately 30 minutes of acquisition at 10 Hz.
At 10 Hz, the stored 17400 samples span 1740 seconds, or 29 minutes.
The Vib channel has the same stored length, but this alone does not establish
whether it is a raw high-frequency vibration waveform or a processed measure.
Original measurement bandwidth, units and calibration require acquisition
specifications; do not assign them from array dimensions alone.

3. Sensor order

   1 P1 : pump inlet pressure
   2 P2 : pump outlet pressure
   3 P3 : filter outlet / cooler inlet pressure
   4 P4 : cooler outlet pressure
   5 Pb : bearing supply pressure
   6 T1 : pump inlet temperature
   7 T2 : pump outlet temperature
   8 T3 : cooler inlet temperature
   9 T4 : cooler outlet temperature
  10 T5 : return-path temperature
  11 F1 : pump inlet flow
  12 Tb : bearing metal temperature
  13 F3 : filter outlet / cooler inlet flow
  14 F5 : return-path flow
  15 Vib: bearing vibration channel

Component group (12 channels): P1,P2,P3,P4,T1,T2,T3,T4,T5,F1,F3,F5
  MATLAB column indices: [1 2 3 4 6 7 8 9 10 11 13 14]
Bearing response group (3 channels): Pb,Tb,Vib
  MATLAB column indices: [5 12 15]

4. Fault modes and labels

Columns of code:
  1: pump fault (gear tooth fault in the supplied diagram)
  2: filter blockage (emulated by valve restriction)
  3: cooler blockage (emulated by valve restriction)
  4: cooler cooling fault (cooling fan airflow obstruction)

Current practice convention for every column:
  0 = normal
  1 = moderate fault
  2 = severe fault

Three levels for four targets yield 3^4=81 conditions. Each condition is one
stored run, not a count of independent repeated runs.
  Condition 28: [0 0 0 0], all normal.
  Condition 27: [2 2 2 2], all severe.
  Condition  1: [0 0 0 1], moderate cooling fault; other components normal.

The current code.mat already converts the former reversed convention using
2-code. Do not apply that transformation again.
For binary diagnosis, code==0 maps to normal 0 and code!=0 maps to fault 1.
Component fault labels are not labels of physical bearing damage.

5. Preprocessing and features

Divide each run into 58 nonoverlapping windows of 300 samples:
  1-300, 301-600, ..., 17101-17400.
No samples are discarded. At 10 Hz, one window spans 30 seconds. The length is
a practice setting, not a physically optimized diagnostic duration.

Mahalanobis distance uses channel means and standard deviations: 24 features
for components and 6 for bearing response. Fit scaling and Ledoit-Wolf covariance
on all 58 windows of normal condition 28. Use the 95th percentile of reference
distances as threshold.
The autoencoder reconstructs standardized raw windows of shape 300×12 or 300×3.
It trains on the complete normal condition and uses the 95th percentile of
normal window RMSE as threshold. An independent false-alarm rate cannot be
estimated because only one all-normal record is available.

Shared FEATURE_MODE options for binary and severity component diagnosis:
  24: mean and standard deviation of 12 component channels.
  32: 24 plus mean and standard deviation of four difference signals.
  44: 32 plus mean and standard deviation of six physical proxies.
Differences:
  P2-P1, P2-P3, P3-P4, T3-T4.
Physical proxies:
  F1*(P2-P1)
  (P2-P3)/Q, (P2-P3)/Q^2
  (P3-P4)/Q, (P3-P4)/Q^2
  F3*(T3-T4)
For denominators, Q=max(F3,epsilon), with epsilon equal to 1% of the median
positive F3 in training conditions. Compute products and ratios sample by sample
before extracting window statistics. These are physically motivated proxies,
not dimensionless groups or fully calibrated physical quantities.

6. Training and interpretation

Partition complete conditions before flattening windows: 44 training,
12 validation and 25 test conditions, giving 2552, 696 and 1450 windows.
A condition never appears in multiple subsets. Fit scaling and the flow floor
only on training conditions.
Binary condition decisions average sigmoid probabilities across windows and
apply 0.5. Severity decisions average each class probability across windows and
select the class with the largest mean.
Windows from one run are not independent repeated experiments.

Bearing anomaly scores measure departure from a normal response; they do not
confirm bearing damage. Supervised models train on diverse fault combinations,
so their results do not guarantee equal performance at sites with few or no
labeled fault cases.

Practice notebooks: https://tinyurl.com/phm-practice
Prepared: 2026-10-18
