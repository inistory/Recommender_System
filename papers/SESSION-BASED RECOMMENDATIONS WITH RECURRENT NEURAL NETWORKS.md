
# SESSION-BASED RECOMMENDATIONS WITH RECURRENT NEURAL NETWORKS

- [SESSION-BASED RECOMMENDATIONS WITH RECURRENT NEURAL NETWORKS](https://arxiv.org/pdf/1511.06939v4.pdf)
![content img](https://d3s0tskafalll9.cloudfront.net/media/images/model.max-800x600.png)
- 여러 RNN 계열의 모델(e.g. LSTM)이 있겠지만 저자가 실험해본 결과 GRU의 성능이 제일 좋았다
- Embedding Layer를 사용하지 않았을 때가 사용했을 때보다 성능이 좋았다
- Session-Parallel Mini-Batches를 제안:  Session을 순차적으로 계산하는 것이 아니라, 이 전 Session이 끝날 때까지 기다리지 않고 병렬적으로 계산
- Mini-Batch의 shape은 (3, 1, 1)
- RNN cell의 state가 1개
- Tensorflow 기준으로 RNN을 만들 때 stateful=True 옵션을 사용
- 세션이 끝나면 state를 0으로 만들어줌
- **SAMPLING ON THE OUTPUT**  : Negative Sampling와 같은 개념, Item의 수가 많기 때문에 Loss를 계산할 때 모든 아이템을 비교하지 않고 인기도를 고려하여 Sampling

- **Ranking Loss**  : Session-Based Recommendation Task를 여러 아이템 중 다음 아이템이 무엇인지 Classification하는 Task로 생각할 수도  있고, 여러 아이템을 관련도 순으로 랭킹을 매겨서 높은 랭킹의 아이템을 추천하는 Task로도 생각할 수 있음
본 논문에서는 Ranking을 맞추는 objective function Loss를 사용함

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTE1ODYzOTQ4Niw3MzA5OTgxMTZdfQ==
-->