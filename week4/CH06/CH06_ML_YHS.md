# CH06 머신러닝 프로세스

6장에서 다루는 내용
- 데이터로부터의 지도 및 비지도학습의 작동법
- 회귀와 분류 작업을 지도학습 모델의 훈련과 평가
- 편향 분산 트레이드오프가 예측 성과에 영향을 미치는 방법
- 과대적합에 기인한 예측 오차를 진단하고 다루는 방법
- 시계열 데이터에 초점을 맞추고 하이퍼파라미터를 최적화하고자 교차 검증을 사용하는 방법
- 표본 외 테스트를 수행할 때 금융 데이터가 추가적인 주의를 요하는 이유

## 데이터로부터 머신러닝이 작동하는 방법
### 도전 과제: 알고리듬을 작업에 매칭
- 지도학습의 가장 큰 도전 과제는 새로운 데이터에 대해서도 모델의 학습이 일반화될 수 있게 훈련 데이터에서 의미 있는 패턴을 인식하는 것

### 지도학습: 예제에 의한 학습
- 지도학습의 목적은 입력과 출력 데이터 간의 함수적 관계를 반영하는 데이터에서 이러한 관계를 포착하고, 학습을 적용해 새로운 데이터에 대해 유효한 명제를 도출하는 것

### 비지도학습: 유용한 패턴의 발견
- 비지도학습의 목적은 미래 결과를 예측하거나 변수 간의 관계를 추론하는 대신 데이터에 포함된 정보의 새로운 표현을 허용하는 구조를 식별하는 것
#### 사용 사례: 리스크 관리에서 텍스트 처리까지
- 유사한 리스크와 수익률 성격을 지닌 자산들을 그룹화하는 것(13장의 계층적 리스크 패리티)
- 주성분 분석(13장)과 오토인코더(19장)을 활용한 훨씬 많은 수의 증권의 성과를 내는 작은 수의 리스크 팩터를 찾는 것
- 일련의 문서에서 가장 중요한 측면을 포함하는 잠재적인 주제를 식별하는 것(14장)

#### 클러스터링 알고리듬: 유사한 관측을 찾는 것
- 클러스터링 알고리듬의 목적은 유사도를 적용해 비교 가능한 정보를 지닌 관측치들이나 데이터 특성들을 식별하는 것
    - K-평균 클러스터링
    - 가우시안 혼합 모델
    - 밀도 기반 클러스터
    - 계층적 클러스터

#### 차원 축소: 정보의 압축
- 차원 축소의 목적은 원천 데이터에 포함된 가장 중요한 정보를 포착하는 새로운 데이터를 산출하는 것
    - 주성분 분석
    - 매니폴드 학습
    - 오토인코더

### 강화학습
- 강화학습에서 환경에 의해 주어진 정보를 기반으로 매 타임스텝에서 행동을 선택하는 에이전트 존재
- 에이전트의 목적은 환경의 현재 상태를 묘사하는 일련의 관측치를 기반으로 시간에 걸쳐 가장 큰 보상을 산출하는 행동을 선택하는 것

## 머신러닝 워크플로
1. 문제 정의와 성공 척도
2. 데이터 수집, 정제 및 검증
3. 특성 탐색, 추출 및 공학
4. 머신러닝 알고리듬 선택
5. 모델 설계와 하이퍼파라미터의 교차 검증
6. 모델 실행과 예측

### 기본 설명: k-최근접 이웃
- k-최근접 이웃(KNN)은 유클리드 거리를 기반으로 k개의 최근접 데이터 포인트를 식별해서 예측을 수행

### 문제의 구성: 목적과 성과 측정
- 회귀 문제: 연속 출력 변수
- 분류 문제: 범주형 변수
- 순위 문제: 순서가 있는 범주형 변수
#### 예측 대 추론
##### 인과 추론: 상관관계는 인과를 의미하지 않는다.
- 인과 추론의 목적은 특정 입력값이 특정 출력값을 의미하는 관계를 식별하는 것

##### 회귀: 인기 있는 손실함수와 오차 척도
- 회귀 문제의 목적은 연속 변수를 예측하는 것
    - 평균 제곱근 오차(RMSE)
    - 평균 제곱근 로그 오차(RMSEL)
    - 평균 절대 오차(MAE)
    - 중앙값 절대 오차(MedAE)
    - 설명된 분산 점수
    - R2 점수(=결정 계수)

##### 분류 문제: 혼동 행렬의 이해
- 분류 문제의 목적은 범주형 결과 변수의 예측기에서 어떤 관측이 특정 클래스에 속하는가를 나타내는 점수를 산출하는 것
- 혼동 행렬

    |          | 예측값 0 | 예측값 1 |
    |----------|---------|---------|
    | 실제값 0 |   TN    |   FP    |
    | 실제값 1 |   FN    |   TP    |

    - 참 양성(True Positive, TP): 실제로 참인 데이터를 참으로 예측한 경우
    - 참 음성(True Negative, TN): 실제로 거짓인 데이터를 거짓으로 예측한 경우
    - 거짓 양성(False Positive, FP): 실제로는 거짓인 데이터를 참으로 잘못 예측한 경우
    - 거짓 음성(False Negative, FN): 실제로는 참인 데이터를 거짓으로 잘못 예측한 경우

##### ROC는 곡선 아래 면적을 특징짓는다.
- ROC 곡선은 모든 조합의 참 양성률(TPR)과 거짓 양성률(FPR)를 계산하는 것

##### 정밀도-재현율 곡선
- 정밀도-재현율 곡선은 여러 임곗값에 대한 오차 척도 간의 트레이드오프를 시각화
- 재현율(Recall)은 분류기가 주어진 임곗값에 대해 양성으로 예측하는 클래스 멤버의 실제 양성 비중을 측정
- 정밀도: 양성 예측 중 참인 비중
- F1 점수(F1 score): 주어진 임곗값에 대한 정밀도와 재현율의 조화 평균

#### 데이터 수집과 준비

#### 특성 탐험, 추출, 특성 공학

#### 정보 이론을 활용한 특성 평가
- 상호 정보량: 두 변수 간 상호 의존성의 척도

### ML 알고리듬 선택

### 모델 설계와 조정
- ML 프로세스의 목적은 통계적으로 건전하고 효율적인 절차를 사용해 일반적 오차의 불편 추정치를 얻는 것

#### 편향과 분산의 트레이드오프
- 축소 불가능한 부분
    - 자연적 변이
    - 측정 오차에 기인한 데이터 랜덤 변이(잡음)
- 축소 가능한 부분
    - 편향에서 기인한 에러
    - 분산에서 기인한 에러

#### 과소적합 대 과대작업: 시각적 예제

#### 편향 분산 트레이트오프를 다루는 방법

#### 학습 곡선

### 모델 선택을 위한 교차 검증의 활용
- 모델 선택 문제: 사용 사례를 위해 여러 후보 모델이 존재할 때 그 중 하나를 선택하는 것
- 교차 검증(CV): 데이터를 한 번 또는 여러 번으로 분할하여, 각 분할이 검증 세트로 한 번 사용되고 나머지는 훈련 세트로 사용되도록 수행
    - CV의 주요 가정: 데이터가 독립적으로 동일한 분포를 따른다는 것
    - 단일 잔류 교차 검증
    - P개 잔류 교차 검증
    - 셔플 분할

### 금융에서 교차 검증의 문제
- 금융 데이터는 이분산성이라고도 하는 계열 상관과 시간 가변 표준 편차 때문에 독립적이거나 독립 분포가 아니다.

#### 사이킷런을 이용한 시계열 교차 검증
- 데이터 스누핑: 데이터의 시계열적 특성은 교차 검증이 미래 데이터를 사용해 과거 데이터를 예측하는 상황 초래
    - 시간 의존성을 해결하고자 TimeSeriesSplit 객체는 확장 훈련 세트로 전방 진행 테스트를 구현

#### 제거, 엠바고, 조합적 CV
- 제거: 검증 세트에서 시점 데이터 포인트를 예측한 후 평가가 이뤄지는 훈련 데이터 포인트를 제거해 선경 편향을 방지
- 엠바고: 테스트 기간을 따른 훈련 표본을 추가로 제거
- 조합적 교차 검증
    1. 전방 진행 CV는 테스트할 수 있는 과거 경로를 심각하게 제한한다.
    2. 대신 T개의 관측치가 주어지면 각각의 순서를 유지하는 N<T 그룹에 대해 가능한 모든 훈련/테스트 분할을 계산하고 중복될 가능성이 있는 그룹을 제거하고 엠바고한다. 
    3. 그런 다음 나머지 k 그룹에서 모델을 테스트하는 동안 N-k 그룹의 모든 조합에 대해 모델을 훈련한다.

### 사이킷런을 이용한 파라미터 조정과 옐로우 브릭
- 엘로우 브릭 라이브러리: sklearn API를 확장해 모델 선택 프로세스를 용이하게 하는 진단적 시각화 도구를 생성
#### 검증 곡선: 하이퍼파라미터의 영향을 그대로 표현
#### 학습 곡선: 편향 분산 트레이드오프의 진단
#### 그리드서치와 파이프라인을 이용한 파라미터 조정
