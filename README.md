# 3조 - 재비산먼지 분석을 통한 미세먼지 프리존 설립 방안 연구
---

## 프로젝트 내용
### 분석 배경 및 목적

### 데이터 획득


### 결론
#### 기대 효과
#### 한계점 및 추후 개선 방안

## Repository 구성
```
|__ data_preprocess: 데이터 수집 및 기본적인 처리 작업
     |__ bus_stop.ipynb: 동별 버스정류장수 수집 코드(위경도 -> 주소 by geopy)
     |__ data_preprocess.ipynb: API 활용 데이터 수집 및 CSV 통합 과정
|__ modeling: 메인 데이터셋 생성 및 eda, clustering, classification 과정
     |__ eda.ipynb: 데이터 기본적인 확인 및 시각화 코드
     |__ classification.ipynb: 재비산먼지 농도가 좋을지 나쁠지 분류하는 모델링 코드
     |__ clustering.ipynb: 동별 군집분석 코드
```
