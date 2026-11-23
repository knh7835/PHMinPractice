CWRU Bearing Dataset Guide

1. Data source and test overview

This dataset contains vibration measurements from healthy and faulty bearings,
provided by the Bearing Data Center at Case Western Reserve University (CWRU), USA.
Inner-race, outer-race, and ball defects were introduced into drive-end and fan-end
bearings using electrical discharge machining. Vibration acceleration was measured
under different motor loads and defect sizes.
These are fault diagnosis data, rather than run-to-failure lifetime data.

Official dataset information and downloads:
https://engineering.case.edu/bearingdatacenter
https://engineering.case.edu/bearingdatacenter/download-data-file

CSV distribution and description for Chapter 4:
https://info.endaq.com/hubfs/Plots/bearing_data.csv
https://blog.endaq.com/top-vibration-metrics-to-monitor-how-to-calculate-them

2. Original filenames and data structure

The original files are in MATLAB MAT format. The download pages are organized as follows:

  Normal Baseline Data: healthy bearings, 48 kHz
  12k Drive End Bearing Fault Data: drive-end bearing faults, 12 kHz
  48k Drive End Bearing Fault Data: drive-end bearing faults, 48 kHz
  12k Fan End Bearing Fault Data: fan-end bearing faults, 12 kHz

Links on these pages identify bearing condition and motor load.
Normal denotes a healthy bearing; IR/OR/B denote inner-race/outer-race/ball defects.
007/014/021 denote defect diameters of 0.007/0.014/0.021 inch.
The suffixes _0/_1/_2/_3 denote motor loads of 0/1/2/3 HP.
For outer-race defects, @6, @3, and @12 denote the defect's clock positions.

Downloading a link provides a numbered file such as 100.mat.
The main variables within the MAT files are:

  Xnumber_DE_time: vibration acceleration array from the drive-end sensor
  Xnumber_FE_time: vibration acceleration array from the fan-end sensor
  Xnumber_BA_time: vibration acceleration array from the base sensor (some files)
  XnumberRPM: measured rotational speed

Example: Normal_3 on the Normal page provides 100.mat: 3 HP, 48 kHz.
Example: IR021_3 on the 48k drive-end page provides 217.mat:
         drive-end inner-race defect, 0.021 inch, 3 HP, 48 kHz.

3. Data used in Chapter 3: feature extraction and selection for healthy,
   inner-race, and outer-race conditions

Drive-end sensor signals at 3 HP are selected from the normal baseline and
48k drive-end bearing fault data. The defect diameter is 0.021 inch,
and the outer-race defect is at the 6 o'clock position.
The sampling rate is 48,000 Hz and the nominal rotational speed is approximately 1,730 rpm.

  Healthy: Normal_3 -> 100.mat -> X100_DE_time -> Normal.mat
  Inner race: IR021_3 -> 217.mat -> X217_DE_time -> Inner.mat
  Outer race: OR021@6_3 -> 241.mat -> X241_DE_time -> Outer.mat

Each continuous signal, approximately 10 seconds long, was divided into
10 consecutive segments and stored column by column.

  Normal.mat: x = 48,564 rows x 10 columns
  Inner.mat:  x = 48,912 rows x 10 columns
  Outer.mat:  x = 48,912 rows x 10 columns

  Number of rows: vibration samples in one segment
  Number of columns: segments obtained from the original signal

The 10 columns represent segments of one continuous recording, not 10 independent tests.
The original signals do not contain exactly 480,000 samples, so each segment
is not exactly one second long.

Files in the folder:

  Normal.mat, Inner.mat, Outer.mat: segmented vibration arrays x for Chapter 3
  info.mat: sampling rate, rotational frequency, bearing geometry, and other Chapter 3 information

4. Data used in Chapter 4: acceleration and velocity RMS for different defect sizes

Drive-end sensor signals are selected for fan-end inner-race defects of
0.007, 0.014, and 0.021 inch at a motor load of 1 HP.
The original fault signals were sampled at 12,000 Hz, and the nominal
rotational speed is approximately 1,772 rpm.
The original healthy signal was sampled at 48,000 Hz.
The faults are in the fan-end bearing, while the selected signals were
measured by the drive-end sensor.

  Healthy: Normal_1 -> 98.mat -> X098_DE_time -> Normal column
  Inner race, 0.007 inch: IR007_1 -> 279.mat -> X279_DE_time -> Fault_007 column
  Inner race, 0.014 inch: IR014_1 -> 275.mat -> X275_DE_time -> Fault_014 column
  Inner race, 0.021 inch: IR021_1 -> 271.mat -> X271_DE_time -> Fault_021 column

bearing_data.csv is a processed dataset distributed by enDAQ.
It contains the first 120,000 samples of each original array,
organized into 120,000 rows and five columns:

  Column 1, Time: time (s)
  Column 2, Fault_021: vibration acceleration for a 0.021-inch inner-race defect (g)
  Column 3, Fault_014: vibration acceleration for a 0.014-inch inner-race defect (g)
  Column 4, Fault_007: vibration acceleration for a 0.007-inch inner-race defect (g)
  Column 5, Normal: vibration acceleration for the healthy condition (g)

This CSV and the Chapter 4 code use a common time axis corresponding to approximately 12,000 Hz.
However, the Normal column uses the first 120,000 samples of the 48,000 Hz
original signal without resampling. Its actual duration of approximately
2.5 seconds is therefore displayed as approximately 10 seconds in the CSV.
This time-axis mismatch must be considered when interpreting the healthy
signal's frequencies and velocity RMS.
Chapter 4 does not use info.mat or its Chapter 3 sampling rate of 48,000 Hz.

Files in the folder:

  bearing_data.csv: time axis and vibration signals for four conditions used in Chapter 4

5. Preparing the textbook data from the original files

MAT files for Chapter 3

  1) Download Normal_3 (100.mat) from Normal Baseline Data.
  2) Download IR021_3 (217.mat) and OR021@6_3 (241.mat), both at 3 HP,
     from 48k Drive End Bearing Fault Data.
  3) Extract X100_DE_time, X217_DE_time, and X241_DE_time, respectively.
     Since 217.mat also contains X215 variables, explicitly select X217.
  4) Divide the total sample count by 10 and use the integer quotient as
     the number of samples per segment. Discard the remaining tail samples
     and arrange 10 consecutive segments as columns.
     Discard the last three samples for the healthy signal and the last
     five samples for each faulty signal.
  5) Save the arrays as Normal.mat, Inner.mat, and Outer.mat, using x as
     the variable name. Store shared measurement settings and bearing
     geometry in info.mat.

CSV file for Chapter 4

  1) Download Normal_1 (98.mat) from Normal Baseline Data.
  2) Download IR007_1 (279.mat), IR014_1 (275.mat), and IR021_1 (271.mat),
     all at 1 HP, from 12k Fan End Bearing Fault Data.
  3) Select the first 120,000 samples of X098_DE_time, X279_DE_time,
     X275_DE_time, and X271_DE_time, respectively.
  4) To reproduce the existing textbook CSV, create 120,000 time values
     spanning 0 to 10 seconds, including both endpoints. Arrange the columns
     as Time, Fault_021, Fault_014, Fault_007, and Normal.
  5) Save as bearing_data.csv with column names in the first row and no row index.

To create a new CSV with a correct common time axis, resample the healthy
signal from 48 kHz to 12 kHz, then select 120,000 samples covering approximately
10 seconds. The steps above reproduce the existing CSV; results obtained
with a resampled CSV may differ from the existing results.

When preparing data for other conditions, check the original sensor locations
and sampling rates. If the rates differ, resample the signals to a common
sampling rate before constructing the time axis.
