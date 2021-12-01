# AutoRec: Autoencoders Meet Collaborative Filtering

## 오토인코더란?
- Auto Encoder는 동일한 입, 출력 구조를 사용해 압축된 latent representation을 학습할 수 있는 비지도 학습 모델
- Auto Encoder는 Encoder와 Decoder로 구성되어 서로 대칭을 이루는 구조
- 학습은 일반적으로 모델 Output과 Input 사이의 RMSE를 최소화하는 방향으로 진행
- 학습이 완료된 Auto Encoder는 분리를 통해 다양하게 쓰임
	- Encoder : 저차원 Latent space 로의 임베딩을 위해 사용
	- Decoder: generative model 로 사용
	-
## Abstract
- 협업 필터링(CF)를 위한 auto encoder framework인 AutoRec 소개
- Movielens 및 Netflix 데이터 세트에서 biased matrix factorization, RBMCF and LLORMA 들보다 성능(Representation, Complexity)이 좋음


#### **Model architecture**
- 일반적인 Latent Factor 모델과 달리, AutoRec은 아이템, 유저 중 하나에 대한 임베딩만을 진행합니다.
- 저자들은 아이템을 임베딩하는 Item-based 구조를 I-AutoRec, 유저를 임베딩하는 user-based 구조를 U-AutoRec으로 명명하였습니다.
- item i,  I = {1...n}
- user u, U =  {1...m}|
- item i에 대한 rating r, R = {R1i ... Rmi}
- AutoRec 은 각각의 아이템 i 에 대한  ratings vector r 을 auto-encoder의 input 으로 합니다.
- 일반적인 auto encoder 와 동일하게 autoRec 은 Encoder & Decoder reconstruction 과정을 수행합니다.
- 최종 목적: 파라미터 예측


![](https://blog.kakaocdn.net/dn/tDCRC/btrahiYo3ig/7ofk2u7FNCFeBiANcRA53k/img.png)

위의 그림은 하나의 아이템에 대한 AutoRec 구조를 보여줍니다.  하지만 n개 만큼의 AutoRec 구조가 존재하는 것은 아닙니다. 모든 개별적인 아이템에 대한 AutoRec은 파라미터를 공유하는 하나의 모델입니다.

#### **Training**

AutoRec은 일반적인 Auto Encoder와 동일하게 RMSE를 최소화 하는 방향으로 학습을 진행합니다.

#### **Comparison**
- MF 와 차이
	- MF는 SVD와 유사한 방법으로 각기 다른 차원의 User, Item Matrix를 동시에 Latent space로 매핑하는 반면, AutoRec은 I-AutoRec, U-AutoRec 각각의 형태에서 유저와 아이템 중 하나만을 매핑합니다.
	- MF가 Linear한 저차원의 Latent representation과 Interaction을 학습하는 반면, AutoRec은 non-linear한 Activation Function을 사용하여 복잡한 non-linear latent representation을 학습할 수 있습니다.

## Results
- 모델 검증을 위한 과정에는 Movielens dataset과 Netflix dataset이 사용
![](https://blog.kakaocdn.net/dn/bmJb4N/btracLtAeKL/9f55l0GYc8N7iRyba4edd0/img.png)

1. User based 모델과 Item based 모델의 성능 비교
비교모델과 AutoRec 모두 Item based 모델의 성능이 좋았음을 알 수 있습니다.

이는 rating per user보다 rating per Item이 상대적으로 높은 경우가 많았기 때문이라고 합니다.

또한 I-AutoRec의 성능이 가장 좋았음을 확인할 수 있습니다.

![](https://blog.kakaocdn.net/dn/b1Tfqa/btraeyOcfbw/vXK0L4kQJcD8n93f225nhk/img.png)
2. 두번째 실험은 비 선형의 latent feature를 학습하는게 정말 도움이 되었는지를 검증하는 실험입니다.

hidden layer에 비 선형성을 추가시켜주는 sigmoid  g(⋅)g(⋅)의 사용이 AutoRec의 성능에 큰 영향을 준다는 사실을 알 수 있습니다.

![](https://blog.kakaocdn.net/dn/PDRaM/btragL7dvx1/A92dXKjNwUzJOw6sr1gTlk/img.png)
- 당시 쓰이던 모든 Baseline들과의 성능 비교입니다. I-AutoRec이 가장 낮은 RMSE를 달성하였습니다.

![](https://blog.kakaocdn.net/dn/OKZ0c/btradSzmwXQ/7MQvnnYz1jL4VRKM7A0iR0/img.png)

히든 유닛의 수에 따른 RMSE의 성능을 비교해 보았습니다.

깊게 AutoRec을 만드는 것이 성능 향상에 도움을 준다는 사실을 말해주고 있습니다.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUxNTYyMTA5NSwyNzQ1OTYxOSwtMTQ3ND
E3NzMxMiw0MzI0NDE2NzUsODY1OTM3NTc0LDczMDk5ODExNl19

-->