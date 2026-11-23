ZeMA 유압시스템 데이터 사용 안내

1. 데이터 출처 및 시험 개요

독일 ZeMA의 유압 시험장치에서 냉각기·밸브·펌프·축압기의 상태를 변화시키며
수집한 다중 센서 데이터이다. 60초 부하 사이클을 총 2,205회 수행하였으며,
여러 부품의 결함이 동시에 포함될 수 있다.

공식 데이터 소개 및 다운로드:
https://archive.ics.uci.edu/dataset/447/condition+monitoring+of+hydraulic+systems

데이터 출처: Helwig, N., Pignanelli, E., and Schütze, A. (2015).
Condition monitoring of hydraulic systems, UCI Machine Learning Repository.
https://doi.org/10.24432/C5CW21

시험장치 및 측정방법: Helwig, N., Pignanelli, E., and Schütze, A.,
“Condition Monitoring of a Complex Hydraulic System Using Multivariate Statistics,”
IEEE I2MTC, 2015. https://doi.org/10.1109/I2MTC.2015.7151267

2. 원본 파일명과 데이터 구성

원본은 탭으로 구분된 숫자 배열을 담은 TXT 파일이다.
PS1.txt, FS1.txt처럼 채널별 파일로 제공하며, 행은 2,205개 측정 사이클,
열은 한 사이클의 60초 동안 수집한 샘플이다.

  PS1~PS6: 압력(bar), 100 Hz, 각각 2,205행 × 6,000열
  EPS1: 모터 전력(W), 100 Hz, 2,205행 × 6,000열
  FS1~FS2: 유량(L/min), 10 Hz, 각각 2,205행 × 600열
  TS1~TS4: 온도(°C), 1 Hz, 각각 2,205행 × 60열
  VS1: 진동(mm/s), 1 Hz, 2,205행 × 60열
  CE: 냉각 효율(%), CP: 냉각 출력(kW), SE: 효율 계수(%)
      각각 1 Hz, 2,205행 × 60열이며 CE·CP는 가상 센서이다.

profile.txt는 사이클별 상태정보를 담은 2,205행 × 5열 배열이다.
센서 파일과 같은 행은 같은 사이클에 해당하며, 각 열의 의미는 다음과 같다.

  1열 냉각기(%): 100 정상 효율, 20 효율 저하, 3 거의 고장 상태
  2열 밸브(%): 100 정상 전환, 90 작은 전환 지연, 80 심한 전환 지연, 73 거의 고장 상태
  3열 펌프 내부 누설: 0 없음, 1 약한 누설, 2 심한 누설
  4열 축압기(bar): 130 정상 압력, 115 약간 저하, 100 크게 저하, 90 거의 고장 상태
  5열 안정 상태 여부: 0 안정, 1 아직 안정 상태에 도달하지 않았을 가능성 있음

밸브의 백분율은 전환 동작의 상태이며 개도율이 아니다.

3. 교재에서의 활용

원본 데이터 전체를 txt와 동일한 구조를 유지하면서 MAT 형식으로 저장하였다. 
14개 센서와 profile을 사용하며, CE·CP·SE는 입력으로 사용하지 않는다.

4장: 오토인코더로 냉각기 이상을 감지한다. 효율 100%인 741개 사이클을
     학습·검증·시험에 사용하고, 20%인 732개 사이클을 이상 평가에 사용한다.
     3% 상태는 평가에서 제외한다. 여기서 정상은 냉각기만을 기준으로 한다.

5장: ANN은 14개 센서의 평균·표준편차·왜도·첨도, 총 56개 특징을 이용하여
     네 부품의 정상·결함을 동시에 진단한다. 정상값은 냉각기·밸브·펌프·축압기
     각각 100·100·0·130이며, 나머지 수준은 결함으로 묶는다.
     CNN은 센서 신호로 선택한 부품의 상태 수준을 분류한다.
     target_name에서 cooler, valve, pump, accumulator 중 하나를 선택하면 
     해당 부품에 대한 상태수준을 분류한 진단결과를 얻는다. 

현재 4·5장 실습에서는 profile의 5열에 따른 안정 상태 여부로 데이터를 제외하지 않는다.

4. 폴더 내 파일의 구분

  PS1.mat~PS6.mat, EPS1.mat: 압력 및 모터 전력
  FS1.mat~FS2.mat: 유량
  TS1.mat~TS4.mat: 온도
  VS1.mat: 진동
  CE.mat, CP.mat, SE.mat: 냉각 효율·냉각 출력·효율 계수
  profile.mat: 사이클별 상태정보
  documentation.txt: 제공자 원본 문서. 부품별 상태 수준의 데이터 개수와 기존 연구 결과를 수록한다.

