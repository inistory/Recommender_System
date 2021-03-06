
# Chapter 3 Model-Based Collaborative Filtering

# 3.1 Introduction

**요약**
- Collaborative filtering은 NEIGHBORHOOD-BASED COLLABORATIVE FILTERING과 Model-Based Collaborative Filtering로 나뉨
-  3장에서는 Model-Based 을 다룸
- 모델기반 방법은 머신러닝 기반으로 평점을 예측
- **행 : 유저**
- **열: 아이템**

---
**Neighborhood-based Collaborative Filtering**
- 이웃 기반 방법은 기계 학습에서 일반적으로 사용되는 k-최근접 이웃 분류기를 일반화한 것
- 이러한 방법은 인스턴스 기반 방법이므로 효율적인 구현을 위해 필요한 선택적 사전 처리1 단계 외에는 예측을 위해 미리 모델을 특별히 생성하지 않습니다. 
- 이웃 기반 방법은 인스턴스 기반 학습 방법의 일반화 또는 예측 접근 방식이 예측되는 인스턴스에 특정한 지연 학습 방법입니다. 
- 예를 들어, 사용자 기반 이웃 방법에서는 예측을 수행하기 위해 대상 사용자의 피어가 결정됩니다.
- ---
**Model-based Collaborative Filtering**
- 모델 기반 방법에서는 지도 또는 비지도 기계 학습 방법과 마찬가지로 데이터의 요약 모델이 미리 생성됨
-  따라서 훈련(또는 모델 구축 단계)은 예측 단계와 명확하게 분리
- 전통적인 기계 학습에서 이러한 방법의 예로는 의사 결정 트리, 규칙 기반 방법, Bayes 분류기, 회귀 모델, 지원 벡터 기계 및 신경망
- 흥미롭게도, k-최근접 이웃 분류기가 협업 필터링을 위해 이웃 기반 모델로 일반화될 수 있는 것처럼 거의 모든 이러한 모델은 협업 필터링 시나리오로 일반화될 수 O
-  이는 전통적인 분류 및 회귀 문제가 행렬 완성(또는 협업 필터링) 문제의 특수한 경우이기 때문입니다.

-Figure 3.1: Revisiting Figure 1.4 of Chapter 1. Comparing the traditional classification problem with collaborative filtering. Shaded entries are missing and need to be predicted.
- 데이터 분류 문제에서 첫 번째(n − 1) 열이 특성 변수(또는 독립 변수)이고 마지막(즉, n번째) 열이 클래스 변수(또는 종속 변수)인 m × n 행렬이 있습니다. ). 
- 첫 번째(n − 1) 열의 모든 항목은 완전히 지정되지만 n번째 열의 항목 하위 집합만 지정됩니다. 
- 따라서 행렬에 있는 행의 하위 집합이 완전히 지정되고 이러한 행을 훈련 데이터라고 합니다. 
- 나머지 행을 테스트 데이터라고 합니다. 
- 테스트 데이터에 대해 누락된 항목의 값을 학습해야 합니다. 
- 음영 처리된 값은 행렬에서 누락된 항목
- --
-  등급 매트릭스의 항목이 누락될 수 있습니다. 
- 따라서 행렬 완성 문제는 분류(또는 회귀 모델링) 문제의 일반화임을 분명히 알 수 있습니다. 
- 따라서 이 두 문제의 결정적인 차이점은 다음과 같이 요약할 수 있습니다.
--- 
 **Classification과 Collaborative filtering 차이점**
1. **데이터 분류 문제에서 특성(독립) 변수와 클래스(종속) 변수 사이에 명확한 구분**이 있습니다**. 행렬 완성 문제에서는 이러한 명확한 분리가 존재하지 않습니다.** 각 열은 주어진 지점에서 예측 모델링을 위해 고려되는 항목에 따라 종속 및 독립 변수입니다.
2. **데이터 분류 문제에서 훈련 데이터와 테스트 데이터가 명확하게 구분**됩니다. **행렬 완성 문제에서 이 명확한 경계는 행렬의 행 사이에 존재하지 않습니다.** 기껏해야 지정된(관찰된) 항목을 교육 데이터로 간주하고 지정되지 않은(누락된) 항목을 테스트 데이터로 간주할 수 있습니다.
3. **데이터 분류에서 열은 기능을 나타내고 행은 데이터 인스턴스**를 나타냅니다. 그러나 **협업 필터링에서는 누락된 항목이 분산되는 방식 때문에 등급 매트릭스 또는 전치(열과 행 바꿔도됨) 에 동일한 접근 방식을 적용**할 수 있습니다. 예를 들어, 사용자 기반 이웃 모델은 최근접 이웃 분류기의 직접적인 일반화로 볼 수 있습니다. 이러한 방법을 등급 매트릭스의 전치에 적용하면 item-based neighborhood model이라고 합니다. 일반적으로 협업 필터링 알고리즘의 많은 클래스에는 user-wise and item-wise versions 이 있습니다.
---
- 협업 필터링 문제의 더 큰 일반성은 데이터 분류에 비해 협업 필터링에서 더 많은 알고리즘 가능성으로 이어집니다.
- --
**협업 필터링 문제와 데이터 분류 문제 사이의 유사성**  
협업 필터링 문제에 대한 학습 알고리즘을 설계할 때 염두에 두면 유용
- 사실, 대부분의 기계 학습 및 분류 알고리즘은 협업 필터링 문헌에서 직접적인 유사성을 가지고 있습니다.
-  분류 모델과 유사한 방식으로 추천 시스템을 이해하면 분류 문헌에서 가져온 많은 수의 메타 알고리즘을 적용할 수 있습니다. 예를 들어, 배깅, 부스팅 또는 모델 조합과 같은 분류 문헌의 고전적인 메타 알고리즘은 협업 필터링으로 확장될 수 있습니다. 
- 흥미롭게도 분류에서 앙상블 방법을 위해 개발된 이론의 대부분은 계속해서 추천 시스템에 적용됩니다. 사실 앙상블 기반 방법[311, 704]은 Netflix 챌린지에서 가장 성능이 좋은 방법 중 하나였습니다. 
- --
- 그러나 데이터 분류 모델을 행렬 완성 문제로 직접 일반화하는 것이 항상 쉬운 것은 아닙니다. 
- 특히 대부분의 항목이 누락된 경우에는 더욱 그렇습니다. 
- 더욱이 다양한 모델의 상대적 효율성은 상황에 따라 다릅니다. 예를 들어, 잠재 요인 모델과 같은 많은 최근 협업 필터링 모델은 협업 필터링에 특히 적합합니다. 그러나 이러한 모델은 데이터 분류의 맥락에서 경쟁 모델로 간주되지 않습니다.
---
**model based 장점**

1. **공간 효율성:** 일반적으로 학습된 모델의 크기는 원래 등급 매트릭스보다 훨씬 작습니다. 따라서 공간 요구 사항은 종종 매우 낮습니다. 반면에 사용자 기반 이웃 방법은 O(m2) 공간 복잡도를 가질 수 있습니다. 여기서 m은 사용자 수입니다. item 기반 방법은 O(n2) 공간 복잡도를 갖습니다. 계산할게 줄어든다.
2. .**학습 속도 및 예측 속도:** 이웃 기반 방법의 한 가지 문제는 전처리 단계가 사용자 수 또는 item 수에서 2차적이라는 것입니다. **모델 기반 시스템은 일반적으로 훈련된 모델을 구성하는 전처리 단계에서 훨씬 빠릅니다.** 대부분의 경우 간결하고 요약된 모델을 사용하여 예측을 효율적으로 수행할 수 있습니다.
3. **과적합 방지:**  **model-based 의 요약 접근 방식은 종종 과적합을 피하는 데 도움이 될 수 있습니다.** 또한 정규화 방법을 사용하여 이러한 모델을 견고하게 만들 수 있습니다.

---
**Neighborhood-based Collaborative Filtering 한계**

- 이웃 기반 방법이 가장 초기의 협업 필터링 방법 중 하나였으며 단순성 때문에 가장 인기가 있었지만 오늘날 사용할 수 있는 가장 정확한 모델은 아닙니다. 
- 사실, 가장 정확한 방법 중 일부는 일반적으로 모델 기반 기술, 특히 잠재 요인 모델을 기반으로 합니다.
- --
**3장의 구성**

- 3.2 : 추천 시스템을 위한 의사 결정 및 회귀 트리의 사용
-  3.3 : 규칙 기반 협업 필터링 방법
-  3.4: 추천 시스템에 대한 순진한 Bayes 모델의 사용
- 3.5:  다른 분류 방법이 협력 필터링으로 확장되는 방법에 대한 일반적인 논의
- 3.6 : 잠재 요인 모델
- 3.7 : 잠재 요인 모델과 이웃 모델의 통합
- 3.8 : 요약

## 3.2 Decision and Regression Trees
- The decision tree is a hierarchical partitioning of the data space with the use of a set of hierarchical decisions, known as the split criteria in the independent variables.
- When each node in a decision tree has two children, the resulting decision tree is said to be a binary decision tree.

### 3.2.1 Extending Decision Trees to Collaborative Filtering
- Decision tree를 어떻게 추천에서 쓸지
- 의사 결정 트리를 협업 필터링으로 확장할 때의 주요 과제는 **예측 항목과 관찰 항목이 특성 및 클래스 변수로 열 방식으로 명확하게 분리되지 않는다는 것**입니다. 
- 또한, Rating Matrix 는 대부분의 항목이 누락되어있음
- 이로 인해 트리 구축 단계에서 train 데이터를 계층적으로 분할하는 데 문제가 발생합니다.
-  게다가, 종속변수와 독립변수(항목)가 협업 필터링에서 명확하게 구분되지 않기 때문에 의사결정 트리에서 어떤 item을 예측해야 합니까? 
- --
**종속변수와 독립변수(item)가 협업 필터링에서 명확하게 구분되지 않는 문제**

- 각 item의 등급을 예측하기 위해 별도의 의사 결정 트리를 구성하여 비교적 쉽게 해결할 수 있습니다. 
- m개의 사용자와 n개의 항목이 있는 m × n 등급 행렬 R을 고려하십시오. 각 속성(항목)을 종속으로 고정하고 나머지 속성을 독립으로 고정하여 별도의 의사결정 트리를 구성해야 합니다. 
- 따라서 구성된 의사결정 트리의 수는 속성(항목)의 수 n과 정확히 동일합니다. 사용자에 대한 특정 항목의 등급을 예측하는 동안 해당 항목에 해당하는 의사 결정 트리를 사용하여 예측합니다.
- ---
- 반면에 독립적인 기능이 누락된 문제는 해결하기가 더 어렵습니다. 
- 특정 항목(예: 특정 영화)이 분할 속성으로 사용되는 경우를 고려하십시오. 
- 평가가 임계값보다 작은 모든 사용자는 트리의 한 분기에 할당되고 평가가 임계값보다 큰 사용자는 다른 분기에 할당됩니다. 
- 평가 매트릭스가 희소하기 때문에 대부분의 사용자는 이 항목에 대한 평가를 지정하지 않습니다. 
- 그러한 사용자는 어느 지점에 할당되어야 합니까? 
- 논리에 따르면 이러한 사용자는 두 분기에 모두 할당되어야 합니다. 
- 그러나 이러한 경우 의사 결정 트리는 더 이상 훈련 데이터의 엄격한 분할로 남아 있지 않습니다. 
- 또한, 이 접근 방식에 따르면 테스트 인스턴스는 의사 결정 트리의 여러 경로에 매핑되며 다양한 경로의 충돌 가능성이 있는 예측을 단일 예측으로 결합해야 합니다.
- ---
- 두 번째(더 합리적인) 접근 방식은 차원 축소 방법을 사용하여 데이터의 저차원 표현을 만드는 것입니다.
-  j 번째 item의 등급이 다음과 같아야 하는 시나리오를 고려하십시오. 
- 맨 처음에 j번째 열을 제외한 m × (n − 1) 등급 행렬은 d ≪ n − 1 및 모든 속성이 완전히 지정되는 저차원 m × d 표현으로 변환됩니다. 
- m × (n − 1) 등급 행렬의 각 item 쌍 간의 공분산은 2장의 섹션 2.5.1.1에서 논의된 방법을 사용하여 추정됩니다. 
- 최상위 고유벡터 e1 . . . 추정된 (n − 1) × (n − 1) 공분산 행렬의 ed가 결정됩니다. 
- 각 고유 벡터는 (n − 1) 요소를 포함하는 벡터입니다. 
- 수학식 2.17은 j번째 항목이 수학식 2.17의 우변에 포함되지 않는다는 점을 제외하고 고유 벡터에 대한 각 사용자의 등급을 투영하는 데 사용됩니다. 
- 그 결과 각 사용자에 대한 등급의 d차원 벡터가 생성되며 완전히 지정됩니다. 
- 이 축소 표현은 문제를 표준 분류 또는 회귀 모델링 문제로 처리하여 j번째 항목에 대한 의사 결정 트리를 구성하는 데 사용됩니다. 
- 이 접근법은 총 n개의 결정 트리를 구성하기 위해 1에서 n까지 j 값을 변경하여 반복됩니다.
-  따라서 j번째 의사결정트리는 j번째 항목의 등급을 예측하는 데에만 유용합니다. n개의 경우 각각에 대한 고유 벡터와 트리는 모두 모델의 일부로 저장됩니다.
- --
- 사용자 i에 대한 j 항목의 등급을 예측하기 위해 m x d 행렬의 i번째 행을 테스트 인스턴스로 사용하고 j번째 결정/회귀 트리를 모델로 사용하여 해당 등급의 값을 예측합니다. 
- 첫 번째 단계는 식 2.17에 따라 테스트 인스턴스의 축소된 d-차원 표현을 생성하기 위해 나머지 n - 1개 항목(j번째 항목 제외)을 사용하는 것입니다. 
- j번째 고유 벡터 세트는 투영 및 축소 프로세스에 사용됩니다. 
- 그런 다음 이 표현은 예측을 수행하기 위해 j번째 항목에 대한 해당 결정 또는 회귀 트리와 함께 사용됩니다. 
- 차원 축소를 분류 모델과 결합하는 이러한 광범위한 접근 방식이 의사 결정 트리 말고도 많음
- 거의 모든 분류 모델과 함께 이 접근 방식을 사용하는 것은 비교적 쉽습니다. 
- 또한 차원 축소 방법은 추천 시스템의 등급을 예측하기 위해 별도로 사용됩니다. 이 두 가지 문제는 이 장의 뒷부분에서 설명합니다.


**Quiz**

1. 의사결정나무를 CF로 확장할 때의 가장 큰 문제점?
-  예측 항목과 관찰 항목이 특성 및 클래스 변수로 열 방식으로 명확하게 분리되지 않는다는 것
2. rating matrix의 sparsity를 해결한 방법?
- 차원축소 방식을 사용해서 데이터를 저차원으로 만드는 것
- Rating을 매기지 않은 사용자들은 어떻게 분류되는지(두개 다에 분류해줌(?))
3. CF에서는 종속변수와 독립변수가 명확하게 구분되지 않음. 그래서 decision tree에서 예측하고자 하는 것?
- mxn: m은 사용자, n은 item
- n을 종속으로 고정을 하고, 나머지를 독립으로 고정,의사결정의 수랑, 속성의 수 n이랑 동일한걸 보면 item의 등급을 예측하고자 한 것 같다. 
-  각 속성을 종속으로 만들고, 나머지 속성은 독립적으로 바꿔서 별도로 의사결정트리를 구성해준다음, 유저에 대해서 특정item등급을 예측하면서, item에 대한 의사결정트리를 만든다.

## 3.3 Rule-Based Collaborative Filtering
- 연관규칙 분석이라고 데이터 마이닝에서 부름
- 경영학에서는 장바구니 분석
- contents-based recoomendation 의 기본 방법
- --
- 연관 규칙[23]과 협업 필터링 사이의 관계는 연관 규칙 문제가 슈퍼마켓 데이터 간의 관계를 발견하는 맥락에서 처음 제안되었기 때문에 자연스러운 것입니다. 
- 연관 규칙은 이진 데이터에 대해 자연스럽게 정의되지만 이러한 데이터 유형을  이진데이터로 변환하여 범주 및 숫자 데이터로 접근 방식을 확장할 수 있습니다. 
---
- 트랜잭션 데이터베이스 T = {T1 . . . Tm }, n개의 항목 I에 정의된 m개의 트랜잭션을 포함합니다. 
- 따라서 I는 보편적인 item 집합이고 각 트랜잭션 Ti는 I에 있는 item의 하위 집합입니다. 
- **연관 규칙 마이닝의 핵심은 item 집합을 결정하는 것**
- 트랜잭션 데이터베이스: Support(지원)과 Confidence(신뢰)의 개념으로 달성
- 이러한 측정은 item 집합 간의 관계를 수량화합니다.
- --
**Definition 3.3.1 (Support)**

-  (Support) 항목 집합 X ⊆ I의 지원은 T의 트랜잭션 비율이며, X는 부분 집합입니다.
---
- 항목 집합의 지원이 미리 정의된 임계값 s와 적어도 같으면 항목 집합을 frequent(빈도)라고 합니다. 
- 이 threshold(임계값) 을 minimum support(최소 지원)이라고 합니다. 
- 이러한 항목 집합을 빈번한 항목 집합 또는 빈번한 패턴이라고 합니다. 
- 빈번한 항목 집합은 고객 구매 행동의 상관 관계에 대한 중요한 통찰력을 제공할 수 있습니다.
--- 
- 예를 들어, 표 3.1
- 행은 고객에 해당하고 열은 item에 해당
- 1은 특정 고객이 항목을 구매한 경우에 해당
- 이 데이터 세트는 단항이고 0은 결측값에 해당하지만 이러한 암시적 피드백 데이터 세트의 일반적인 관행은 결측값을 0으로 근사하는 것입니다. 
	- 단항 : 좋아요만 있는것
	- 이항: 좋아요와 싫어요가 있는것
- 테이블의 열은 밀접하게 관련된 항목의 두 세트로 분할될 수 있음이 분명합니다. 이 세트 중 하나는 {빵,버터,우유}이고 다른 세트는 {생선,쇠고기,햄}입니다. 
- 이것들은 최소 3개의 항목이 있는 유일한 항목 세트이며 최소 0.2의 지원도 있습니다. 따라서 이러한 항목 집합은 모두 빈번한 항목 집합 또는 빈도 패턴입니다. 
- 높은 지원을 받는 이러한 패턴을 찾는 것은 판매자에게 유용합니다. 판매자가 이를 사용하여 권장 사항 및 기타 대상 마케팅 결정을 내릴 수 있기 때문입니다. 
	- 예를 들어, Mary는 이미 {Butter,Milk}를 구입했기 때문에 결국 빵을 살 가능성이 높다고 결론을 내리는 것이 합리적입니다. 마찬가지로 John은 {Fish,Ham}도 구입했기 때문에 Beef를 구입할 가능성이 높습니다. 이러한 추론은 추천 시스템의 관점에서 매우 유용합니다.
---
- 연관 규칙 및 신뢰도의 개념을 사용하여 이러한 상관 관계의 방향과 관련하여 추가 수준의 통찰력을 얻을 수 있습니다.
-  연관 규칙은 X ⇒ Y 형식으로 표시되며, 여기서 "⇒"는 x a n d Y 사이의 상관 관계 특성에 대한 방향을 제공하기 위한 것입니다. 
	- 예를 들어, { B t er r , M i l k } ⇒ { B r e a d }는 그녀가 우유와 버터를 샀다는 것을 이미 알고 있기 때문에 Bread를 Mary에게 추천하는 데 매우 유용할 것입니다. 그러한 규칙의 강도는 그 신뢰도로 측정됩니다.
- --
**Definition 3.3.2 (Confidence)**

- 규칙 X ⇒ Y의 신뢰는 T 의 트랜잭션에 Y 가 포함되어 있을 조건부 확률이며 X 도 포함되어 있습니다. 따라서 신뢰는 X ∪ Y 의 지원을 X 의 지원으로 나누어 얻습니다.
- X ∪ Y 의 지원은 항상 X 의 지원보다 작습니다. 이는 트랜잭션에 X ∪ Y 가 포함되어 있으면 항상 X 가 포함되기 때문입니다. 
- 그러나 그 반대는 사실이 아닐 수도 있습니다. 따라서 규칙의 신뢰도는 항상 범위 (0, 1)에 있어야 합니다. 
- 신뢰도 값이 높을수록 항상 규칙의 강도가 높음을 나타냅니다. 
	- 예를 들어 규칙 X ⇒ Y가 참이면 특정 고객 세트가 X 품목 세트를 구매했다는 것을 알고 있는 판매자는 Y 품목 세트로 이러한 고객을 타겟팅할 수도 있습니다. 연관 규칙은 최소 지원 s 및 최소 신뢰 c를 기반으로 정의됩니다.

**Definition 3.3.3 (Association Rules)**

- 규칙 X ⇒ Y는 다음 두 조건이 충족되는 경우 최소 지지도 s와 최소 신뢰도 c에서 연관 규칙이라고 합니다.
1. X ∪Y의 지지도는 s 이상입니다.
2. X ⇒ Y의 신뢰도는 적어도 c입니다.

- 연관 규칙을 찾는 프로세스는 2단계 알고리즘입니다. - 첫 번째 단계에서는 최소 지원 임계값 s를 충족하는 모든 항목 집합이 결정됩니다. 
- 이 항목 집합 Z 각각에서 가능한 모든 2방향 분할(X, Z − X)을 사용하여 잠재적 규칙 X ⇒ Z − X를 생성합니다. 
- 최소 신뢰도를 충족하는 규칙은 유지됩니다. 
- 빈번한 항목 집합을 결정하는 첫 번째 단계는 특히 기본 트랜잭션 데이터베이스가 매우 큰 경우 계산 집약적인 단계입니다. 
- 효율적인 빈번한 항목 집합 발견 문제에 대해 계산적으로 효율적인 수많은 알고리즘이 사용되었습니다.

### 3.3.1 Leveraging Association Rules for Collaborative Filtering
- 연관 규칙은 단항 등급 매트릭스의 맥락에서 권장 사항을 수행하는 데 특히 유용합니다. 
-  단항 평가 매트릭스는 고객 활동(예: 구매 행동)에 의해 생성되며, 고객이 항목에 대한 선호도를 지정하는 자연스러운 메커니즘이 있지만 싫어요를 지정하는 메커니즘은 없습니다. 
- 이 경우 고객이 구매한 항목은 1로 설정되고 누락된 항목은 근사치로 0으로 설정됩니다. 결측값을 0으로 설정하면 예측에 편향이 발생할 수 있으므로 대부분의 등급 매트릭스 유형에서 일반적이지 않습니다.
-  그러나 이러한 경우 속성의 가장 일반적인 값은 일반적으로 0이기 때문에 일반적으로 희소 단항 행렬에서 허용되는 방식으로 간주됩니다. 
- 결과적으로 편향의 영향은 상대적으로 작으며 이제 행렬을 이진 데이터 세트로 취급할 수 있습니다.
- ---
**실제로 어떻게 어소시에이션 룰을 적용하는지**

- **규칙 기반 협업 필터링의 첫 번째 단계는 미리 지정된 minimum support(최소 지원)  및 minimum confidence(최소 신뢰도) 수준에서 모든 연관 규칙을 검색하는 것입니다.** 
- 최소 지원및 최소 신뢰도는 예측 정확도를 최대화하기 위해 조정된 매개변수
- 결과에 최소 하나의 item이 포함되어야 규칙이 유지됨
- 이 규칙 집합은 특정 사용자에 대한 권장 사항을 수행하는 데 사용할 수 있는 모델입니다. 
- 관련 상품을 추천하고자 하는 특정 고객 A를 생각해 보십시오. 
	- 첫 번째 단계는 고객 A가 실행한 모든 연결 규칙을 확인하는 것입니다. 
	- 규칙의 선행 항목에 있는 항목 집합이 해당 고객이 선호하는 항목의 하위 집합인 경우 연결 규칙은 고객 A가 실행했다고 합니다. 
	- 실행된 모든 규칙은 신뢰도를 내림차순으로 정렬됩니다. 
	- 이러한 정렬된 규칙의 결과에서 발견된 첫 번째 k개 항목은 고객 A에게 상위 k개 항목으로 권장됩니다.
- --
-  **연관 규칙은 단항 등급 매트릭스를 기반으로 하며, 이를 통해 좋아요를 지정하는 기능은 허용하지만 싫어요를 지정하는 기능은 허용하지 않습니다** 
	

**QUIZ**
1. 추천시스템에서 평점이 어떻게 주어진 경우에 연관규칙분석 모델이 유용할까요?
- 단항 데이터
-  고객이 경험했으면 1, 아니면 표기안하는 unary(단항) rating
-  페이스북 좋아요 예시, 좋아요 누르거나 안누르거나

2. 이러한 평점을 썻을 때 단점은 뭐가 있을까요?
- 단항등급 메트릭스, 좋아요를 누른거는 명확하게 좋아한다고 명시가 되지만, 안누른거는 싫어하는게 아니기 때문에 추천을 할 때 한계
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY3NjAyNTUxNiwtMTYxNzYxNTUxOCwtNj
A5NTE2Mjc4LDE0OTc5MTkyNDgsLTQ2NzQ0NjQ1MiwtMjA2NjE4
NDY3MCwtMTM3MDkzNTE1NCw3MDI1NDYyMjMsNDk3NzAxMjcxLD
cyMjMwOTkyOSw2MjMyMTY4NjMsLTE2OTY0MzUyNzAsNzMwOTk4
MTE2XX0=
-->