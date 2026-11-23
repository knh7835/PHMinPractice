PMDC 모터 데이터 사용 안내

1. 데이터 출처 및 시험 개요

영구자석 직류(PMDC) 웜기어 모터의 입력 전압·전류·회전속도를 측정한
공개 데이터이다. 동일 모델의 모터 A와 B에 여러 입력신호를 인가하였으며,
무부하 및 1~4 kg 질량을 이용한 부하 조건에서 시험하였다.
모터 결함이나 고장까지의 수명시험 데이터가 아니라 동적 모델 식별과
외부 부하 추정을 위한 데이터이다. 샘플링 주파수는 약 100 Hz이다.

원본 데이터:
Permanent Magnet Direct Current Worm Geared Motor Data: Voltage, Current, and Speed
https://data.mendeley.com/datasets/2rkpsss6fd/2
https://doi.org/10.17632/2rkpsss6fd.2

관련 논문:
dos Reis, W. P. N., Couto, G. E., and Morandin Junior, O.,
“Direct current geared motor data: Voltage, current, and speed measured under
different experimental conditions,” Data in Brief, 40, 107802, 2022.
https://doi.org/10.1016/j.dib.2022.107802

2. 원본 파일과 데이터 구성

원본 측정값은 CSV 형식으로 저장되며, 모터 A/B와 입력신호별로 구분된다.
무부하에서는 계단·정현파·삼각파·구형파·의사난수 이진신호(PRBS)를 사용하고,
부하가 있는 조건에서는 계단 입력 시험을 수행하였다.
측정값에는 시간 정보, 전류·전압·속도, 센서 원시값과 모터 제어정보가 포함된다.

3. 교재용 데이터 구성 및 활용

모터 A의 계단 입력 시험만 선택하여 PMDC_MotorA.mat으로 정리하였다.
PWM은 40~100%의 7개 수준이며, 부하는 무부하와 1~4 kgf의 5개 수준이다.
각 조건에서 10~11회, PWM 40% 4 kgf에서만 2회의 시험을 포함하여 총 353회 실시하였다.
조건별 반복시험 수는 다음과 같다.

  PWM(%)    0 kgf  1 kgf  2 kgf  3 kgf  4 kgf
  40          10     11     11     10      2
  50          10     11     11     10     10
  60          10     11     11     10     10
  70          10     11     11     10     10
  80          10     10     10     10     10
  90          10     10     11     10     10
  100         10     11     11     10     10

mat 파일내에서 각 조건은 변수명 pwm{PWM}_load{부하수준}으로 구성되어 있다.
예) pwm50_load2: PWM 50%, 부하 2 kgf의 반복시험 모음

각 변수는 반복시험을 담은 MATLAB cell 배열로 구성되어 있으며, 따라서 반복시험 수 만큼 있다.
각 cell은 Array (N, 5)를 담고 있다. 여기서 
행 수 N은 한 회의 시험 동안 측정한 샘플 수, 열 수 5는 다음과 같은 변수이다.

  1열 time_s: 시간(s)
  2열 current_A: 모터 전류(A)
  3열 speed_rad_s: 회전속도(rad/s)
  4열 voltage_V: 모터 단자전압(V)
  5열 pwm: PWM 명령(%)

N은 시험조건 별로 다르다. 예로 pwm50_load2는 11회 시험을 담으며 각 시험은 301행 × 5열이다.

6장에서는 PWM 80·90·100%의 첫 8회 시험, 총 3x5x8=120회 데이터를 이용하여 파라미터를 식별하였다.
이어서 PWM 50%의 0~4 kgf 부하 조건에서 식별한 파라미터와 측정 전류·속도를 이용하여 외부 부하를 추정하였다.

4. 폴더 내 파일의 구분

  PMDC_MotorA.mat: 모터 A의 7x5 = 35개의 PWM·부하 조건에 대한 시험 데이터

5. 다른 시험을 사용하는 방법

현재 MAT 내에서는 PWM·부하 변수와 반복시험 번호를 선택하면 된다.
모터 B나 다른 입력신호를 사용하려면 원본에서 해당 시험을 선택하고,
시간(s)·전류(A)·속도(rad/s)·전압(V)·PWM(%)의 순서로 정리하여 저장한다.
원본의 단위와 시간 정보를 확인하고, 대상 모터가 달라지면 파라미터도 다시 식별한다.
