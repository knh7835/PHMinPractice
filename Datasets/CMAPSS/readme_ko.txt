C-MAPSS 터보팬 엔진 데이터세트

1. 데이터 출처 및 개요

NASA Ames Research Center에서 공개한 Turbofan Engine Degradation Simulation
Data Set이다. C-MAPSS(Commercial Modular Aero-Propulsion System Simulation)를
이용하여 터보팬 엔진의 열화를 모사한 데이터로, 실제 엔진에서 측정한 데이터는 아니다.
엔진별 초기 마모와 제조 편차, 운전조건 및 센서 잡음을 반영하며,
정상 상태에서 열화가 진행되어 고장에 이르는 센서값의 변화를 제공한다.

원본 다운로드:
https://www.nasa.gov/intelligent-systems-division/discovery-and-systems-health/pcoe/pcoe-data-set-repository/
위 페이지의 “6. Turbofan Engine Degradation Simulation”에서 내려받는다.

관련 논문:
Saxena, A., Goebel, K., Simon, D., & Eklund, N. (2008).
Damage Propagation Modeling for Aircraft Engine Run-to-Failure Simulation.
2008 International Conference on Prognostics and Health Management, 1–9.
https://doi.org/10.1109/PHM.2008.4711414

2. 원본 파일명과 데이터 구성

원본은 공백으로 구분된 TXT 형식이며, 다음 네 데이터로 구성된다.
FD004의 엔진 수는 원본 안내자료와 실제 배포 파일이 다르며, 여기서는 실제 배포 파일 기반으로 하였다.

  FD001: 학습 100개·시험 100개 엔진, 운전조건 1개, 고압압축기 열화
  FD002: 학습 260개·시험 259개 엔진, 운전조건 6개, 고압압축기 열화
  FD003: 학습 100개·시험 100개 엔진, 운전조건 1개, 고압압축기·팬 열화
  FD004: 학습 249개·시험 248개 엔진, 운전조건 6개, 고압압축기·팬 열화

각 데이터에는 다음 세 파일이 있다.

  train_FD00#.txt: 학습 엔진의 고장까지의 전체 운전이력
  test_FD00#.txt: 시험 엔진의 고장 이전 특정 사이클까지의 운전이력
  RUL_FD00#.txt: 시험 엔진의 마지막 사이클에서의 실제 잔여수명

학습·시험 엔진은 서로 다른 엔진이며, 각 파일에서 엔진 번호를 따로 부여한다.
train 및 test 파일에서 각 행은 특정 엔진의 한 운전 사이클, 각 열은 다음과 같다.

  1열: 엔진 번호
  2열: 운전 사이클 번호
  3~5열: 운전조건 변수 3개
  6~26열: 센서값 21개(S1~S21)

엔진별 운전이력이 엔진 번호와 사이클 순서로 이어져 있다.
RUL 파일은 각 시험엔진의 RUL값이 순서대로 수록되어 있다. (단위는 남은 운전 사이클 수이다.)

3. 교재에서 사용하는 데이터

교재에서는 원본 파일을 변환없이 그대로 사용한다.
폴더에 FD001~FD004를 모두 수록하였으며, 이중에서 FD001을 교재 실습에 사용한다.
FD001은 단일 운전조건에서 고압압축기가 열화한 사례로, 학습 엔진 100개, 시험 엔진 100개로 구성된다.

  train_FD001.txt: 20,631행 × 26열
  test_FD001.txt: 13,096행 × 26열
  RUL_FD001.txt: 100행 × 1열

8장에서는 MLP, CNN, LSTM 및 Transformer로 잔여수명을 예측한다.
9장에서는 사전학습된 Chronos-2의 임베딩과 회귀 헤드를 이용하여 잔여수명을 예측한다.
두 장 모두 전체 학습 엔진을 모두 학습한 경우와 10개 엔진만 학습한 경우의 예측성능을 비교한다.

4. 폴더 내 파일의 구분

  train_FD001.txt~train_FD004.txt: 학습용 운전이력
  test_FD001.txt~test_FD004.txt: 시험용 운전이력
  RUL_FD001.txt~RUL_FD004.txt: 시험 엔진의 실제 잔여수명
  Damage Propagation Modeling.pdf: 데이터 생성 방법을 설명한 논문

5. 다른 데이터를 사용하는 방법

FD003은 입력 파일명을 변경하면 기존 절차를 그대로 적용할 수 있다. 
FD002·FD004는 운전조건이 6개이므로, 운전조건별 정규화 및 센서선정 등의 전처리 수정이 필요하다.