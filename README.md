# 2020_dacon_jeju_creditcard
2020데이콘 제주 신용카드 빅데이터  경진대회 참가를 위한 repository

## 1. 학습한 모델
<jeju_creditcard_pred20Y04M_COVratio.ipynb>

1. jeju_creditcard_M1M4ratio_COVratio.ipynb (Baseline)
    - LB(Public) 3.1570802218
    - "non_COV 4월 총 AMT" = 19년4월AMT / 20년1월AMT * 20년1월AMT
    - "COV_ratio" = 평균(20년2월AMT / 20년1월AMT, 20년3월AMT / 20년1월AMT)
    - log 적용 ->  "non_COV 4월 총 AMT" * "COV_ratio" -> exp 적용
    
2. jeju_creditcard_19Y04M_COVratio.ipynb
    - LB(Public) 3.0896265684
    - "non_COV 4월 총 AMT" = 19년4월AMT
    - "COV_ratio" = 평균(20년2월AMT / 20년1월AMT, 20년3월AMT / 20년1월AMT)
    - log 적용 ->  "non_COV 4월 총 AMT" * "COV_ratio" -> exp 적용

## 2. jeju_creditcard_EDA.ipynb
   
