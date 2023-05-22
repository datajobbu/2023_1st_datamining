# 3조 - 재비산먼지 분석을 통한 미세먼지 안심 버스정류장 설립 방안 연구
---

![title](https://github.com/datajobbu/2023_1st_datamining/assets/132864649/22b69640-475d-4071-a915-23a0bc91a767)

## 분석배경 <br>

### 재비산먼지란 ?
* 다시 날리는 먼지를 의미. 주로 도로 & 건설현상 인근에서 발생
* 특히 도로 위 재비산먼지는 차량이 움직이면서 또 날려 도로 근처 미세먼지 수준을 심각하게 높임 => 호흡기 질환 등 문제 발생 <br>

![redust_news](https://github.com/datajobbu/2023_1st_datamining/assets/132864649/974bc5be-72df-4933-b673-7413a815e381)
* 실제로도 여러 지자체별 재비산먼지의 심각성을 깨닫고 이를 줄이기 위해 노력중<br>
<br><br>
## 분석목적
1. Clustering : 서울시 행정동별 Clustering -> 미세먼지 안심 버스정류장 우선 설치지역 선정
2. Classification : 일별 행정동별 재비산먼지 농도 -> 측정하기 어려운 재비산먼지 농도의 좋고 나쁨을 측정하기 쉬운 변수들을 통해 예측
<br><br>


## 데이터 획득
1. 한국환경공단_도로 재비산먼지 측정 정보 : 3개 feature: 기온, 습도, 재비산먼지 평균농도
2. 서울시 일별 평균 대기오염도 정보 : 6개 feature : 이산화질소농도, 오존농도, 일산화탄소농도, 아황산가스, 미세먼지, 초미세먼지
3. 서울시 행정동별 버스 총 승차승객 정보 : 1개 feature : 버스 총 승차 승객 수 
4. 서울시 버스정류소 위치정보 : 1개 feature : 버스정류장 수
5. 서울시 건설공사 추진 현황 : 1개 feature : 건설공사 진행 수 
<br>
### 데이터 처리
![data1](https://github.com/datajobbu/2023_1st_datamining/assets/132864649/295f0b09-2122-4d0c-9529-3f2e9297a435)
* JOIN : 다른 데이터 소스에서 온 데이터들을 일, 구 기준으로 조인
* 전처리 : 조인하는 과정에서 필요한 전처리 진행
* 재비산먼지 : 여러개의 cvs파일을 한 파일로 합침. 서울 외의 지역 제외
* 버스정류소 : 서울시 버스정류서의 위도, 경도 -> 주소로 변경. 동별 버스정류장 수 집계
* 건설공사현황 : 현장 주소에서 구 정보를 가져오고, 특정 일자에 공사기간이 포함되면 집계 
<br><br>

## EDA
![EDA](https://github.com/datajobbu/2023_1st_datamining/assets/132864649/0b6c8cc7-037e-466d-a46e-693571726d28)
0 날짜 정보
1 서울시 행정동
2 온도
3 습도
4 재비산먼지
5 미세먼지
6 미세먼지
7 오존
8 이산화질소
9 일산화탄소
10 아황산가스 
11 버스이용인구
12 버스 정류장 수
13 건물 수

* 여러 데이터 소스의 전처리 후 조인하여 완성된 메인 데이터셋
* 조인과정에서 null처리 되어 결측치 없음
* 일별 동별 데이터 shape : (1140, 14)
<br>
* 데이터 통계 요약
![EDA2](https://github.com/datajobbu/2023_1st_datamining/assets/132864649/47c8da71-d897-4623-b14d-cb40556ca3c7)
<br>
![EDA3](https://github.com/datajobbu/2023_1st_datamining/assets/132864649/3175615a-7f02-48e7-91dd-8126afc343a8)
* 변수간 스케일 차이 큼
* Redust(재비산먼지), pm10(미세먼지)에서 outlier 확인
<br>

![EDA4](https://github.com/datajobbu/2023_1st_datamining/assets/132864649/4b8a655e-0692-414d-a74e-d74853924c63)
* 재비산먼지 데이터 히스토그램 시각화 
* 0~40사이의 값이 대부분





## clustering
* 서울시 행정동별 '재비산먼지', '유동인구(버스승차객수)', '버스정류장수'를 바탕으로 clustering -> 미세먼지 안심 버스정류장 우선 설치지역 설정
     * 실루엣 스코어와 엘보우 차트를 이용하여 최적의 군집 발견 -> 군집별 특징 분석하여 우선순위(ex. 1.유동인구 낮/재비산먼지 높 2. 유동인구 높/재비산먼지 높인 경우 2번 군집에 해당하는 동부터 설치)


### classification
* 일별 행정동별 재비산먼지 농도 Classification -> 측정하기 어려운 재비산먼지 농도의 좋고 나쁨을 측정하기 쉬운 변수들을 통해 에측
     * f1 score = 0.54
     * `temp`와 `hum`만 주요 변수로 작용하고 이 외의 변수들은 영향력 작음 -> 상관성 강한 변수의 부족


### 결론
* 행정동별 '재비산먼지', '유동인구(버스승차객수)', '버스정류장수'를 바탕으로 clustering하여 군집별 특징을 분석하여 미세먼지 안심 버스정류장 설립의 우선순위 제안
* 상관성 강한 변수의 부족으로 재비산먼지 농도 classification 문제는 성능 좋지 않음 -> 더 많은 재비산먼지 혹은 차량통행 데이터 등 다양한 데이터를 구한다면 성능 향상 기대됨


## Repository 구성
```
|__ data_preprocess: 데이터 수집 및 기본적인 처리 작업
     |__ bus_stop.ipynb: 동별 버스정류장수 수집 코드(위경도 -> 주소 by geopy)
     |__ data_preprocess.ipynb: API 활용 데이터 수집 및 CSV 통합 과정
|__ modeling: 메인 데이터셋 생성 및 eda, clustering, classification 과정
     |__ main_data_process.ipynb: `data_preprocess`를 통해 수집한 다양한 소스의 데이터를 합치는 코드
     |__ eda.ipynb: 데이터 기본적인 확인 및 시각화 코드
     |__ classification.ipynb: 재비산먼지 농도가 좋을지 나쁠지 분류하는 모델링 코드
     |__ clustering.ipynb: 동별 군집분석 코드
```


## 출처
* [한국환경공단_도로 재비산먼지 측정 정보](https://www.data.go.kr/data/15021888/fileData.do)
* [서울시 일별 평균 대기오염도 정보](http://data.seoul.go.kr/dataList/OA-2218/S/1/datasetView.do)
* [서울시 행정동별 버스 총 승차 승객수 정보](http://data.seoul.go.kr/dataList/OA-21225/S/1/datasetView.do)
* [서울시 버스정류소 위치정보](http://data.seoul.go.kr/dataList/OA-15067/S/1/datasetView.do)
* [서울시 건설공사 추진 현황](http://data.seoul.go.kr/dataList/OA-2540/S/1/datasetView.do)
