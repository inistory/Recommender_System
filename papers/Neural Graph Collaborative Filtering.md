
# Neural Graph Collaborative Filtering

## 1. Abstract
- MF의 한계
: 유저와 아이템의 관계를 리니어하게 1차원 레이어에서 담아내기가 어려웠음
: 잠재요인들을 리니어하게 결합하는 내적을 사용하기 때문에 유저 아이템 상호작용 데이터의 복잡한 구조를 알아내기가 너무 어려움
: 새로운 유저가 나타나면 저차원 공간에 이를 표현하기가 어렵다는 한계 존재

- Neural collaborative Filtering은 collaborative filtering에 Deep Neural Network (딥러닝)를 사용하여 유저-아이템 상호작용을 학습하는 프레임워크를 제안
- 유저-아이템 간의 복잡한 상호작용을 임베딩 공간에 표현하기 위해 non-linear한 요소를 표현할 수 있는 딥러닝을 제안
	-	MF (Linear)
	-	Deep Learning (Non-Linear)

## 2. Related work
1) Embedding Layer
- 콜라보레이티브 필터링 세팅에 중점을 두었기 때문에 유저와 아이템의 ID만 입력 피처로 사용하여 희소벡터로 변환함
-  이렇게 획득한 유저 임베딩은 Latent Factor Model에서 유저에 대한 Latent 벡터를 볼 수 있게 됨
2) Neural CF Layer
- 유저 임베딩과 아이템 임베딩은 잠재벡터를 예측값에 매핑하기 위해 Multi Layer Neural Architecture 로 구성된 Neural self layer에 입력
- 뉴럴넷을 사용한 CF는 레이어마다 유저 아이템 사용작용의 특정 잠재구조를 발견할 수 있도록 커스터마이징 가능: 논문에서는 Neural CF layer에 Multi Layer Perceptron을 사용
- 레이어 마지막에 있는 레이어 X의 차원은 모델의 capability를 결정하는 부분
3) Neural Matrix Factorization
- 앞서 만든 Neural CF 와 generalized MF과 통합한 모델
- generalized MF : 리니어하고 고정된 특징으로 인해 유저-아이템 간의 복잡한 관계를 잘 표현하지 못함
- Neural CF 는 Non-linear 하고 Flexible 한 특성을 지니고 있기 때문에 복잡한 관계를 표현할 수 있게 됨
- 서로의 장점을 살리며 부족한 부분을 채움
- 특징: 각 모델이 서로다른 임베딩 레이어를 사용한다는 점, 다른 피처없이 딥러닝을 잘 사용해서 학습했다는 점에서 큰 의의

유저, 아이템 간의 복잡한 관계까지 표현할 수 있는 모델이 나왔지만, 유저, 아이템 각각 임베딩하는 기존의 방식은 결과적으로 행렬구조이기 때문에 유저-아이템 간의 상호작용을 더 잘 나타내기에는 부족

=> 그래서 Neural Graph Collaborative Filtering 이 나옴

[정리]
- 이전까지는 유저-아이템 상호작용 그래프 형태
- 뉴럴 그래프 CF 에서는 이를 고차원 그래프 형태로 나타냄 

## 3. Architecture
1) Embedding layer
- 일반적인 협업 필터링 모델처럼 각각의 유저와 아이템에 대해 임베딩하는 레이어
2) Embedding Propagation Layers
- 가장 중요한 레이어, 기존논문과 가장 차별되는 부분
- 그래프 구조를 따라 협업 시그널을 캡처하고 유저와 아이템 임베딩을 개선하기 위해 그래프 뉴럴넷에 메세지 전달 아키텍처를 구축하는 레이어
- First-order Propagation (1차원 프로파게이션)
	- 유저가 아이템을 소비할 때, 소비된 아이템 정보가 유저에게 전달됨
	- 특정 유저의 아이템 소비정보는 아이템 피처로 사용됨
	- 이를 토대로 아이템 또는 유저의 유사성을 측정할 수 있음
-주요 작업 1) Message Construction
	- 아이템 정보가 유저에게 전달되는 메세지를 구성
	- 아이템 자체 어떤 정보가 유저에게 전달되는 메세지를 구성
	- 아이템에서 유저로 정보가 전달되는 메세지 값을 의미
	- 아이템에서 어떤 정보가 유저에게 전달? 협업 시그널 잠재요인, 레이턴트 팩터이기 때문에 알순 없음
	- 메세지의 구성: 아이템 해당 자체의 값 +  아이템과 유저의 연관성을 전달되는 값으로 구성
-주요 작업 2) Message Aggregation
	- 이렇게 construction 된 수많은 메세지들을 aggregation
	- 각각의 layer 마다 임베딩 전파 계층 이후 얻은 유저의 표현 또는 아이템의 표현을 수식으로 나타냄

[요약] 
- Embedding Propagation Layers 는 유저와 아이템의 표현을 관련 짓기 위해 주변 유저와 아이템에 1차 연결정보를 명시적으로 활용하는 것
- 그래프에서 첫번째 레이어에 해당하는 노드, 엣지 생성한 후, 이를 Embedding Propagation Layers를 통해 다음 레이어로 인코딩하는 형식

- high-order Propagation(고차원 연결성 그래프)
	-	Embedding Propagation Layerd을 여러개 쌓은 것
	-	이를 통해 상위 연결정보를 탐색 할 수 있음
	-	high-orde connectivity 는 유저와 아이템간의 관련도를 추정하기 위해 잠재요인을 인코딩하는 일에 중요한 역할을 함

3) Model Prediction Layer
- Embedding Propagation Layers 에서 나온 표현은 각기 다른 메세지를 강조하기 때문에 유저 선호도 반영에 서로 다른 기여를 하게 됨
- 따라서 이들을 마지막으로 concatenate 하여 유저 또는 아이템을 위한 최종 임베딩을 구성하게 되는 레이어

4) Output Layer
- 타겟 아이템에 대한 유저의 선호도를 추정
- doc product 사용
- 유저는 선호도가 가장 높은 아이템을 추천받게 됨
References
- https://arxiv.org/pdf/1905.08108.pdf
- https://www.youtube.com/watch?v=ce0LrvVblCU

## 4. Experiments & Result
[실험1]
- 세 가지 종류의 데이터 사용
- train, test = 8:2
- 평가지표: recall(실제로 유저가 관심 있는 아이템 중 추천된 아이템의 비율을 평가하는지표), ndcj(추천에서 가중치를 주어서 성능을 평가하는 지표)
-  베이스라인 모델: MF, NeuralMF, SOTA 모델
- 여러 Embedding Propagation Layers 를 쌓아서 고차원 연결성을 탐색할 수 있지만, CMN, GC-MC 는 일차원 아웃풋 만을 활용한 모델이기 때문에 연결성 확인 불가 => 임베딩에서 협업 시그널을 캡처하는 것이 중요
	- PinSage: Multi-grained 표현을 고려하는 NGCF모델이 성능이 우수

[실험2]Embedding Propagation Layers 가 성능에 미치는 영향?
	- 레이어 갯수를 늘릴 수록 recall, ndcg가 증가
	- 추천이 더 잘됨
	- 하지만 yelp의 경우 오버피팅 발생
	 
[실험3] 오버피팅 방지 역할을 하는 두 가지 Dropout의 효과
- Node dropout : 렌담 셀렉션으로 모드를 선택해 제거하고, 그 노드와 관련된 메세지 역시 전부 삭제하는 드롭아웃
- message dropout : 랜덤 셀렉션으로 선택은 하지만 메세지만 삭제하는 드롭아웃
=> Node dropout을 채택 (성능이 더 좋음)

[실험4] Epoch의 recall 에 대한 테스트 성능
- NGCF가 MF 보다 recall이  빠르게 수렴
- NGCF가 모델케파가 좋고 임베딩 공간에서 임베딩 전파를 수행할 때 나타나는 효과가 훨씬 좋기 때문에 성능이 좋다.

[실험5] Embedding Propagation Layer의 깊이가 임베딩 공간에서 표현 학습에 어떤 영향을 주는지
- 별: 유저
- 동그라미: 아이템
- 유저 아이템 연결성은 임베딩 공간에서 서로 가까운 부분에 임베딩
- NGCF가 깊어질수록 과거아이템의 임베딩이 더 가까운 경향 (클러스터링이 훨씬 잘됨)

## 5. Coclusion
- Embedding Propagation Layer 에서 Message Construction과 Message Aggregation과정을 통해 High-order connectivity 그래프를 만든 후, 아웃풋을 하나의 벡터로 표현하여 유저-아이템 선호도를 추정할 수 있는 방안을 제안
- 유저와 가까이에 있는 아이템들이 유저와의 유사도가 높다는 가정하에 추천받을 수 있는 구조

<!--stackedit_data:
eyJoaXN0b3J5IjpbOTk4MTQxNjMsNTY4MzU4Mzk5LC05Nzk1MD
I2NDEsLTM4MjUwMDE3NCwtOTkzMzAxODQ2LC0xMDU4ODMyNDM5
LDQ1MDM0NjY4MywyNDMwNzcxNjUsLTE2MTQzNDAwMjQsNzMwOT
k4MTE2XX0=
-->