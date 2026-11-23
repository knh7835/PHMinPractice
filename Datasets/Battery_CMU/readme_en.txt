CMU Battery Dataset

1. Data source and test overview

The eVTOL Battery Dataset was released by Carnegie Mellon University.
A total of 21,392 charge-discharge cycles were conducted across 22 Sony-Murata
18650 VTC-6 lithium-ion cells. Discharge profiles simulating takeoff, cruise,
and landing were repeated with rest and charging. Battery degradation was
measured under different discharge powers, cruise durations, charging currents,
end-of-charge voltages, and ambient temperatures.
In addition to the simulated mission tests, reference performance tests (RPTs)
were conducted at regular cycle intervals to measure capacity.

Original data download:
https://kilthub.cmu.edu/articles/dataset/eVTOL_Battery_Dataset/14226830

Related publication:
Bills, A., Sripad, S., Fredericks, L., et al. (2023).
A battery dataset for electric vertical takeoff and landing aircraft.
Scientific Data, 10, 344. https://doi.org/10.1038/s41597-023-02180-5

2. Original filenames and data structure

The original files are in CSV format. VAH##.csv contains the test history of
one cell, where ## is the cell identifier. VAH##_impedance.csv contains the
internal resistance data for that cell.

The test conditions for each cell are as follows.

  VAH01, VAH17, VAH27: Baseline conditions
  VAH02, VAH15, VAH22: Cruise duration of 1,000 s
  VAH05, VAH28: Discharge power reduced by 10% relative to the baseline
  VAH06, VAH24: Constant-current (CC) charging current of C/2
  VAH07: Constant-voltage (CV) charging voltage of 4.0 V
  VAH09, VAH25: Thermal chamber temperature of 20°C
  VAH10: Thermal chamber temperature of 30°C
  VAH11: Discharge power reduced by 20% relative to the baseline
  VAH12: Cruise duration of 400 s
  VAH13, VAH26: Cruise duration of 600 s
  VAH16, VAH20: Constant-current (CC) charging current of 1.5C
  VAH23: Constant-voltage (CV) charging voltage of 4.1 V
  VAH30: Thermal chamber temperature of 35°C

Each row in VAH##.csv is an individual sample recorded during testing.
The file contains the following 10 columns.

  time_s: Elapsed time since the start of the test (s)
  Ecell_V: Cell terminal voltage (V)
  I_mA: Cell current (mA); positive for charging and negative for discharging
  EnergyCharge_W_h: Charge energy (Wh)
  QCharge_mA_h: Charge capacity (mAh)
  EnergyDischarge_W_h: Discharge energy (Wh)
  QDischarge_mA_h: Discharge capacity (mAh)
  Temperature__C: Cell surface temperature (°C)
  cycleNumber: Cycle number recorded by the test equipment
  Ns: Test segment identifier within a cycle

Because multiple test files were combined, cycleNumber may not correspond
to a continuous cycle count over the entire test history. Some tests contain
missing or interrupted sections; details are provided in the original README_original.txt.

Each row in an internal resistance (impedance) file represents one resistance
measurement. The file contains the following five columns.

  20%_1_second, 20%_30_second: Internal resistance at 20% depth of discharge (DOD), evaluated at 1 s and 30 s (Ω)
  60%_1_second, 60%_30_second: Internal resistance at 60% DOD, evaluated at 1 s and 30 s (Ω)
  cycle numbers: Corresponding cycle number

3. Data used in the textbook

This folder contains the original CSV files for the following five of the 22 cells.

  VAH01: Baseline mission conditions
  VAH11: Discharge power reduced by 20% relative to the baseline
  VAH12: Cruise duration of 400 s
  VAH13, VAH26: Cruise duration of 600 s

The textbook uses VAH12, which completed 2,348 charge-discharge cycles,
the largest number among the 22 cells.
Chapter 6 uses VAH12.csv and VAH12_impedance.csv to identify a battery
equivalent circuit model and estimate state of charge (SOC) using an
extended Kalman filter (EKF).
Chapter 7 constructs a state of health (SOH) estimation model using charging
curves and capacities measured during RPTs. SOH is then estimated from the
charging voltage and charge capacity of each cycle, and remaining useful
life is predicted using a particle filter (PF).

4. Files in this folder

  VAH01.csv, VAH11.csv, VAH12.csv, VAH13.csv, VAH26.csv: Test histories for individual cells
  VAH01_impedance.csv, VAH11_impedance.csv, VAH12_impedance.csv: Internal resistance data for individual cells
  README_original.txt: Original provider’s documentation of test conditions, variables, and data anomalies

5. Using other cells

Download the desired VAH##.csv from the original site, place it in the same
folder, and change test_cell in the notebook to the corresponding cell name.
Chapter 6 also requires VAH##_impedance.csv for the same cell.
For cells tested under different conditions, check that the analysis intervals
and criteria for distinguishing test types are appropriate.

