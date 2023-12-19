# KoSplinter

![fig2](https://github.com/YunSoungKim/KoSplinter/assets/82452117/eaab23f2-d62b-4aac-b61c-0d675287194c)

[Splinter](https://arxiv.org/abs/2101.00438)는 recurring span selection 사전학습방법을 사용합니다.

위 그림과 같이 동일한 반복 구간 집합마다 하나의 구간을 남겨두고 나머지 모든 구간을 [QUESTION] 토큰으로 마스킹하는 방법입니다.

![fig3](https://github.com/YunSoungKim/KoSplinter/assets/82452117/33ab154c-e730-4b8e-9626-fe86ba1fce33)

Splinter는 Question Answering task에서 Fine-tuning을 할 때 위 그림처럼 질문 뒤에 [QUESTION] 토큰을 추가합니다.

사전학습과 Fine-tuning에서의 학습방법이 동일한 Splinter는 Question Answering에서 좋은 성능을 보여주며 데이터가 적을 때에 다른 모델들에 비해 더 좋은 성능을 보여줍니다.

Question Answering 모델을 만들어야했고 Fine-tuning을 할 학습데이터가 너무 적은 상황이여서 Splinter를 약 40GB로 학습하였습니다.

KoELECTRA처럼 편하게 사용할 수 있게 배포를 하고 싶었지만 Hugging Face에서 지원하는 splinter 모델과 original repository에 있는 모델의 구조가 조금 달라서 구현되어있는 ModelWithQASSHead를 통해 사용해야합니다.

사용하기 조금 불편하더라도 다른 분들께 조금이라도 도움이 될까 싶어 올립니다.

## Pretraining

### Data

#### 모두의 말뭉치

- 신문말뭉치

- 신문말뭉치 2020

- 신문말뭉치 2021

- 신문말뭉치 2022

- 문어말뭉치

- 구어말뭉치

- 국회 회의록 말뭉치

- 메신저말뭉치

- 일상대화 말뭉치 2020

#### Ai Hub

- 전문분야 말뭉치

- 대규모 구매도서 기반 한국어 말뭉치 데이터

### Details

- 마스킹할 때 이전 말뭉치들에서 사용한 구간은 다시 사용하지 않았습니다.

- KoELECTRA의 vocabulary를 사용했습니다.

- Train steps는 4.5M이며 그 외 Hyperparameter는 Splinter 논문과 동일합니다.

- Google Cloud Platform의 TPU v2-8을 이용하여 학습했습니다. 약 8일 정도 소요되었습니다.

## Fine-tuning
