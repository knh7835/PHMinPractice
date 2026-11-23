KAU Bearing Data Guide

1. Data source and test overview

These bearing vibration data were collected at Korea Aerospace University
(KAU) under normal and artificial inner-race fault conditions and are used
to compare signal-processing methods.

The test bearing was an NSK NJ2306 cylindrical roller bearing. A 0.3 mm-wide
inner-race defect was introduced by wire cutting. The rotational speed was
1,000 rpm. Each file contains 1 second of data sampled at 51.2 kHz.
See Section 2.3.6 of the textbook for the test rig and sensor locations.

2. CSV filenames and data structure

  Normal.csv: normal condition, 200 kgf load
  Innerrace_1.csv: inner-race fault, no load
  Innerrace_2.csv: inner-race fault, 200 kgf load
  Innerrace_3.csv: inner-race fault, no load

Each CSV file contains 51,200 rows and 4 columns of numerical data without
a header. Column 1 is time in seconds; columns 2-4 are vibration signals
measured by sensors at three locations.

3. Data used in the textbook

Chapter 2 uses Innerrace_2.csv for signal-processing exercises, selecting
the following channels:

  Column 2 (data[:, 1]): sensor 1 -> test bearing with an artificial fault
  Column 4 (data[:, 3]): sensor 3 -> normal support bearing

The normal signal therefore comes from sensor 3 in Innerrace_2.csv,
rather than the separate Normal.csv file. Fault-induced vibration at
sensor 1 may consequently propagate to sensor 3.

4. Files in this folder

  Normal.csv, Innerrace_1.csv, Innerrace_2.csv, Innerrace_3.csv: practice CSV files

5. Reading the data and using other records

Normal and Innerrace_2 were measured at 200 kgf, whereas the other two
files were measured without load. Check the load conditions when comparing
separate files.
