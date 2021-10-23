
-   추천시스템 기본
    
    -   content based filtering
    -   collaborative filtering
    -   hybrid model: 머신러닝에서 앙상블
-   content based filtering
    
    -   제품 중심 추천
    -   좋아하는 영화의 속성을 발견해서 새로운 영화를 추천
        -   배우, 감독, 장르 등등
-   collaborative filtering
    
    -   사용자의 행동 데이터를 기반으로 추천
    -   Nearest Neighborhood based
        -   item based( 더 많이 사용) : 내가 산 제품을 산 고객은 이러한 다른 제품을 샀다
        -   user based: 나와 비슷한 고객들은 이런 제품을 샀다.
    -   Latent factor based : 잠재요소기반
        -   매우 큰 행렬을 분해하는 방식
        -   maxtrix factorization: 유저가 아직 보지 못한 item 중 어떤 것을 좋아할지 예측하는 것 = sparse matrix의 빈칸을 채우겠다는 것
        -   user-item matix를 user matix와 item maxtix 로 나누어 표현 (행렬분해)
-   Implicit Feedback : log data (간접적으로 유저 판단)
    
-   Explicit Feedback : rating
    
-   Cold start problem : 유저가 오랜만에 들어오거나, 로그인하지않거나 하면 기존의 상호작용 데이터가 없어서 유저에게 새로운 아이템을 추천해주지 못하는 것
    
-   Collaborative Filtering
    
-   Content-Enriched Rec : 추천시스템에서 아이템의 정보와 유저의 정보의 상호작용
    
    -   유저의 정보를
    -   유튜브 영상의 색감, 음성
    -   다양한 딥러닝 모델들을 동원해서 추천시스템에서 활용가능
-   추천 시스템의 GOAL
    
    -   Relevance-유저가찾는걸추천해줘야한다.
        
    -   Novelty-유저가예전에안본걸추천해줘야한다.
        
    -   Serendipity-예상치못한걸추천해주면좋다.(배민에서내입맛에딱맞는음식점을갑자기추천해주는경우)
        
    -   Diversity-다양한물품을추천해줘야한다.
        
    -   CollaborativeFiltering
        
        -   memory-based:user-based,item-based
        -   model-based : latent factor based
    -   Content-based
        
        -   Knowledge-based
        -   Hybrid
        -   Evaluation

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1MzkzNjY0ODksMTI1NDEzNDA1MSw3Mz
A5OTgxMTZdfQ==
-->