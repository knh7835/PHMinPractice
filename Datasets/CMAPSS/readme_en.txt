C-MAPSS Turbofan Engine Dataset

1. Data source and overview

The Turbofan Engine Degradation Simulation Data Set was released by NASA
Ames Research Center. It simulates turbofan engine degradation using C-MAPSS
(Commercial Modular Aero-Propulsion System Simulation), rather than measurements
from actual engines. It accounts for differences in initial wear and manufacturing,
operating conditions, and sensor noise, and provides sensor values as engines
degrade from normal operation to failure.

Original data download:
https://www.nasa.gov/intelligent-systems-division/discovery-and-systems-health/pcoe/pcoe-data-set-repository/
Download from “6. Turbofan Engine Degradation Simulation” on this page.

Related publication:
Saxena, A., Goebel, K., Simon, D., & Eklund, N. (2008).
Damage Propagation Modeling for Aircraft Engine Run-to-Failure Simulation.
2008 International Conference on Prognostics and Health Management, 1–9.
https://doi.org/10.1109/PHM.2008.4711414

2. Original filenames and data structure

The original files are space-separated TXT files organized into four datasets.
For FD004, the engine counts in the original documentation differ from those
in the distributed files; the counts below are based on the actual files.

  FD001: 100 training and 100 test engines; one operating condition; high-pressure compressor degradation
  FD002: 260 training and 259 test engines; six operating conditions; high-pressure compressor degradation
  FD003: 100 training and 100 test engines; one operating condition; high-pressure compressor and fan degradation
  FD004: 249 training and 248 test engines; six operating conditions; high-pressure compressor and fan degradation

Each dataset contains the following three files.

  train_FD00#.txt: Complete operating histories of training engines up to failure
  test_FD00#.txt: Operating histories of test engines up to a selected cycle before failure
  RUL_FD00#.txt: True remaining useful life at the last recorded cycle of each test engine

Training and test engines are different engines, with engine IDs assigned
separately in each file. Each row in a train or test file represents one
operating cycle of a particular engine. The columns are as follows.

  Column 1: Engine ID
  Column 2: Operating cycle number
  Columns 3–5: Three operating condition variables
  Columns 6–26: Twenty-one sensor values (S1–S21)

Engine histories are arranged by engine ID and cycle number.
The RUL file lists the RUL values for the test engines in order.
The unit is the number of remaining operating cycles.

3. Data used in the textbook

The textbook uses the original files directly without conversion.
The folder contains FD001–FD004, of which FD001 is used in the textbook exercises.
FD001 represents high-pressure compressor degradation under a single operating
condition and contains 100 training engines and 100 test engines.

  train_FD001.txt: 20,631 rows × 26 columns
  test_FD001.txt: 13,096 rows × 26 columns
  RUL_FD001.txt: 100 rows × 1 column

Chapter 8 predicts remaining useful life using MLP, CNN, LSTM, and Transformer models.
Chapter 9 predicts remaining useful life using embeddings from pretrained
Chronos-2 and a regression head.
Both chapters compare prediction performance using the full training engine
dataset and a subset of only 10 engines.

4. Files in this folder

  train_FD001.txt–train_FD004.txt: Training operating histories
  test_FD001.txt–test_FD004.txt: Test operating histories
  RUL_FD001.txt–RUL_FD004.txt: True remaining useful life of test engines
  Damage Propagation Modeling.pdf: Paper describing how the data were generated

5. Using other datasets

FD003 can be processed using the existing procedure by changing the input filenames.
FD002 and FD004 contain six operating conditions and require changes to preprocessing,
such as operating-condition-specific normalization and sensor selection.
