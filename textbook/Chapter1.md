# Chapter 1 : An Introduction to Recommender Systems

## 1.1 개요

- 추천시스템은 다양한 데이터 소스를 활용해 고객의 관심을 추론하는 것
- 추천 분석은 사용자와 아이템 간의 과거의 상호작용에 기반
- 지식 기반 추천 시스템은 사용자의 과거 이력보다는 사용자가 지정한 요구 사항에 따라 추천
- 추천 알고리즘의 기본 원칙
  - 사용자와 아이템 중심의 활동 사이에 상당한 의존성(dependency)가 존재
  - 의존성은 평점 행렬(사용자와 아이템 간의 평점이 작성된 행렬)에서 데이터 기반 방식으로 학습
  - 학습을 통해 만들어진 결과 모델은 대상 사용자에 대한 예측을 하는 데 사용
  - 사용자가 이용할 수 있는 평가 항목의 수가 많을 수록 사용자의 미래 행동을 견고하게 예측
- 추천시스템의 간략한 예시
  - 협업 필터링 (collaborative filtering): 아직 평가하지 않은 평점을 예측하기 위해 여러 사용자가 이미 평가한 평점을 사용하는 것
  - 콘텐츠 기반 추천시스템 : 콘텐츠는 사용자의 평가 및 아이템의 속성
  - 지식 기반 추천시스템 : 사용자가 대화식으로 자신의 관심 분야를 지정

## 1.2 추천 시스템의 목표

### 예측 모델

- 아이템에 대한 사용자 선호도를 나타내는 학습 데이터를 사용
- m(사용자) x n(아이템)
- 누락된 값(불완전한 값)은 훈련 모델을 통해 예측 -> 행렬 완성 문제, matrix completion

### 랭킹 모델

- 실제로는 사용자에게 추천하기 위해 특정 아이템에 대한 사용자 평점을 예측할 필요 없음
- 상위-k 아이템 결정이 상위-k 사용자 결정 보다 일반적으로 사용
- top-k 추천 문제 : 추천 문제의 순위를 계산
- 예측된 평점의 수치값이 중요하지 않다.
- 예측 모델 수행 후, 예측 결과에 순위를 지정해 결과를 얻음(이 방법보다 랭킹 모델을 직접 설계하는 것이 더 쉽고 자연스러움,13장 논의예정)

### 추천시스템의 기술적 목표

1. 관련성

- 사용자와 관련있는 아이템 추천

2. 참신성

- 사용자가 이전에 보지 못했던 아이템을 추천
- 인도 음식을 먹는 사용자에게 새로 생긴 인도 음식점을 추천

3. 의외성(serendipity)

- 사용자들이 이전에 알지 못했던 내용의 추천이라기보다는 "정말 뜻밖의 추천"
- 사용자는 다른 유형의 아이템에 대한 잠재적 관심이 있더라도, 특정 유형의 아이템만을 소비하는 경우가 있음
- 인도 음식을 주로 먹는 사용자에게 에티오피아 음식을 추천
- 판매 다양성을 높이거나 사용자의 새로운 관심이 시작되는 데 좋을 효과
- 새로운 관심 영역 발견 -> 판매자에게 장기적이고 전략적인 이익을 가져다줌
- 관련성이 없는 아이템을 추천할 수 있다는 단점 존재

4. 증가된 추천 다양성

- 일반적으로 추천 시스템은 상위-k개의 아이템들을 추천하는데, 이들이 모두 유사한 아이템일 때, 사용자의 선택을 받지 못할 가능성이 존재
- 추천된 리스트에 다른 유형의 상품이 포함되어있으면 사용자들이 추천 리스트 중 최소한 하나를 선택할 것
- 사용자가 반복된 추천으로 인해 지루해지지 않도록함

### 추천 시스템 예시

1. 그룹렌즈 추천 시스템

- 유즈넷 독자의 평점을 수집해 다른 독자가 기사를 읽기 전에 기사를 읽고 싶어하는지 예측하는데 사용
- 협업 필터링 연구에 관한 선구적 공헌, 데이터셋 공개

2. 아마존 추천 시스템

- 추천 정보는 명시적으로 제공된 평가(명시적 평점), 구매 행위 및 검색 행위(암묵적 평점)에 근거해 제공
- 선호 단계는 5점 척도로 지정
- 아마존에서 지원하는 계정 인증 메커니즘으로 사용자가 로그인하면 고객별 구매 및 검색 데이터를 쉽게 수집 가능
- 추천 정보에 대한 설명 제공: 추천 상품과 이전에 구매한 상품의 관계가 추천 시스템 인터페이스에 포함될 수 있음

3. 넷플릭스 영화 추천 시스템

- 사용자에게 영화 및 TV 프로그램을 5단계로 평가
- 다양한 아이템을 보는 관점으로 사용자 평가를 저장
- 이러한 "평가"와 "저장 방식"은 넷플릭스에서 추천 정보를 작성하는 데 사용됨
- 사용자가 시청한 특정 아이템을 기반으로 추천 정보를 제공
- 넷플릭스 프라이즈 콘테스트를 주최, 잠재 요인 모델과 같은 추천 알고리듬이 대중화됨

4. 구글 뉴스 개인화 시스템

- 클릭 기록을 기반으로 사용자에게 뉴스를 추천
- 아이템 : 뉴스 기사
- 뉴스 기사를 클릭하는 행위 : 해당 기사에 대한 긍정적인 평가
- 아이템에 대한 선호는 표시할 수 있지만, 비선호는 표시 못함 = 단항 평가
- 평점은 사용자 작업에서 유추되기 때문에 암시적
- 협업 추천 알고리듬이 적용되므로 특정 사용자에 대한 개인화된 기사에 대한 추론이 이루어질 수 있음

5. 페이스북 친구 추천 시스템

- 제품 추천과 다른 목표 : 사용자의 경험이 풍부해짐, 소셜 네트워크 성장에 기여
- 평점 데이터가 아닌 구조적 관계에 기반하기 때문에 특성이 완전히 다름

### 1.3 추천 시스템의 기본 모델

1. 사용자-아이템 인터렉션을 이용하는 방법

- 협업 필터링 방법 (collaborative filtering methods)

2. 사용자와 아이템에 관련된 속성 정보를 이용한 방법

- 콘텐츠 기반 추천 방법 (content-based recommender systems)
  : 모든 사용자가 아닌 특정 사용자의 평점에 비중을 둠

3. 지식 기반 추천 시스템 (knowledge-based recommender systems)

- 특정 사용자의 조건을 기반으로 추천
- 과거 평점이나 구매 데이터를 이용하는 대신, 외부 지식 기반과 제한 조건을 활용

4. 하이브리드 시스템
   : 여러 종류의 추천 시스템의 장점을 혼합해 다양한 환경에 대응할 수 있도록 함

### 1.3.1 협업 필터링 모델

- 여러 사용자의 평점을 협업해 추천
- 배경 : 기본이 되는 평점 행렬의 분포가 고르지 않음
  ex) 영화 평점 -> 보지 않은 영화의 평점은 명시되지 않음
  - 명시된(specified) = 관측한(observed)
  - 명시되지 않은 = 관측하지 않은(unobserved) = 누락된(missing)
- 기본 구조 : 발견된 평점은 사용자와 아이템과 매우 높은 상관관계를 갖고 있어 명시되지 않은 평점 또한 대체가 가능하다는 것
  ex) 두 사람의 평점 후기가 매우 유사하다면, 한 사람만 특정 영화에 대해 평가했을 때, 다른 한 사람도 유사한 평가를 내렸다고 유추할 수 있다
- 목표 : 아이템 간 상관관계나 사용자 간 상관관계를 예측 프로세스에 활용하는 데 중점을 둠, 둘의 상관관계를 모두 활용하기도 함, 최적화 방법을 통해 모델 훈련시키기도 함
- 대표적인 방법 두 가지
  - 메모리 기반 방법 (memory-based methods)
  - 모델 기반 방법 (model-based methods)

1. 메모리 기반 방법 (= 이웃 기반 협업 필터링 알고리듬)

- 협업 필터링 알고리듬 중 초기의 방법론
- 이웃을 기반으로 사용자-아이템 조합의 평점을 예측
- 이웃을 정의하는 두 가지 방법
  1. 사용자 기반 협업 필터링
  - 사용자 A와 유사한 성향의 사용자들의 평점 결과로 A에게 추천
  - A와 가장 유사한 k명의 사용자들의 평점 결과로 A에게 추천
  2. 아이템 기반 협업 필터링
  - 사용자 A를 통해 타깃 아이템 B의 평점을 예측하려면 타깃 아이템인 B와 가장 유사한 아이템 집합 S의 정의
  - 아이템 집합 S는 사용자 A가 아이템 B를 좋아할지 안 좋아할지 예측하는데 쓰임
    ex)A가 아직 보지 않은 <터미네이터>영화 평점을 예측하는데, A가 본 비슷한 유형의 영화인 <에이리언>과 <프레데터>에 대한 평점을 사용
- 장점 : 적용하기 간단, 추천결과 설명 간단
- 단점 : 분포가 고르지 못한 평점에는 잘 작동하지 않음
  ex) A와 유사한 사용자 중에서 아이템 B에 평점을 매긴 사용자가 별로 없다면 예측이 어려움 (상위 k개의 아이템만 필요하다면 괜찮다)

2. 모델 기반 방법

- 예측 모델에 머신러닝과 데이터 마이닝 기술을 이용
- 모델(파라미터)는 최적화 단계에 따라 학습됨
- 예시: 의사 결정 트리(decision trees), 룰 기반 모델(rule-based models), 베이지안 방법론(Bayesian methods), 잠재 요인 모형(Latent factor models)
- 잠재 요인 모형(Latent factor models) : 고르지 않은 평점 행렬에서도 높은 수준의 커버리지를 보여줌

#### 1.3.1.1 평점의 종류

-

## 1.5

## 1.6

## 1.7

## 1.8
