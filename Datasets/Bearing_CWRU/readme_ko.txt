CWRU 베어링 데이터 사용 안내

1. 데이터 출처 및 시험 개요

이 데이터는 미국 Case Western Reserve University(CWRU)의 Bearing Data Center가
제공하는 정상 및 결함 베어링의 진동 데이터이다. 모터의 구동측(Drive End)과
팬측(Fan End) 베어링에 방전가공으로 내륜, 외륜 및 볼 결함을 부여하고,
모터 부하와 결함 크기를 바꾸어 진동 가속도를 측정하였다.
고장까지의 수명시험 데이터가 아니라 상태별 결함 진단용 데이터이다.

공식 데이터 소개 및 다운로드:
https://engineering.case.edu/bearingdatacenter
https://engineering.case.edu/bearingdatacenter/download-data-file

4장에서 사용하는 CSV의 배포처 및 설명:
https://info.endaq.com/hubfs/Plots/bearing_data.csv
https://blog.endaq.com/top-vibration-metrics-to-monitor-how-to-calculate-them

2. 원본 파일명과 데이터 구성

원본은 MATLAB MAT 형식이며, 다운로드 페이지는 다음과 같이 구분된다.

  Normal Baseline Data: 정상 베어링, 48 kHz
  12k Drive End Bearing Fault Data: 구동측 베어링 결함, 12 kHz
  48k Drive End Bearing Fault Data: 구동측 베어링 결함, 48 kHz
  12k Fan End Bearing Fault Data: 팬측 베어링 결함, 12 kHz

각 페이지 마다 결함정보 및 부하를 나타내는 링크가 있으며, 여기서 
Normal은 정상, IR/OR/B는 내륜/외륜/볼 결함을 뜻한다.
007/014/021은 결함 직경 0.007/0.014/0.021 inch, _0/_1/_2/_3은 모터 부하 0/1/2/3 HP를 뜻한다.
외륜 결함의 @6, @3, @12는 결함의 시계방향 위치이다.

링크 다운로드하면 고유번호로 표시된 번호.mat를 얻으며 내부 변수는 다음과 같다.

  X번호_DE_time: 구동측 센서의 진동 가속도 배열
  X번호_FE_time: 팬측 센서의 진동 가속도 배열
  X번호_BA_time: 베이스 센서의 진동 가속도 배열(일부 파일)
  X번호RPM: 측정 회전속도

예) Normal 페이지에서 Normal_3 을 클릭하면 100.mat을 얻으며 이는 3 HP, 48 kHz 이다.
예) 48k 페이지에서 IR021_3 을 클릭하면 217.mat을 얻으며 이는 구동측 IR, 0.021", 3 HP, 48 kHz 이다.

3. 교재 3장에서 사용하는 데이터: 정상·내륜·외륜의 특징 추출 및 선정

정상 데이터와 48k 구동측 베어링 결함 데이터에서 부하 3 HP의 구동측 센서신호를 선택한다.
결함 직경은 0.021 inch이며 외륜 결함 위치는 6시 방향이다.
샘플링 주파수는 48,000 Hz이며 명목 회전속도는 약 1,730 rpm이다.
그 결과 데이터는 다음과 같이 선정된다. 

  정상: Normal_3 → 100.mat → X100_DE_time → Normal.mat
  내륜: IR021_3 → 217.mat → X217_DE_time → Inner.mat
  외륜: OR021@6_3 → 241.mat → X241_DE_time → Outer.mat

약 10초 길이의 연속 신호를 10등분하여 각 열에 저장하였다. 
  Normal.mat: x = 48,564행 × 10열
  Inner.mat:  x = 48,912행 × 10열
  Outer.mat:  x = 48,912행 × 10열

  행 수: 한 구간에 포함된 진동 샘플 수
  열 수: 원본 신호를 나눈 구간 수

따라서 10개 열은 독립된 시험 10회가 아니라 동일한 연속 측정신호의 10개 구간이다.
원본 길이가 정확히 480,000개가 아니므로 각 구간은 정확히 1초가 아니다.

폴더 내 파일의 구분

  Normal.mat, Inner.mat, Outer.mat: 3장에서 사용하는 구간별 진동 배열 x
  info.mat: 3장용 샘플링 주파수, 회전 주파수, 베어링 제원 등의 정보

4. 교재 4장에서 사용하는 데이터: 결함 크기별 가속도와 속도 RMS 비교

부하 1 HP에서 팬측 베어링 내륜 결함 세가지 007/014/021에 대해 구동측 센서신호를 선택한다. 
결함 신호의 원본 샘플링 주파수는 12,000 Hz이며 명목 회전속도는 약 1,772 rpm이다.
정상 신호의 원본 샘플링 주파수는 48,000 Hz이다.
결함은 팬측 베어링에 있지만, 센서신호는 구동측 신호를 사용하였다.

  정상: Normal_1 → 98.mat → X098_DE_time → Normal 열
  내륜 0.007 inch: IR007_1 → 279.mat → X279_DE_time → Fault_007 열
  내륜 0.014 inch: IR014_1 → 275.mat → X275_DE_time → Fault_014 열
  내륜 0.021 inch: IR021_1 → 271.mat → X271_DE_time → Fault_021 열

bearing_data.csv는 enDAQ에서 배포한 가공 데이터이며, 각 원본 배열의
처음 120,000개 샘플을 모아 120,000행 × 5열로 구성하였다.

  1열 Time: 시간(s)
  2열 Fault_021: 내륜 결함 직경 0.021 inch의 진동 가속도(g)
  3열 Fault_014: 내륜 결함 직경 0.014 inch의 진동 가속도(g)
  4열 Fault_007: 내륜 결함 직경 0.007 inch의 진동 가속도(g)
  5열 Normal: 정상 상태의 진동 가속도(g)

이 CSV와 4장 코드는 약 12,000 Hz의 공통 시간축을 사용한다.
다만 정상 열은 48,000 Hz 원본의 처음 120,000개 샘플을 재표본화하지 않고 사용하므로,
정상 신호의 실제 약 2.5초 구간이 CSV에서는 약 10초로 표시된다.
따라서 정상 신호의 주파수 및 속도 RMS 해석에는 이 시간축 불일치를 고려해야 한다.
참고로 4장에서는 info.mat을 사용하지 않으므로, 3장용으로 정의된 fs=48,000 Hz를 적용하지 않는다.

폴더 내 파일의 구분

  bearing_data.csv: 4장에서 사용하는 시간축과 네 상태의 진동신호

5. 원본에서 교재용 데이터를 준비하는 방법

3장용 MAT 파일

  1) Normal Baseline Data에서 Normal_3(100.mat)을 내려받는다.
  2) 48k Drive End Bearing Fault Data에서 부하 3 HP의
     IR021_3(217.mat)와 OR021@6_3(241.mat)을 내려받는다.
  3) 각각 X100_DE_time, X217_DE_time, X241_DE_time을 추출한다.
     217.mat에는 X215 변수도 들어 있으므로 X217을 명시적으로 선택한다.
  4) 전체 샘플 수를 10으로 나눈 몫을 구간별 샘플 수로 정한다.
     끝부분의 나머지 샘플을 제외하고, 시간순으로 연속된 10개 구간을
     각 열에 배치한다. 정상은 마지막 3개, 내륜과 외륜은 마지막 5개가 제외된다.
  5) 내부 변수명은 x로 지정하여 Normal.mat, Inner.mat, Outer.mat으로 저장한다.
     공통 측정조건과 베어링 제원은 info.mat에 저장한다.

4장용 CSV 파일

  1) Normal Baseline Data에서 Normal_1(98.mat)을 내려받는다.
  2) 12k Fan End Bearing Fault Data에서 부하 1 HP의
     IR007_1(279.mat), IR014_1(275.mat), IR021_1(271.mat)을 내려받는다.
  3) 각각 X098_DE_time, X279_DE_time, X275_DE_time, X271_DE_time의
     처음 120,000개 샘플을 선택한다.
  4) 현재 교재 CSV를 동일하게 재현하려면 0~10초를 포함하는
     120,000개 시간값을 만들고 Time, Fault_021, Fault_014, Fault_007,
     Normal 순으로 열을 배치한다.
  5) 첫 행에 열 이름을 넣고 별도의 행 번호 없이 bearing_data.csv로 저장한다.

새 CSV를 올바른 공통 시간축으로 구성하려면 정상 신호를 48 kHz에서 12 kHz로
재표본화한 후 약 10초의 120,000개 샘플을 선택한다. 위 절차는 기존 CSV를
동일하게 재현하는 방법이며, 재표본화한 CSV는 기존 결과와 달라질 수 있다.

다른 조건으로 새 데이터를 구성할 때는 원본의 센서 위치와 샘플링 주파수를
확인하고, 주파수가 서로 다르면 동일 주파수로 재표본화한 후 시간축을 구성한다.
