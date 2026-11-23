HS Gear Dataset Guide

1. Data Source and Measurement Overview

This dataset contains wind turbine high-speed gear vibration data provided by
Eric Bechhoefer. Measurements were collected from a 3 MW V90 wind turbine
under healthy conditions and with a naturally occurring pinion gear fault.
The dataset consists of 6 healthy and 11 faulty measurement files.

The original distribution site, data-acoustics.com, no longer provides the data.
The Korea Aerospace University laboratory redistributes the dataset with
permission from the provider.

Dataset information and download:
https://www.kau-sdol.com/kaug

Data source:
Eric Bechhoefer, High Speed Gear Dataset, Acoustics and Vibration Database (2014).

2. Distributed Filenames and Data Structure

Extracting HS_gear.zip distributed by the laboratory produces the following
folders and files:

  data1/case_1.mat through case_11.mat: faulty-condition measurements
  data2/case_1.mat through case_6.mat: healthy-condition measurements

Vibration signals and measurement parameters are provided in MATLAB MAT format.
Each file contains the following variables:

  gs: vibration acceleration signal
  tach: array of tachometer pulse times for rotational synchronization
  sr: sampling frequency, 97,656 Hz
  ppr: number of tachometer pulses per revolution, 8
  teeth: number of pinion gear teeth, 32

Each gs array has 585,936 rows and 1 column, representing approximately
6 seconds of vibration measurements. The shaft rotational frequency is
approximately 30 Hz, and the gear mesh frequency is 960 Hz.

gs and tach are separate arrays and do not need to have the same number of elements.
Each element of gs is a vibration sample; each element of tach is a pulse time.
The case number identifies a measurement and does not indicate fault severity.

3. Use in the Textbook

In the textbook dataset, the distributed data1 and data2 folders are named
Fault and Normal, respectively. The parameters sr, ppr, and teeth, which are
identical across all files, are stored in a shared info.mat file.
Each case MAT file contains only gs and tach.

Chapter 2: Uses Fault/case_1.mat for gear signal processing, including
           time synchronous averaging (TSA).
Chapter 3: Extracts features from the 6 healthy and 11 faulty files and compares
           their ability to distinguish healthy and faulty conditions.

4. Files in the Folder

  Fault/case_1.mat through case_11.mat: gs and tach arrays under faulty conditions
  Normal/case_1.mat through case_6.mat: gs and tach arrays under healthy conditions
  info.mat: shared measurement parameters sr, ppr, and teeth

info.mat is located in the Gear_HS folder and is used by all files in both
Fault and Normal.
