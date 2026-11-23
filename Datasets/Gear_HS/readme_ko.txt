HS Gear 데이터 사용 안내

1. 데이터 출처 및 측정 개요

이 데이터는 Eric Bechhoefer가 제공한 풍력발전기 고속 기어(High-Speed Gear)의
진동 데이터이다. 3 MW급 V90 풍력발전기에서 정상 상태와 피니언 기어의
자연 발생 결함 상태를 측정하였으며, 정상 6개와 고장 11개 파일로 구성된다.

원래 배포처는 data-acoustics.com이며 현재 해당 사이트에서 자료를 받을 수 없다.
한국항공대학교 연구실은 제공자의 허가를 받아 이 데이터를 재배포한다.

데이터 소개 및 다운로드:
https://www.kau-sdol.com/kaug

데이터 출처:
Eric Bechhoefer, High Speed Gear Dataset, Acoustics and Vibration Database (2014).

2. 배포 파일명과 데이터 구성

연구실에서 배포하는 HS_gear.zip을 해제하면 다음 폴더와 파일이 있다.

  data1/case_1.mat ~ case_11.mat: 고장 상태의 측정 데이터
  data2/case_1.mat ~ case_6.mat: 정상 상태의 측정 데이터

진동신호와 측정조건은 MATLAB MAT 형식으로 제공된다.
각 파일에는 다음 변수가 들어 있다.

  gs: 진동 가속도 신호
  tach: 회전 동기화를 위한 타코미터 펄스 발생시각 배열
  sr: 샘플링 주파수, 97,656 Hz
  ppr: 회전당 타코미터 펄스 수, 8
  teeth: 피니언 기어 잇수, 32

각 gs 배열은 585,936행, 1열이며 약 6초간 측정한 진동신호이다.
축 회전 주파수는 약 30 Hz이며 기어 맞물림 주파수는 960 Hz이다.

gs와 tach는 별도의 배열이며 원소 수가 같을 필요가 없다.
gs의 각 원소는 진동 샘플이고, tach의 각 원소는 펄스 발생시각이다.
파일의 case 번호는 측정 데이터의 식별번호이며 결함 심각도 수준을 뜻하지 않는다.

3. 교재에서의 활용

교재에서는 배포본의 data1과 data2 폴더를 각각 Fault와 Normal로 구분하였다.
모든 파일에서 동일한 sr, ppr, teeth는 공통 info.mat에 저장하고,
각 case MAT 파일에는 gs와 tach만 저장하였다.

2장: Fault/case_1.mat을 이용하여 시간 동기 평균(TSA) 등 기어 신호처리를 수행한다.
3장: 정상 6개와 고장 11개 파일에서 특징을 추출하고 정상·고장 구분 성능을 비교한다.

4. 폴더 내 파일의 구분

  Fault/case_1.mat ~ case_11.mat: 고장 상태의 gs와 tach 배열
  Normal/case_1.mat ~ case_6.mat: 정상 상태의 gs와 tach 배열
  info.mat: 공통 측정조건 sr, ppr, teeth

info.mat은 Gear_HS 폴더에 두며, Fault와 Normal의 모든 파일에 공통으로 사용한다.

