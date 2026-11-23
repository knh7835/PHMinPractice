PMDC Motor Dataset Guide

1. Data source and test overview

This public dataset contains measurements of input voltage, current, and
rotational speed from permanent magnet direct current (PMDC) worm-geared
motors. Motors A and B, which are of the same model, were tested with
various input signals under no-load conditions and loads applied using
masses of 1–4 kg. The data are intended for dynamic model identification
and external load estimation, rather than motor fault diagnosis or
run-to-failure testing. The sampling rate is approximately 100 Hz.

Original dataset:
Permanent Magnet Direct Current Worm Geared Motor Data: Voltage, Current, and Speed
https://data.mendeley.com/datasets/2rkpsss6fd/2
https://doi.org/10.17632/2rkpsss6fd.2

Related publication:
dos Reis, W. P. N., Couto, G. E., and Morandin Junior, O.,
“Direct current geared motor data: Voltage, current, and speed measured under
different experimental conditions,” Data in Brief, 40, 107802, 2022.
https://doi.org/10.1016/j.dib.2022.107802

2. Original files and data structure

The original measurements are stored in CSV format and organized by
motor (A/B) and input signal. No-load tests use step, sine, triangle,
square-wave, and pseudorandom binary sequence (PRBS) inputs.
Loaded tests use step inputs. The measurements include time information,
current, voltage, speed, raw sensor readings, and motor control information.

3. Textbook data structure and use

Only Motor A step-input tests were selected and assembled into PMDC_MotorA.mat.
There are seven PWM levels from 40% to 100% and five load levels:
no load and 1–4 kgf. Each condition has 10–11 repeated tests, except
40% PWM at 4 kgf, which has two tests, giving 353 tests in total.
The number of repeated tests for each condition is:

  PWM (%)   0 kgf  1 kgf  2 kgf  3 kgf  4 kgf
  40          10     11     11     10      2
  50          10     11     11     10     10
  60          10     11     11     10     10
  70          10     11     11     10     10
  80          10     10     10     10     10
  90          10     10     11     10     10
  100         10     11     11     10     10

Each condition is stored in the MAT file under the variable name
pwm{PWM}_load{load level}.
Example: pwm50_load2 contains repeated tests at 50% PWM and a load of 2 kgf.

Each variable is a MATLAB cell array with one cell per repeated test.
Each cell contains an array of shape (N, 5). N is the number of samples
measured during one test, and the five columns contain the following variables:

  Column 1, time_s: time (s)
  Column 2, current_A: motor current (A)
  Column 3, speed_rad_s: rotational speed (rad/s)
  Column 4, voltage_V: motor-terminal voltage (V)
  Column 5, pwm: PWM command (%)

N varies with the test condition. For example, pwm50_load2 contains
11 tests, each with 301 rows and five columns.

Chapter 6 identifies the parameters using the first eight tests at each
load level for PWM 80%, 90%, and 100%, giving 3 × 5 × 8 = 120 tests.
The identified parameters and measured current and speed are then used
to estimate the external load under 50% PWM at load levels of 0–4 kgf.

4. Files in the folder

  PMDC_MotorA.mat: Motor A test data for 7 × 5 = 35 PWM–load conditions

5. Using other tests

Within the current MAT file, select the PWM–load variable and repeated
test number. To use Motor B or another input signal, select the corresponding
tests from the original dataset and store the columns in the order:
time (s), current (A), speed (rad/s), voltage (V), and PWM (%).
Check the original units and time information, and identify the parameters
again when using a different motor.
