# KoSplinter

![fig2](https://github.com/YunSoungKim/KoSplinter/assets/82452117/eaab23f2-d62b-4aac-b61c-0d675287194c)

[Splinter](https://arxiv.org/abs/2101.00438)는 recurring span selection 사전학습방법을 사용합니다.

위 그림과 같이 동일한 반복 구간 집합마다 하나의 구간을 남겨두고 나머지 모든 구간을 [QUESTION] 토큰으로 마스킹하는 방법입니다.

![fig3](https://github.com/YunSoungKim/KoSplinter/assets/82452117/33ab154c-e730-4b8e-9626-fe86ba1fce33)

Splinter는 Question Answering task에서 Fine-tuning을 할 때 위 그림처럼 질문 뒤에 [QUESTION] 토큰을 추가합니다.

사전학습과 Fine-tuning에서의 학습방법이 동일한 Splinter는 Question Answering에서 좋은 성능을 보여주며 데이터가 적을 때에 다른 모델들에 비해 더 좋은 성능을 보여줍니다.

Question Answering 모델을 만들어야했고 Fine-tuning을 할 학습데이터가 너무 적은 상황이여서 Splinter를 약 40GB로 학습하였습니다.

KoELECTRA처럼 편하게 사용할 수 있게 배포를 하고 싶었지만 Hugging Face에서 지원하는 splinter 모델과 original repository에 있는 모델의 구조가 조금 달라서 구현되어있는 ModelWithQASSHead를 통해 사용해야합니다.
