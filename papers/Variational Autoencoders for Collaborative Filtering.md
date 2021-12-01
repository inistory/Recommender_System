
# Variational Autoencoders for Collaborative Filtering

##  Abstract
  

이 비선형 확률 모델은  선형 요인 모델의 제한된 모델링 능력을 넘어서기 위해  여전히 대부분의 협업 필터링 연구를 지배하고 있습니다.


## 질문
1. 선형변환이란
MF  자체가 비선형이 없음. 그냥 인풋이랑 웨이트랑 곱하는건 행렬곱인것 처럼 두개의 메트릭스를 곱할 때는 비선형성이 없다.

2. 베이지안

bag of words -> 아이템의 텍스트 정보를 단어들의 집합
유저별로 하나
Rating matrix 안에서의 한 행

아이템정보를 활용할때 텍스트 정보를 활용
양념감자 + 소스 


3. 다항분포란?
- 좋고, 싫음 -> 이항분포
-  평점 1~5점 -> 다항분포


벡터는 i-1 차원이다. 공간적인 차원을 일반화하기 위해서 이렇게 씀

## Model
- 1번식 :  학습시키기 위해서 인코더에게 줄 때 제약조건을 줌. 몇가지 식을 주는데 그 식을 정규분포로 보겠다. 멀티노미얼 모수 두개( 값, 값에 대한 확률)
- 2번식: 비선형함수에 들어갈 확률을 파이제트유 거기에서 에스유를 뽑겠다??? 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTI4NjMyNjIwOCwzMDI3MzM4ODksLTIxOT
E4MjQ3MiwtMTg2NjEzNzE2NCw1MTYxMzA2NzMsMTAzMjMxNTcz
MywtNjc5MDI2OTg1LDczMDk5ODExNl19
-->