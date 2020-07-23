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

5. jeju_creditcard_mean2019_m03m01_product_m03m01
    - LB(Public) : 2.9918479873
    - "non_COV 4월 총 AMT" = 19년 전체 평균
    - "COV_ratio" = (20년2월AMT / 20년1월AMT) * (20년3월AMT / 20년1월AMT)
    - log 적용 ->  "non_COV 4월 총 AMT" * "COV_ratio" -> exp 적용

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