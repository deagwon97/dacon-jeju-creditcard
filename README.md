# 2020_dacon_jeju_creditcard
2020데이콘 제주 신용카드 빅데이터  경진대회 참가를 위한 repository


# 진행 과정
 baseline코드, 혹은 다른 시계열 예측 모델을 사용하여 여러가지 시도를 해보았지만, 큰 성과가 없었다.  
이때, '제루스챈스'님이 공유한 아이디어를 보고, 큰 충격을 받았다. 3월 데이터를 그대로 예측에 사용할 경우 스코어가 가장 높게 나왔다.

 하지만 public score는 4월의 매출로 평가한 점수이다. 이는 당연히 3월의 매출과 가장 유사할 것이다. 우리는 7월의 매출을 예측해야 하므로 단순히 3월을 사용하면 안된다.

 여기서  2020년과 2019년을 분리해서 생각해 보았다. 2020년이 2019년과 다른 점은 '코로나-19'라는 질병이 창궐했다는 사실이다. '코로나-19'로 인해서 어떤 그룹은 매출이 크게 감소하였지만, 어떤 업종은 큰 변화가 없었다.

 그렇다면 코로나라는 질병이 없었을 때의 20년 7월 매출에 코로나의 영향을 곱해준다면 실제 7월의 매출과 비슷하지 않을까 라는 생각을 하게되었다.

 이제 우리는 코로나가 없었을 때의 7월 매출인 non_cov_07과 이때의 코로나 영향력인 cov_ratio를 찾는 것으로 목표를 설정하였다.

 당시(4월데이터 공개 전)에는 4월 데이터는 pulblic score로 하루에 3번만 채점이 가능했기 때문에 우선적으로 non_cov_04와 4월의 cov_ratio를 찾아보았다.



## 1. Model history
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


7. jeju_creditcard_mean2019_13상대적변화율제곱_log_submission_zero.ipynb
    - LB(Public) : 1.6228390603
    - "non_COV 4월 총 AMT" = 19년 전체 평균
    - "COV_ratio" = (20년3월AMT / 20년2월AMT) / (20년3월AMT / 20년2월AMT) <-  작년 변동률 대비 올해 변동률
        -"COV_ratio" : 20년1월AMT, 20년2월AMT, 20년3월AMT 중 하나라도 없는 경우 - > 동일 지역 COV_ratio평균
        - 작년에는 매출이 증가했던 그룹이 올해는 감소했다면, 코로나의 영향을 많이 받았다고 볼 수 있다.  
        - 변화율을 더 크게 줄 경우 정확 도가 올라갈 것으로 예상했지만 COV_ratio를 제곱하여 곱했을때, Public score는 1.6236752818으로 감소함.
    - log 적용 ->  "non_COV 4월 총 AMT" * "COV_ratio" -> exp 적용
    - 3월 데이터가 없는 경우 -> "0으로"

8. ??
    - LB(Public) : ??
    - "non_COV 4월 총 AMT"
    - "COV_ratio" = (20년3월AMT / 20년2월AMT) / (20년3월AMT / 20년2월AMT) <-  작년 변동률 대비 올해 변동률
        -"COV_ratio" : 20년1월AMT, 20년2월AMT, 20년3월AMT 중 하나라도 없는 경우 - > 동일 지역 COV_ratio평균
    - log 적용 ->  "non_COV 4월 총 AMT" * "COV_ratio" -> exp 적용
    - 3월 데이터가 없는 경우 -> "0으로"
    - std가 0.3보다 큰 그룹은 다시 처리
        - 모든 월별 데이터가 없는 경우 -> 평소에도 매출이 많지 않다고 판단, 코로나 영향으로 20년 매출은 매우 적게 잡힐 것이다. -> 모두 0 으로 채우기
        - 모든 월별 데이터가 있지만 std가 0.3보다 큰 경우 -> (특정 처리)


9. 다음 시도 할 내용
    - jeju_creditcard_mean2019_m03m01_product_m03m01_zero.ipynb파일 마지막에 등장하는 특이 그룹 정리
    - 규제가 큰 회귀모델을 이용하여 예측하고 rmse or std or p-value검정으로 예측이 잘 안되는 그룹 분류
        - cf) p-value로 할 경우 분류가 힘듦
    - 분류에 따라 다른 "non_COV 4월 총 AMT" 말고 다른 값을 사용해볼 것.


10. 여러가지 기준으로 그룹들을 분리하고 non_COV_7월과 cov_ratio를 각각 다르게 계산한다.  
    위의 방법들을 바탕으로 그룹들을 묶는 기준을 다음과 같이 4가지로 선정하였다.  

    1. std(그룹내 기간별 표준편차)
        std가 0.15보다 작은 그룹이 많이 존재한다. 이러한 그룹들은 non_cov_7을 예측할 때 전체 기간 평균을 사용했다.
    2. 새로운 변수 p
        p가 0.5보다 큰 그룹이 꽤 많이 존재한다.(이는 한 row에 이용건수가 3건 이하인 경우가 50%를 넘는 경우이다.  
        p가 0.5보다 큰 그룹과 작은 그룹에 대하여 다른 방식으로 non_cov와 cov_ratio를 구하였다.  
    3. 선형성(r2score)
        r2score가 0.5를 넘는 경우, 기간에 따라 계속 성장하거나,  
        하락하는 그룹이라고 판단하여 가장 최근의 데이터를 위주로 예측에 사용하였다.
    4. 4차함수의 fitting결과(rmse, 최대 최소 차이 대비 처음-끝 차이)
        2019년 1월 부터 2020년 1월 사이에는 계절성을 갖는 그룹들이 있었다.(겨울에 감소했다가 여름에 증가 등)
        이 형태가 4차 함수와 유사하였다. 따라서 4차 다항함수를 fitting했을 때 오차가 크지 않고,
        19년 1월과 20년 1월의 차이가 전체 기간 중 최대값 최솟값 차이보다 충분히 작을 경우  
        계절성을 갖는 그룹으로 분류하였다.

    다음의 수도코드에서 이를 정리했다.  


## Idea
### 새로운 변수 p (그룹내 이용객 n명이하인 row수/그룹 전체 row수)로 그룹에 변동성을 부여.
    - p = (그룹내 이용객 n명이하인 row수/그룹 전체 row수)  
    1. 한 그룹의 p가 크다면 대부분의 row에서  이용객이 n명 이하라는 뜻  
    2. p가 크다면 시기별 데이터 변동이 커질 것 이다.  
        - 고객층이 충분히 많지 않아서 데이터 변동이 커진다.
    3. 이 p값을 이용하여 변동이 큰 집단(A)과 그렇지 않은 집단(B)(그룹의 묶음을 집단이라 하자.)을 나눈다.  
    4. 집단(A)와 집단(B)에서도 각각 규칙성, 불규칙성이 있는 그룹들이 있다.  
    5. 집단(A)에서 규칙성을 갖은 경우와 집단(B)에서 규칙성을 갖는 경우에 대하여 다른 처리를 한다.  
## Find Pattern
#### Pseudocode
```python
        if (19년 데이터 + 20년 1월) 전부 존재:
            (19년 데이터 + 20년 1월)만 가지고 std를 계산
            if std > 0.15: # error_group
                if p < 0.5:  ## 변동성이 적다.

                    if 변화가 선형적이다: .........1)
                        cov_ratio_07 = (1 + 3 * mean(20Y02M, 20Y03M, 20Y04M) / mean(19Y07M, 19Y08M, 19Y09M)) / 4
                        cov_ratio_04 = mean(20Y02M, 20Y03M) / mean(19Y07M, 19Y08M, 19Y09M)
                        
                        non_cov_07 = mean(19Y11M, 19Y12M, 20Y01M)
                        non_cov_04 = mean(19Y11M, 19Y12M, 20Y01M)
                        
                        예측 7월 = non_cov_07 * cov_ratio_07
                        예측 4월 = non_cov_04 * cov_ratio_04
                    
                    else 변화가 비선형적이다:
                    
                        if 계절성을 갖는다.: .........2)
                            # 다항회귀 모델 예측후 rmse 가 0.3보다 작고, diff_per가 30% 미만일 경우
                            cov_ratio_07 = (1 + 3 * 20Y04M/19Y04M) / 4
                            cov_ratio_04 = 20Y04M/19Y04M

                            non_cov_07 = mean(19Y06M, 19Y07M, 19Y08M)
                            non_cov_04 = mean(19Y03M, 19Y04M, 19Y05M)
                            
                            예측 7월 = non_cov_07 * cov_ratio_07
                            예측 4월 = non_cov_04 * cov_ratio_04

                        elif 특별한 규칙이 없다: .........3)
                            # 다항회귀 모델 예측후 rmse 가 0.3보다 크거나 같고, diff_per가 30% 이상일 경우
                            cov_ratio_07 = (1 + 4 * mean(20Y02M, 20Y03M, 20Y04M) / mean(19Y02M, 19Y03M, 19Y04M)) / 5
                            cov_ratio_04 = mean(20Y02M, 20Y03M) / mean(19Y02M, 19Y03M)
                            
                            non_cov_07 = mean(19Y05M, 19Y06M, 19Y07M, 19Y08M)
                            non_cov_04 = mean(19Y02M, 19Y03M, 19Y04M, 19Y05M)
                            
                            예측 7월 = non_cov_07 * cov_ratio_07
                            예측 4월 = non_cov_04 * cov_ratio_04
                            
                elif p >= 0.5:  ## 변동성이 크다.
                
                    if 변화가 선형적이다: .........4)
                        cov_ratio_07 = (1 + 3 * mean(20Y02M, 20Y03M, 20Y04M) / mean(19Y07M, 19Y08M, 19Y09M)) / 4
                        cov_ratio_04 = mean(20Y02M, 20Y03M) / mean(19Y07M, 19Y08M, 19Y09M)
                        
                        non_cov_07 = mean(19Y05M, 19Y06M, 19Y07M, 19Y08M)
                        non_cov_04 = mean(19Y05M, 19Y06M, 19Y07M, 19Y08M)
                        
                        예측 7월 = non_cov_07 * cov_ratio_07
                        예측 4월 = non_cov_04 * cov_ratio_04

                    else 변화가 비선형적이다:
                        if 계절성을 갖는다.: .........5)
                            # (다항회귀 모델 예측후 rmse 가 0.3보다 작다) and (diff_per가 30% 미만이다)
                            cov_ratio_07 = (1 + 4 * mean(20Y02M, 20Y03M, 20Y04M) / mean(19Y02M, 19Y03M, 19Y04M)) / 5
                            cov_ratio_04 = mean(20Y02M, 20Y03M) / mean(19Y02M, 19Y03M)
                            
                            non_cov_07 = mean(19Y05M, 19Y06M, 19Y07M, 19Y08M, 19Y09M)
                            non_cov_04 = mean(19Y02M, 19Y03M, 19Y04M, 19Y05M, 19Y06M)
                            
                            예측 7월 = non_cov_07 * cov_ratio_07
                            예측 4월 = non_cov_04 * cov_ratio_04

                        elif 특별한 규칙이 없다: .........6)
                            # (다항회귀 모델 예측후 rmse 가 0.3보다 크다) or (diff_per가 30% 이상이다)
                            cov_ratio_07 = (1 + 4 * mean(20Y02M, 20Y03M, 20Y04M) / mean(19Y02M, 19Y03M, 19Y04M)) / 5
                            cov_ratio_04 = mean(20Y02M, 20Y03M) / mean(19Y02M, 19Y03M)
                            
                            non_cov_07 = ((19Y05M, 19Y06M, 19Y07M, 19Y08M) * 2 + 나머지 기간 AMT) / (전체 기간의 길이 + 4)
                            non_cov_04 = ((19Y02M, 19Y03M, 19Y04M, 19Y05M) * 2 + 나머지 기간 AMT) / (전체 기간의 길이 + 4)
                            
                            예측 7월 = non_cov_07 * cov_ratio_07
                            예측 4월 = non_cov_04 * cov_ratio_04

            else std <= 0.15:
                cov_ratio_07 = (1 + 3 * 20Y04M/19Y04M) / 4
                cov_ratio_04 = 20Y03M/19Y03M
                
                non_cov_07 = mean(19Y06M, 19Y07M, 19Y08M)
                non_cov_04 = mean(19Y03M, 19Y04M, 19Y06M)
                
                전체 평균 * cov_ratio .........7)
                
        else 19년 데이터가 전부 존재하지 않는다: # uncomplete_group
            # cov_ratio_07 구하기
            if 20년 4월 데이터가 존재한다:
                cov_ratio_07 = 20Y04M / 전체평균
            elif 20년 3월 데이터가 존재한다:
                cov_ratio_07 = 0.7 * 20Y03M / 전체평균
            else:
                cov_ratio_07 = 0.4
                
            # cov_ratio_04 구하기
            if 20년 3월 데이터가 존재한다:
                cov_ratio_04 = 20Y03M / 전체평균
            elif 20년 3월 데이터가 존재한다:
                cov_ratio_04 = 20Y02M / 전체평균
            else:
                cov_ratio_04 = 0.4
            
            # 결측을 0으로 채워준다.
            # 20년 1월까지 순차적으로 가중치를 크게 부여한다.
            for i, month in enumerate([20Y01M, 19Y12M, 19Y11M, ..., 19Y01M]):
                총 AMT += AMT[month] * 0.9^(i+1)
            가중치 평균 AMT = 총 AMT / (0.9 + 0.9^2 + 0.9^3 + ...)
                
            non_cov_07 = 가중치 평균 AMT
            non_cov_04 = 가중치 평균 AMT
```
