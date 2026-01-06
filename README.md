# Urban-Forests-and-Fine-Dust-Mitigation
2025/데이터사이언스 팀프로젝트

1. 과제 개요
도시숲의 위치 면적 거리 등 다양한 공간 정보를 활용한 AI 미세먼지 예측 모델을 개발하여 
도시숲에 의한 미세먼지 저감효과를 정량적으로 분석하고 단순 면적 확대를 넘어 저감효과를 극대화할 수 있는 도시숲의 최적 입지와 규모를 과학적으로 도출하고자 함.

2. 활용데이터
1) 도시숲공간정보: 서울시공공데이터 (운영기관 서울특별시청)
형식: CSV /GeoJSON
내용: 도시숲명,위경도,면적등
수집방안: 크롤링하여 자체 데이터로 구축

2) 미세먼지관측데이터: API 서울시 대기환경정보
형식: CSV (시간단위시계열)
내용 수치 : PM10
해상도 시간 단위 : 1
수집방안 호출 : API

3) 미세먼지관측소위경도: 서울시대기환경정보
형식: text
내용 서울시 구별 미세먼지 관측소의 위도, 경도

4) 종관기상관측(ASOS): API 기상청 허브
형식: CSV (시간단위시계열)
내용: 서울시 풍향, 풍량, 지면기압, 기온, 습도, 강수량 
해상도 시간 단위 : 1
수집방안 호출 : API

3. 활용도구
1) Python (Colab) 환경
도구:Pandas,NumPy,Scikit-learn,XGBoost,Matplotlib,Geopandas
버전 최신환경 :Python 3.10 /Colab
비용: 유료(100 컴퓨팅단위 구매)
제작자:Google (Colab), 오픈소스커뮤니티 패키지


4. 분석 내용
1) 전처리 및 탐색적 데이터분석
  1-1) 위경도기반 격자변환 및 좌표계 통일
     도시숲의 중심과 자치구별 측정소 위경도좌표를 서울시 면적 기준 픽셀좌표 변환 / 서울특별시 위경도 경계 기준값 [위도: 37.413, 37.715] [경도: 126.734, 127.269]
  1-2) 각 측정소와 가장 가까운 3개의 도시숲에 대하여 거리와 방향 정보 계산
     거리: 픽셀 간 유클리드 거리 방식으로 계산
     방향: 픽셀간의 sin,cos 값으로 방향 정보 도출
     거리감쇠 함수를 이용하여 가중치를 계산한 후 가중평균을 통해 거리에 따른 도시숲 면적과 거리의 영향을 계산
  1-3) 기상정보 및 미세먼지 데이터 결합
     2024년도의 주 4일(평일 3일, 주말 1일) 1시간 간격으로 풍향, 풍량, 지면기압, 기온, 습도, 강수량 데이터 수집 및 각 구별 측정소 도시숲 데이터와 결합
     기상 데이터는 서울시 전역 일괄데이터 이용
     미세먼지 데이터는 각 자치구별 측정 데이터 이용
  1-4) 날짜 및 시간 파생 데이터
     타임스탬프에서 분리하여 월, 요일 변수 생성
     계절 변수 생성(봄:3,4,5월 여름:6,7,8월 가을:9,10,11월 겨울:12,1,2월)

2) 모델학습 및 예측
   2-1) 모델: Stacking Ensemble Regressor
   2-2) 베이스모델:
     XGBoostRegressor
    파라미터:n_estimators=200,learning_rate=0.05,max_depth=5
     CatBoostRegressor
    파라미터:learning_rate=0.05,depth=5,iterations=200,verbose=0
     RandomForestRegressor
    파라미터:n_estimators=200,max_depth=10
     MLPRegressor
    파라미터: hidden_layer_sizes=(64, 32, 16), activation='relu', solver='adam', learning_rate_init=0.005, max_iter=200, batch_size=64
   2-3) Meta모델
     XGBoostRegressor
     파라미터:learning_rate=0.05,n_estimators=100
   2-4) 스태킹 방식
     Scikit-learn의 Stacking Regressor를 활용하여 다중모델 예측 결과 종합
     각 모델은 병렬학습 되고 최종 결과는 메타모델(XGBoost) 을 통해 예측
     다양한 모델 특성을 융합함으로써 PM10 예측정확도와 안정성을  동시에 확보
