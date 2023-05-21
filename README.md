# 3조 - 재비산먼지 분석을 통한 미세먼지 프리존 설립 방안 연구
---

## 프로젝트 내용
* 서울시 행정동별 '재비산먼지', '유동인구(버스승차객수)', '버스정류장수'를 바탕으로 clustering -> 미세먼지 안심 버스정류장 우선 설치지역 설정
     * 실루엣 스코어와 엘보우 차트를 이용하여 최적의 군집 발견 -> 군집별 특징 분석하여 우선순위(ex. 1.유동인구 낮/재비산먼지 높 2. 유동인구 높/재비산먼지 높)인 경우 2번 군집에 해당하는 동부터 설치
* 일별 행정동별 재비산먼지 농도 Classification -> 측정하기 어려운 재비산먼지 농도의 좋고 나쁨을 측정하기 쉬운 변수들을 통해 에측
     * f1 score = 0.54
     * `temp`와 `hum`만 주요 변수로 작용하고 이 외의 변수들은 영향력 작음 -> 상관성 강한 변수의 부족


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
