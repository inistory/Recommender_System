
# LightGCN: Simplifying and Powering Graph Convolution Network for Recommendation

## Abstract

NGCF(Neural Graph Collaborative Filtering) 라는 연구를의 단점들을 지적하면서, ngcf보다 개선된 모델, 더 가볍고 학습이 쉬운 모델을 제시하는 연구입니다.

### Why GCN works well for CF?

-   GCN을 추천에 쓰려는 시도 -> NGCF
-   GCN의 어떤 점 때문에 CF에서 작동을 잘 하는건지 들여다봄

## RELIMINARIES

### NGCF(Neural Graph Collaborative Filtering)

-   유저와 아이템 인터렉션 데이터를 가지고 유저임베딩, 아이템임베딩을 만듦
-   유저임베딩 생성 시에는 유저의 이전 레이어에서의 임베딩과 유저의 neighborhood에 있는 아이템의 임베딩을 인터렉션하고 더하고, 그렇게 만든 임베딩을 어그리게이션해서 sumation해서, 그 다음 레이어의 임베딩을 만드는 방식

### [component 1]

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/721ff1b2-5849-4706-8c9a-8fc5d5f13649/Untitled.png)

1.  self connection

-   다음 레이어에 임베딩을 만들 때 자기자신이 포함이 됨
-   첫번째 텀에서는 자기자신의 임베딩이 포함
-   뒷 텀에서는 이웃아이템의 임베딩과의 element wise multiplication을 통해서 임베딩을 생성하기도 함

1.  Aggregation components

-   이웃의 임베딩들을 임베딩
-   유저의 이웃에 있는 아이템들의 임베딩을 활용하는 부분

1.  Normalization

-   아이템 임베딩과 유저 임베딩의 상호작용을 모델링한다음 그대로 사용하지 않고 Normalization 한다.
-   user의 degree의 값을 루트 역수를 씌워서 쓰고, 아이템 degree의 역수의 루트 값, 이 두 값을 곱해서 Normlization 한다.

### [component 2]

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5ab3d1d4-292a-4162-918a-77a94ecb7e29/Untitled.png)

1.  Featur Transformation

-   이전 레이어에서 생성된 임베딩을 그대로 쓰는 것이 아니고 weight matrix를 곱하여 이전에 생성된 임베딩을 변형(transformation) 을 시켜줌

1.  Non Linear Activation

-   이렇게 만들어진 값을 Non Linear Activation 을 거쳐서 다음 레이어의 임베딩으로 만든다.

결론 ⇒ NGCF는 Semi-Supervised Classification with Graph Convolutional Networks 논문의 구조를 그대로 반영, 하지만 이 논문과 추천에서의 상황은 비슷하지 않을 수 있다.

⇒ Semi-Supervised Classification with Graph Convolutional Networks 논문>

-   task 자체가 node classification task
-   사용한 데이터셋이 논문들끼리의 citation dataset
    -   노드를 representation할 때, feature로 쓰는게 다양: 논문의 제목, abstract에 있는 words들을 feature로 사용
-   이걸 그래도 추천에 적용해서 NGCF라는 논문을 냈는데, 상황이 많이 다르다!

## EXPERIMENTS

### 1. Ablation Study

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cf747ecc-98bc-4e94-9c37-38b59cab4f97/Untitled.png)

-   불필요한 NGCF의 컴포넌트들을 빼보면서 성능이 어떻게 변하는 가를 살펴보았다.
-   집중한 컴포넌트들: non-linear activation, feature transformation
-   NGDF-n : w/o Non-Linear Activation
-   NFCF-f : w/o Feature Transformation
-   NGCF-fn: w/o Non-Linear + w/o Feature Transformation
-   결론: 둘다 뺐을 때, 성능이 좋아짐
-   알고리즘을 추천에 적용할 때는 ablation을 통해 컨포넌트의 효과를 점검해야할 필요가 있다. 다른 분야에서 만들어진 것을 그대로 쓰는게 안 좋을 수 있기 때문
-   **LightGCN : GCN에서 Essential한 컨포넌트만 가져와서 추천에 사용하자!**

### NGCF 와 Light GCN 비교

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5c91c113-d124-4b80-87c0-b25a23590562/Untitled.png)

-   w/o Non-linear activation: sigma가 사라짐
-   w/o Feature Transformation : W1, W2가 사라짐
-   selt connection도 사라짐, 왜? 임베딩을 light하게 바꿈

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/510f70aa-dba5-451a-a89b-50f76449a7c9/Untitled.png)

-   최종 임베딩을 만들때, 맨 처음 임베딩과 L 번째 GCN을 거쳐서 나온 임베딩을 Concat해서 최종으로 임베딩을 만듦
-   concat대신 weighted sum을 함 : 이렇게하면 self connection의 효과를 capture한다.

## 모르는 것

1.  self connection 이란?
2.  loss 확인은 왜 Train, test에서 둘 다 해봐야할까? 그렇다면 validation은?
3.  self connection의 효과?

## References

-   [](https://arxiv.org/pdf/2002.02126.pdf)[https://arxiv.org/pdf/2002.02126.pdf](https://arxiv.org/pdf/2002.02126.pdf)
-   [](https://www.youtube.com/watch?v=5Wy-DL6tdkU)[https://www.youtube.com/watch?v=5Wy-DL6tdkU](https://www.youtube.com/watch?v=5Wy-DL6tdkU)
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1NDEyODc2NTksLTEyNDQ0MjcyLC0xND
U5NDY4NjY1LDE2NTI3NDg4NTgsOTUxNjUxNTc3LC04NzE1ODIy
MzEsMzc0MTIwMzE0LDczMDk5ODExNl19
-->