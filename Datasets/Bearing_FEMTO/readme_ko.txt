FEMTO 베어링 데이터 사용 안내

1. 데이터 출처 및 시험 개요

이 데이터는 프랑스 FEMTO-ST 연구소가 PRONOSTIA 시험장치에서 수행한
베어링 가속수명시험 데이터이다. IEEE PHM 2012 Data Challenge에 사용되었으며,
NASA Prognostics Data Repository에서도 공개되어 있다.

베어링에 인위적인 초기 결함을 가공하는 대신, 회전과 하중을 가하여 열화를
진행시키면서 진동과 온도를 측정하였다. 

대회 데이터는 학습용 베어링 6개와 시험용 베어링 11개로 구성된다.
학습용은 고장까지의 이력이 제공되며, 대회 시험용은 고장 이전에 잘린 이력으로
남은 수명을 예측하도록 구성되었다. 전체 시험이력을 제공하는 배포본과
대회용으로 잘린 시험 데이터는 구분해야 한다.

공식 배포 페이지:
https://www.nasa.gov/intelligent-systems-division/discovery-and-systems-health/pcoe/pcoe-data-set-repository/

원본 다운로드:
https://phm-datasets.s3.amazonaws.com/NASA/10.+FEMTO+Bearing.zip

데이터 인용:
“FEMTO Bearing Data Set”, NASA Prognostics Data Repository,
NASA Ames Research Center, Moffett Field, CA.

관련 논문:
P. Nectoux, R. Gouriveau, K. Medjaher, E. Ramasso, B. Morello,
N. Zerhouni, and C. Varnier (2012).
“PRONOSTIA: An Experimental Platform for Bearings Accelerated Life Test”,
IEEE International Conference on Prognostics and Health Management, Denver, CO, USA.

공식 대회 설명문서 “IEEEPHM2012-Challenge-Details.pdf”는 시험장치,
측정조건과 원본 파일 구성을 설명한다. 본 안내는 교재 실습을 위한 보충 설명이다.

2. 원본 파일명과 데이터 구성

시험조건은 다음 세 가지이고 조건당 시험한 베어링은 각각 7, 7, 3개다.

  조건 1: 회전속도 1,800 rpm, 반경방향 하중 4,000 N
  조건 2: 회전속도 1,650 rpm, 반경방향 하중 4,200 N
  조건 3: 회전속도 1,500 rpm, 반경방향 하중 5,000 N

원본 진동 데이터는 MATLAB MAT 파일이 아니라 ASCII 숫자 데이터가 저장된
CSV 파일이다. 다운로드한 압축파일을 해제하고, 내부 압축파일이 있으면 추가로
해제하여 베어링별 측정파일을 찾는다.

베어링 폴더명은 Bearing조건번호_베어링번호 으로 정의된다.
예: Bearing1_4 -> 운전조건 1에서 시험한 네 번째 베어링

폴더내에서 진동 측정파일 이름: acc_00001.csv, acc_00002.csv, ... 으로 정의된다.
숫자는 측정 순서를 나타내며, 측정 시각은 파일 내부에 기록되어 있다.
파일마다 2,560행과 6열이 있으며, 열의 구성은 다음과 같다.
  1열: 시
  2열: 분
  3열: 초
  4열: 마이크로초
  5열: 수평 방향 진동 가속도
  6열: 수직 방향 진동 가속도

샘플링 주파수는 25,600 Hz이며, 10초마다 0.1초 동안 측정한 2,560개 샘플을 한 파일에 저장한다. 

3. 교재에서 사용하는 데이터

교재의 대표 사례는 다음 두 가지이다.

  d1_x: Bearing1_1의 수평 방향 진동신호
        배열 크기: 2,560행 × 2,803열
  d4_x: Bearing1_4의 수평 방향 진동신호
        배열 크기: 2,560행 × 1,428열

두 사례 모두 운전조건 1에 해당한다. d1, d4는 교재에서 사용하는 사례명이며,
x는 수평 방향, y는 수직 방향 신호를 구분하는 표기이다.

실습 편의를 위해 동일 사례·방향의 측정신호를 측정 순서대로 모아
개별 MAT 파일로 구성하였고 파일의 배열 방향은 다음과 같다.
  행 수: 1회 측정에서 얻은 샘플 수
  열 수: 총 측정 횟수

2장: 두 대표 사례의 진동신호를 읽고 고장까지의 원신호를 보여준다.
3장: 진동신호에서 특징을 추출하고 특징 CSV를 생성한다.
7장: 3장에서 생성한 특징 CSV로 건전성 인자를 구성하고 잔여수명을 예측한다.

4. 폴더 내 파일의 구분

  d1_x/y.mat ~ d7_x/y.mat: 운전조건 1에서 각 사례의 수평 수직 방향 진동신호
  info.mat: 샘플링 주파수, 측정당 샘플 수, 회전속도, 베어링 제원 등의 메타데이터
  *_features.csv: 노트북에서 추출한 특징 데이터
  *.gif, *.mp4: 노트북에서 생성한 결과 애니메이션

info.mat의 회전 주파수 fr=30 Hz는 운전조건 1의 1,800 rpm에 해당한다.
다른 운전조건의 데이터를 추가할 때는 회전 주파수도 해당 조건에 맞춰야 한다.

5. 다른 사례 또는 방향을 사용하는 방법

원본을 내려받은 후 다음 절차로 다른 사례를 준비할 수 있다.

  1) 원하는 운전조건과 베어링 폴더를 선택한다.
  2) acc_*.csv만 선택하고 파일 번호순으로 정렬한다.
  3) 실제 구분자에 맞춰 각 CSV를 읽는다.
  4) 수평 방향은 5열, 수직 방향은 6열을 추출한다.
  5) 추출한 신호를 열 방향으로 쌓아 2,560행의 배열로 구성한다.
  6) 파일명과 내부 변수명을 일치시켜 MAT 파일로 저장한다.
     운전조건이 달라지면 회전 주파수 등 관련 정보도 함께 수정한다.
