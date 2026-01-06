## 📁 데이터 수집 및 전처리 코드 목록

### 공간 정보 처리
- station_nearest_3parks_vector.ipynb : 위·경도 기반 측정소–도시숲 간 거리 및 방향(sin, cos) 계산 코드
- station_nearest_3parks_vector.csv : 각 측정소 기준 최근접 3개 도시숲의 거리·방향·면적 벡터 데이터

### 미세먼지 데이터 수집
- air_quality_api_save.ipynb : 서울시 대기환경정보 API 호출 후 PM10 데이터 CSV 저장 코드

### 데이터 병합
- merge_spatial_airquality.ipynb : 도시숲 공간 데이터와 미세먼지 관측 데이터를 병합하여 단일 CSV 생성 코드
- final_merge.ipynb : 공간·미세먼지·기상·교통 데이터를 통합 병합하는 최종 데이터셋 생성 코드

### 기상 데이터 처리
- asos_api_collect.ipynb : 기상청 ASOS API 호출을 통한 기상 데이터 수집 코드
- asos_gu_merge.ipynb : 기상청 관측 데이터를 서울시 자치구 단위로 매핑 및 병합하는 코드
- batch_weather.ipynb : 서울시 전역 일괄 기상 데이터 생성 코드

### 교통량 데이터
- traffic_data_collect.ipynb : 교통량 데이터 전처리 및 정리 코드
- traffic_merge.ipynb : 교통량 데이터를 기존 환경 데이터와 병합하는 코드

### 가상 도시숲 시뮬레이션
- virtual_urban_forest.ipynb : 가상 도시숲 생성 및 입지·규모 변화 시나리오 분석 코드

### 추가 환경 변수 결합
- batch_weather_traffic.ipynb : 일괄 기상 데이터와 교통량 데이터 병합 코드
- batch_weather_ghg.ipynb : 일괄 기상 데이터와 온실가스 데이터 병합 코드

