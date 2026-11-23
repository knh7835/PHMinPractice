CMU 배터리 데이터세트

1. 데이터 출처 및 시험 개요

Carnegie Mellon University에서 공개한 eVTOL Battery Dataset이다.
Sony-Murata 18650 VTC-6 리튬이온 셀 22개에 대해 총 21,392회의 충·방전 사이클 시험을 수행하였다.
이륙·순항·착륙을 모사한 방전과 휴지·충전을 반복하며, 방전전력, 순항시간,
충전전류, 충전종료 전압 및 주변온도를 달리하여 배터리의 열화를 측정하였다.
운항 모사 시험과 별도로 일정 사이클 간격으로 기준 성능시험(RPT)을 수행하여 용량을 측정하였다.

원본 다운로드:
https://kilthub.cmu.edu/articles/dataset/eVTOL_Battery_Dataset/14226830

관련 논문:
Bills, A., Sripad, S., Fredericks, L., et al. (2023).
A battery dataset for electric vertical takeoff and landing aircraft.
Scientific Data, 10, 344. https://doi.org/10.1038/s41597-023-02180-5

2. 원본 파일명과 데이터 구성

원본은 CSV 형식이다. VAH##.csv는 셀 하나의 시험이력을 담으며,
##는 셀 식별번호이다. VAH##_impedance.csv는 해당 셀의 내부저항 데이터이다.

셀별 시험조건은 다음과 같다. 

  VAH01, VAH17, VAH27: 기준 조건
  VAH02, VAH15, VAH22: 순항시간 1,000초
  VAH05, VAH28: 기준 대비 방전전력 10% 감소
  VAH06, VAH24: 정전류(CC) 충전전류 C/2
  VAH07: 정전압(CV) 충전전압 4.0 V
  VAH09, VAH25: 온도 챔버 온도 20°C
  VAH10: 온도 챔버 온도 30°C
  VAH11: 기준 대비 방전전력 20% 감소
  VAH12: 순항시간 400초
  VAH13, VAH26: 순항시간 600초
  VAH16, VAH20: 정전류(CC) 충전전류 1.5C
  VAH23: 정전압(CV) 충전전압 4.1 V
  VAH30: 온도 챔버 온도 35°C

VAH##.csv의 행은 시험 중 측정한 개별 샘플이며, 열은 다음 10개 항목이다.

  time_s: 시험 시작 이후 경과시간(s)
  Ecell_V: 셀 단자전압(V)
  I_mA: 셀 전류(mA), 충전은 양수이고 방전은 음수
  EnergyCharge_W_h: 충전 에너지(Wh)
  QCharge_mA_h: 충전 전하량(mAh)
  EnergyDischarge_W_h: 방전 에너지(Wh)
  QDischarge_mA_h: 방전 전하량(mAh)
  Temperature__C: 셀 표면온도(°C)
  cycleNumber: 시험장비가 기록한 사이클 번호
  Ns: 사이클 내 시험 단계 식별값

cycleNumber는 여러 시험파일을 합치는 과정에서 전체 이력의 연속 번호와
일치하지 않을 수 있다. 일부 시험에는 누락 또는 중단 구간이 있으며,
세부 사항은 원본 README_original.txt에 설명되어 있다.

내부저항(impedance) 파일의 행은 내부저항 측정 회차이며, 열은 다음 5개 항목이다.

  20%_1_second, 20%_30_second: 방전깊이(DOD) 20%에서 1초·30초 기준 내부저항(Ω)
  60%_1_second, 60%_30_second: DOD 60%에서 1초·30초 기준 내부저항(Ω)
  cycle numbers: 해당 사이클 번호

3. 교재에서 사용하는 데이터

현재 폴더에는 전체 22개 셀 중 다음 5개 셀의 원본 CSV를 수록하였다.

  VAH01: 기준 운항 조건
  VAH11: 기준 대비 방전전력 20% 감소
  VAH12: 순항시간 400초
  VAH13, VAH26: 순항시간 600초

교재에서는 22개 배터리 중 가장 많은 2,348회의 충·방전 사이클을 수행한 VAH12를 사용하였다.
6장에서는 VAH12.csv와 VAH12_impedance.csv를 이용하여 배터리 등가회로 모델을 식별하고, 
확장 칼만 필터(EKF)로 충전상태(SOC)를 추정하였다.
7장에서는 충전곡선과 RPT에서 측정한 용량을 이용하여 열화상태(SOH) 추정모델을 구성하였다. 
이후 각 사이클의 충전 전압·전하량으로 SOH를 추정하고, 파티클 필터(PF)로 잔여수명을 예측하였다.

4. 폴더 내 파일의 구분

  VAH01.csv, VAH11.csv, VAH12.csv, VAH13.csv, VAH26.csv: 셀별 시험이력
  VAH01_impedance.csv, VAH11_impedance.csv, VAH12_impedance.csv: 셀별 내부저항
  README_original.txt: 원본 제공자의 시험조건·변수·데이터 이상 구간 안내

5. 다른 셀을 사용하는 방법

원본 사이트에서 원하는 VAH##.csv를 내려받아 같은 폴더에 넣고,
노트북의 test_cell을 해당 셀 이름으로 변경한다.
6장에서는 같은 셀의 VAH##_impedance.csv도 필요하다.
시험조건이 다른 셀에서는 분석 구간과 시험 구분 조건이 적절한지 확인한다.

