# AI_Section2_Project
## Repository 설명

- AI_11_노희섭_Section2.pptx ⇒ 프로젝트 발표에 사용한 ppt
- README.md ⇒ 프로젝트 진행 당시 활용한 프로젝트 평가표
- Section2_Proejct(03NN).ipynb ⇒ 프로젝트에 사용한 코드
- 깃허브 업로드 용 발표 자료
    - 프로젝트를 발표할 때 사용한 PPT 파일을 수정하여 GitHub에서 바로 볼 수 있게 PDF로 수정
    - 아래 내용보다 더 자세하게 설명이 되어 있음.

## 프로젝트에서 풀고자 하는 문제

- 시청률 회귀 분석을 통해 CF를 어느 채널과 어느 시간대에 넣을 것인지 결정
    - 상황: 30대 남자를 대상으로 하는 TV 마케팅
    - 목표: TV CF를 넣을 채널과 시간대 결정
    - 방법: 시청률 회귀 분석

## 프로젝트 진행 과정

- 데이터 전처리
    - 데이터 세트 기초 현황 파악
    - 데이터 결합, 특성 공학 등 전처리 진행
- 모델 설정 및 평가
    - `Baseline`설정 및 평가지표 확인
    - `RandomForestRegression RandomizedSearchCV`모델 학습 및 평가지표 확인
    - `Multiple Linear Regression`  모델 학습 및 평가지표 확인
    - `XGBoostRegression` 모델 학습 및 평가지표 확인
    - `XGBoostRegression RandomizedSearchCV` 모델 학습 및 평가지표 확인
- 모델 설명
    - Gain 기준
    - Weight 기준
    - Cover 기준
    - PDP 그래프
    - SHAP 그래프

## 데이터셋 설명

- 데이터 출처: 한국문화정보원 문화 빅데이터 플랫폼
- 전체 데이터 세트 ⇒ 14,039 * 28
- K-드라마 프로그램, 채널, 시청률 등 TV 콘텐츠 데이터

## 문제 상황 해결 과정(분석 기법, 모델 등)

- 불필요한 컬럼 삭제
    - 방송 일자, 방송 종료 일자, 프로그램 종료 시간, 프로그램 장르 대분류명 등 개별 데이터가 가진 내용이 크게 차이가 나지 않거나, 회귀 분석에 불필요하다고 판단하는 내용 삭제
- 24시간을 3시간 권역대별로 분류
    - 22시 20분에 시작을 하든 22시 21분에 시작을 하든 시청자의 생활 패턴에는 유의미한 영향을 미치기 어려움.
    - 그렇기에 권역대별로 분류하여 분석을 진행함
    - 예)
        - 00:00 ~ 03:00 에 방송시작한 프로그램은 A시간대
        - 18:00 ~ 21:00 에 방송시작한 프로그램은 G시간대
- 다양한 모델 사용
    - Base Line 기준 모델 ⇒ 모델의 성능을 평가하는 기준이 되는 모델
    - Random Forest 모델 ⇒ 의사결정나무를 기반으로 만든 모델
    - Multiple Linear 모델 ⇒ 선을 기반으로 만든 모델
    - XGBoost 모델 ⇒ 랜덤포레스트와 비슷한 모델이지만 오류에 가중치를 반영하는 모델
- 모델에 영향을 끼친 요인들 분석
    - 특성 중요도
        - Gain: 해당 특성이 모델 예측에 얼마나 영향을 미쳤는가
        - Weight: 해당 특성과 관련된 샘플의 상대적인 개수
        - Cover: 해당 특성이 모델의 의사결정에 사용된 횟수
    - PDP 그래프
        - 해당 특성이 모델의 분석에 어떤 영향을 주었는지 확인할 수 있다.
    - SHAP 그래프
        - 모델이 개별 특성 값을 어떻게 예측하였는지 확인할 수 있다.

## 결과 정리

- 모델
    - XGBoost(Hyper Parameter Tuning) 모델 → 평가지표가 비교적 우수하다.
    - r2: 0.877, adjusted r2: 0.872, MSE: 0.028
- 시간대와 채널
    - G 시간대 → 18:00 ~ 21:00시 시간대에 시작하도록 프로그램 상영 시간대 조절
    - SBS 채널 → 채널 중에서 가장 큰 영향을 미쳤다.
- 기타
    - 30대 여성의 시청률도 30대 남성의 시청률에 큰 영향을 미쳤다.

## 한계점 및 해결 방안

- 결과론적으로 30대 여성의 시청률과 높은 상관관계가 있는 것으로 나온듯하다. 유의미한 해석 결과로 볼 수 있을지 불확실하다고 판단한다.
    - 드라마와 같은 콘텐츠의 질이 중요한 상품의 경우에는 관련 리뷰, 흥행 지수 등도 수집하여 데이터에 넣으면 더욱 예측의 결과가 좋으리라 생각함
    - 특히 최근 큰 인기를 끌고 있는 ‘이상한 변호사 우영우’의 경우는 위 분석방법을 사용하였을 경우 큰 오차가 발생할 것으로 예상함.
    - ‘ENA’라는 신생 채널이라는 특성과, 방송 시간대 등은 1화부터 현재까지 동일하지만 콘텐츠의 질이 소비자의 선호에 부합했기 때문에 큰 인기를 끌고 있기 때문. (1화 0.9%, 13화 13.5%로 폭등)




![image001](https://user-images.githubusercontent.com/88868181/159431142-c24a4b4a-c9ae-474b-aa6b-e85945361e11.png)
![image002](https://user-images.githubusercontent.com/88868181/159431150-ebfa8021-aca0-4eeb-86da-036333121383.png)
![image003](https://user-images.githubusercontent.com/88868181/159431134-a63790ea-212a-427b-a8e5-3beb02e56958.png)

