

# Matrix factorization techniques for recommender systems


- Netflix Prize 대회 덕분에 유명해짐
- Matrix Factorization은 유저-아이템 상호작용에는 유저의 행동, 아이템 평점에 영향을 주는 잠재된 요인(latent factor)이 있을텐데 그 잠재된 요인을 고려하여 유저에게 적합한 아이템을 추천하는 방법
- MF
- 예전에는 유저와 아이템 간의 관계를 1차원 적 으로 생각했음 (유저- 아이템 행렬, m x n)
- 이를 좀 더 쉽게 표현하기 위해 인수분해를 쓰는 방식인 MF 
- 유저-잠재요인(m x k), 아이템-잠재요인(k x n) 으로 행렬을 인수분해해서 값을 추정함



![](https://blog.kakaocdn.net/dn/HyxOt/btrai8udf7O/Rf2rHocxWxvst2XYoJi2w1/img.png)
MF는 SVD와 유사한 방법으로 각기 다른 차원의 User, Item Matrix를 동시에 Latent space로 매핑합니다.

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1Nzc2NTc0MTMsLTE1NzU1NzQxNDgsNz
MwOTk4MTE2XX0=
-->