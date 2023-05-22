# 3조 - 재비산먼지 분석을 통한 미세먼지 안심 버스정류장 설립 방안 연구
---

![title](https://github.com/datajobbu/2023_1st_datamining/assets/132864649/22b69640-475d-4071-a915-23a0bc91a767)

## 분석배경 <br>

### 재비산먼지란 ?
![what_is_redust](https://github.com/datajobbu/2023_1st_datamining/assets/132864649/9dd3008d-8cbb-4f76-a249-fec4fb8f818e)<br>
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
<br>

* JOIN : 다른 데이터 소스에서 온 데이터들을 일, 구 기준으로 조인
* 전처리 : 조인하는 과정에서 필요한 전처리 진행
* 재비산먼지 : 여러개의 cvs파일을 한 파일로 합침. 서울 외의 지역 제외
* 버스정류소 : 서울시 버스정류서의 위도, 경도 -> 주소로 변경. 동별 버스정류장 수 집계
* 건설공사현황 : 현장 주소에서 구 정보를 가져오고, 특정 일자에 공사기간이 포함되면 집계 
<br><br>

## EDA
![EDA](https://github.com/datajobbu/2023_1st_datamining/assets/132864649/0b6c8cc7-037e-466d-a46e-693571726d28) <br>
0 날짜 정보 <br>
1 서울시 행정동<br>
2 온도<br>
3 습도<br>
4 재비산먼지<br>
5 미세먼지<br>
6 미세먼지<br>
7 오존<br>
8 이산화질소<br>
9 일산화탄소<br>
10 아황산가스 <br>
11 버스이용인구<br>
12 버스 정류장 수<br>
13 건물 수<br>

* 여러 데이터 소스의 전처리 후 조인하여 완성된 메인 데이터셋
* 조인과정에서 null처리 되어 결측치 없음
* 일별 동별 데이터 shape : (1140, 14)
<br>

### 데이터 통계 요약

![EDA2](https://github.com/datajobbu/2023_1st_datamining/assets/132864649/47c8da71-d897-4623-b14d-cb40556ca3c7)<br>

<br><br> ![EDA3](https://github.com/datajobbu/2023_1st_datamining/assets/132864649/3175615a-7f02-48e7-91dd-8126afc343a8)<br>

* 변수간 스케일 차이 큼
* Redust(재비산먼지), pm10(미세먼지)에서 outlier 확인
<br>

![EDA4](https://github.com/datajobbu/2023_1st_datamining/assets/132864649/4b8a655e-0692-414d-a74e-d74853924c63)
* 재비산먼지 데이터 히스토그램 시각화 
* 0~40사이의 값이 대부분<br><br><br>


## Clustering
* 클러스터링에 사용한 변수
    * (1) 재비산먼지 농도
    * (2) 버스 정류장 수
    * (3) 버스 승객 수     
<br>

* K-Means 클러스터링 진행 
    * 전처리로 로그변환 & scaling 진행
    * 실루엣스코어 & 엘보우차트 조합 결과 6개의 군집이 가장 잘 나타낸다고 판단 

![clustering2](https://github.com/datajobbu/2023_1st_datamining/assets/132864649/826ee06f-1a52-4915-91f8-2d3f35b87911)
<br>
* 유동인구는 비슷하지만 재비산먼지 농도는 큰 차이가 나는 0번, 5번 클러스터를 보았을 때 군집별로 특성이 잘 나타난다고 볼 수 있음
<br>

### Clustering 결과 시각화

![clustering3](https://github.com/datajobbu/2023_1st_datamining/assets/132864649/2e12ea9f-8fde-4d45-9621-b60df453089c)
<br>

## Classification
* 재비산먼지의 정확한 농도 파악 목적이 아닌, 농도 좋고/나쁨의 분류문제로 정의
* 앞서 EDA -> 0~40 사이가 대부분인 것을 확인, 나머지는 비슷한 수준
    * 40을 기준으로 0/1 구분
    
* `temp`와 `hum`만 주요 변수로 작용하고 이 외의 변수들은 영향력 작음 -> 상관성 강한 변수의 부족(한계)

* Imbalanced 문제 -> Accuracy가 아닌 F1-score 활용
* 3가지 알고리즘 각각에 대해 Hyperparameter 튜닝을 하여 성능 확인
* Validation F1-score 기준 가장 성능 좋은 RandomForestClassifier 사용
* Hyperparameter : {‘n_estimators’: 300, ‘max_depth’: 9} 사용

### Classification 결과

![class1](https://github.com/datajobbu/2023_1st_datamining/assets/132864649/7375ccff-7a34-424d-bc43-f8534a490acc)
<br>
* F1_score 0.53 -> 성능이 좋지는 않음
* 주요 변수로는 온도,습도, 건설현장수, 버스정류장수, 버스승차객수가 나옴<br>
* 분류결과 변수별 평균값 확인
    * 분류결과를 붙여 그룹별로 확인 -> 대부분 온도와 습도에 의해 0, 1 분류를 했음을 알 수 있음
    * 유의미한 변수가 적어 성능이 좋지 않은 것으로 추정됨
<br>

## 결론
* 미세먼지 안심 머스 정류장 설치 고려 우선순위
    * 1순위 High Redust
    * 2순위 High Buspop
    * 3순위 High Busstop
<br>

* 0번 그룹 - 재비산먼지 낮음 / 유동인구 많음 / 버스정류장 많음
* 1번 그룹 - 재비산먼지 보통 / 유동인구 적음 / 버스정류장 적음
* 2번 그룹 - 재비산먼지 보통 / 유동인구 보통 / 버스정류장 보통
* 3번 그룹 - 재비산먼지 낮음 / 유동인구 적음 / 버스정류장 적음
* 4번 그룹 - 재비산먼지 낮음 / 유동인구 보통 / 버스정류장 적음
* 5번 그룹 - 재비산먼지 높음 / 유동인구 많음 / 버스정류장 많음
 <br>
 
 <순위>
* 1순위 5번그룹<br>
* 2순위 2번그룹<br>
* 3순위 0번 그룹 (재비산먼지는 낮지만, 유동인구(=누릴 수 있는 사람)가 많기 때문)<br>
* 4순위 1번 그룹<br>
* 5순위 4번 그룹<br>
* 6순위 3번 그룹<br>

### 설치 1순위 분석

* 1순위 : 5번 그룹에 속한 동 : 재비산먼지 높음 / 유동인구 많음 / 버스정류장 많음
   * 가산동, 개봉동, 공릉동 외 14개 동<br>
   * 주로 주거지역이라는 특징을 보임<br>
   * 서울시 행정동 중 아래 14개 동에 미세먼지 안심 머스정류장 우선 설치 필요  
   * ![con1](https://github.com/datajobbu/2023_1st_datamining/assets/132864649/4367b079-892b-4a41-9a8c-a5a7024c4e99)
<br>
<br><br><br>

## 한계점
### 01 데이터 부족
* 상관성 강한 데이터 부족 -> 차량 통행 수 등의 데이터 구하기에 어려움
* 재비산먼지 측정이 어려운 문제로 인해 데이터가 적음 
* 매일 측정이 불가하여 특정기간만의 데이터가 있음
<br>

### 02 Classification 성능 낮음
* 데이터 수 자체와 상관있는 변수의 부족
* 위에서 언급한 구하기 어려운 데이터(재비산먼지, 차량통행 등)를 구한다면 성능 개선 가능성 있음

<br><br><br>

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
