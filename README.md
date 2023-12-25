# 송파구 일식집 추천시스템 개발 프로젝트 (동덕여대 정보통계학과 캡스톤 디자인 실습, 2022-2학기, A+)
## Summary
### 네이버 리뷰정보를 통한 식당 추천 / 컨텐츠 기반
- 1차 

- 1. 음식점 이름
- 2. 위치(동)
- 3. 분류 (일식 한정 / 베스트 메뉴 기준)

     - 초밥

     - 밥류 (덮밥, 카레, 텐동 등)

     - 라멘

     - 우동, 소바

     - 가스류

     - 사시미

 - 4.  가격 

 - 5.  별점 (5점 만점)


- 2차 

 6.  맛
 7. 가성비
 8. 양
 9. 서비스
- 3차 

 10. 네이버 지도 카테고리 ALL
- 4차 

 11. 데이터랩
- 추천시스템

- 1. 기본 데이터셋을 통한 코사인 유사도

-  2. 가중치 넣은 데이터셋을 통한 코사인 유사도 

- → 코사인 유사도 계산 후 NDCG 평가
### 협업필터링
- Data

- 행 : 초밥으로 뽑은 식당 

- 열 : user

- 값 : 가중치 적용 rating 값

- 추천시스템

- 경사하강법 이용한 협업 필터링 추천 리스트

## Processing
<aside>
💡 공유 드라이브

- 코드 및 데이터셋 공유용
https://drive.google.com/drive/u/0/folders/18c9lfUHzurhkHI0cQINRkQ9xyuBx2rnE

- NDCG 로 결과 평가  
- : abs(가게의 가격 - 타켓의 가격) 의 역수 * 별점(소수 첫번째 자리만 살린 것)

### ▶️  컨텐츠 1차 과제

- 가격대와 별점(수치) 더해서 코사인 유사도 구하기

[[Machine Learning][머신러닝] 데이터 전처리(범주형/연속형)](https://ysyblog.tistory.com/71)

---

발생문제

1. 결과로 나온 시스템들이 제대로 추천이 안된 거 같다는 생각 
→ 성능 지표를 만들어서 결과 비교할 필요 있음 :  NDCG 적용
2. 가중치 식을 1/abs(추천대상 가격 - 비교대상 가격) * steamRating 으로 두었을 때, 
비교대상 가격과 같은 추천항목들이 별점(steamRating) 고려 없이 가중치가 0이 되면서 
상위에 랭크됨 
→ 회전초밥처럼 기존 값들과 너무 차이나는 가격을 갖는 값 삭제 및 가격에 log 적용

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/967ba761-8b8a-4188-943c-ca52d45d7592/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4ca890c7-4a73-4625-92c3-621ae35d32af/Untitled.png)

---

---

### ▶️  컨텐츠 2차 과제

- 별점으로 SORTING
- 맛 /  가성비 / 양 / 서비스 점수화 된 데이터셋 구성
- 맛 / 가성비 / 양 / 서비스 - 별점과의 상관관계로 가중치 부여 후 가중치로 SORTING

[[Python Data Analysis 분석 4] 데이터 분석 - 파이썬 상관관계 분석(타이타닉)](https://tjansry354.tistory.com/9)

---

발생문제

1. relevant_value 관련 이슈- 로그화 여부에 따라 추천항목이 다르게 나옴, 
로그화 했을때 NDCG- 0.391/ 로그화 하지 않았을때 NDCG- 0.34 나옴
→  NDCG 높은 로그화 relevant_value 식 채택

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1c108d8c-4869-4ed1-9b66-0bc0dabb9197/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/16d9fc68-33f1-4e1a-bc31-b43016b764fb/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/659d22e9-27df-410d-9a2d-2f3ba23f1d98/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/15e9982e-10e2-4974-bee4-9d1edc28f3b8/Untitled.png)

1. 피처 수가 작은데 그 안에서 relevant_value를 정하려 하니 추천시스템에 사용했던 피처가 
NDCG 평가기준이 되어 이중 고려가 되는 상황 발생 : NDCG가 독자적인 평가 기준이 될 수 없음

      → 네이버 지도 카테고리를 모두 사용하는 것으로 피처 수를 늘리고 그것으로 추천시스템 개발 / 
          relevant_value는 별점과 가격으로 고려 (별점과 피처가 연결되어 있다)

---

---

### ▶️  컨텐츠 3차 과제

- 데이터 항목 데이터셋 화 ( 네이버 영수증 카테고리)
- 1. 기본 데이터셋을 통한 코사인 유사도
2. PCA로 차원 축소한 데이터셋을 통한 코사인 유사도
3. 가중치 넣은 데이터셋을 통한 코사인 유사도 
→ 코사인 유사도 계산 후 NDCG 평가

✔️  가중치 부여 방식이 NDCG 값 가장 높음

---

---

### ▶️  협업 1차 과제

- 경사하강법 적용하여 matrix 생성
- 생성한 matrix로 추천리스트 생성
