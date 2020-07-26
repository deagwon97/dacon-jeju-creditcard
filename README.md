# 2020_dacon_jeju_creditcard
2020데이콘 제주 신용카드 빅데이터  경진대회 참가를 위한 repository

## 1. 학습한 모델
* <jeju_creditcard_pred20Y04M_COVratio.ipynb>

1. jeju_creditcard_M1M4ratio_COVratio.ipynb (Baseline)
    - LB(Public) : 3.1570802218
    - "non_COV 4월 총 AMT" = 19년4월AMT / 20년1월AMT * 20년1월AMT
    - "COV_ratio" = 평균(20년2월AMT / 19년2월AMT, 20년3월AMT / 19년3월AMT)
    - log 적용 ->  "non_COV 4월 총 AMT" * "COV_ratio" -> exp 적용
    
2. jeju_creditcard_19Y04M_COVratio.ipynb
    - LB(Public) : 3.0896265684
    - "non_COV 4월 총 AMT" = 19년4월AMT
    - "COV_ratio" = 평균(20년2월AMT / 19년2월AMT, 20년3월AMT / 19년3월AMT)
    - log 적용 ->  "non_COV 4월 총 AMT" * "COV_ratio" -> exp 적용

3. jeju_creditcard_19Y04M_20Y02Md20Y01M.ipynb
    - LB(Public) : ??
    - "non_COV 4월 총 AMT" = 19년4월AMT
    - "COV_ratio" = (20년2월AMT / 20년1월AMT)
    - log 적용 ->  "non_COV 4월 총 AMT" * "COV_ratio" -> exp 적용

4. jeju_creditcard_19Y04M_m03m01_product_m03m01.ipynb
    - LB(Public) : 3.0755254587
    - "non_COV 4월 총 AMT" = 19년4월AMT
    - "COV_ratio" = (20년2월AMT / 20년1월AMT) * (20년3월AMT / 20년1월AMT)
    - log 적용 ->  "non_COV 4월 총 AMT" * "COV_ratio" -> exp 적용

5. jeju_creditcard_mean2019_m03m01_product_m03m01.ipynb
    - LB(Public) : 2.9918479873
    - "non_COV 4월 총 AMT" = 19년 전체 평균
    - "COV_ratio" = (20년2월AMT / 20년1월AMT) * (20년3월AMT / 20년1월AMT)
    - log 적용 ->  "non_COV 4월 총 AMT" * "COV_ratio" -> exp 적용

6. jeju_creditcard_mean2019_m03m01_product_m03m01_zero.ipynb
    - LB(Public) : 1.7309402903
    - "non_COV 4월 총 AMT" = 19년 전체 평균
    - "COV_ratio" = (20년2월AMT / 20년1월AMT) * (20년3월AMT / 20년1월AMT)
        -"COV_ratio" : 20년1월AMT, 20년2월AMT, 20년3월AMT 중 하나라도 없는 경우 - > 동일 지역 COV_ratio평균
    - log 적용 ->  "non_COV 4월 총 AMT" * "COV_ratio" -> exp 적용
    - 3월 데이터가 없는 경우 -> "0으로"


7. ??
    - LB(Public) : 1.7309402903
    - "non_COV 4월 총 AMT" = 19년 전체 평균
    - "COV_ratio" = (20년2월AMT / 20년1월AMT) * (20년3월AMT / 20년1월AMT)
        -"COV_ratio" : 20년1월AMT, 20년2월AMT, 20년3월AMT 중 하나라도 없는 경우 - > 동일 지역 COV_ratio평균
    - log 적용 ->  "non_COV 4월 총 AMT" * "COV_ratio" -> exp 적용
    - 3월 데이터가 없는 경우 -> "0으로"
    
    - std가 0.3보다 큰 그룹은 다시 처리
        - 모든 월별 데이터가 없는 경우 -> 평소에도 매출이 많지 않다고 판단, 코로나 영향으로 20년 매출은 매우 적게 잡힐 것이다. -> 모두 0 으로 채우기
        - 모든 월별 데이터가 있지만 std가 0.3보다 큰 경우 -> 


7. 다음 시도 할 내용
    - jeju_creditcard_mean2019_m03m01_product_m03m01_zero.ipynb파일 마지막에 등장하는 특이 그룹 정리
    - 규제가 큰 회귀모델을 이용하여 시계열을 예측하고 rmse or std or p-value검정으로 예측이 잘 안되는 그룹 분류
        - cf) p-value로 할 경우 분류가 힘듬.
    - 분류에 따라 다른 "non_COV 4월 총 AMT" 말고 다른 값을 사용해볼 것.

## 2. jeju_creditcard_EDA.ipynb


## 기타 메모:
### 코로나 신규 확진자 추이
    - 3월 1일 : 1062명 (본격적인 코로나 검사 시작, 신천지 31번 확진자 관련)
    - 3월 14일 : 76명 (강력한 사회적 거리두기 시행, 매우 높은 참여율, 개학 연기)
        - 3월 1일 ~ 3월 14일 : 큰 폭으로 감소
    - 4월 3일 : 94명
        - 3월 14일 ~ 4월 3일 : 70 ~ 100명을 유지
    - 5월 5일 : 2명
        - 4월 3일 ~ 5월 5일 : 지속적으로 감소
    - 5월 9일 : 34명 (이태원 클럽)
    - 5월 23일 : 25명 
        - 5월 9일 ~ 5월 23일 : 30~20명을 유지
        - 5월 23일 ~ 7월 22일 : 30 ~ 60명 사이를 유지


* 코로나 신규 확진자 수가 유지된다고 하더라도, 사회 분위기에 따라서 업종별 매출 변화가 생긴다.
* 지역, 업종 별로 적절한 코로나 변수를 찾아야한다.
    - 7월은 2번(신천지, 이태원)의 큰 유행과, 지속되는 사회적 거리두기로 사람들의 피로가 쌓여있다는 점을 고려.
    - 
    - 휴가 시즌으로 관광, 숙박업의 매출이 증가 한다는 점 고려 .
        -> 코로나에 둔감해진 사람들은 관광, 숙박업을 이용한다는 점 고려.
* [가설] 7월은 5월 이태원 발 2차 파동이 발생한 후 2달이 지났다. 단순히 코로나 변수만 고려하면, 3월 신천지 발 대유행이이후 1달이 지난 4월 말의 상황과 유사할 것이다.


* 아래 링크를 참고하면, 대부분의 업종에서 4월과 5월의 매출이 비슷하다는 사실을 알 수 있다.
https://dacon.io/competitions/official/235618/codeshare/1382?page=2&dtype=recent&ptype=pub

