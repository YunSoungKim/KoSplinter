# KoSplinter

Question Answering Task에서 많은 모델들은 몇 만개의 데이터를 이용해 Fine-tuning하여 좋은 성능을 내고 있습니다.

하지만 실제 업무에서는  질문답변 형식으로 몇 만개의 데이터를 레이블링 하는 것이 현실적으로 어렵습니다.

Splinter는 데이터 많지 않을 때에도 좋은 성능을 보여주는 장점이 있습니다.

KoSpliter는 [original repository](https://github.com/oriram/splinter)에 있는 코드를 사용해 약 40GB의 한국어 text로 사전학습을 진행하였습니다.

[KoELECTRA](https://github.com/monologg/KoELECTRA)처럼 편하게 사용할 수 있게 배포를 하고 싶었지만 Hugging Face에서 지원하는 [Splinter](https://huggingface.co/docs/transformers/model_doc/splinter) 모델과 [original repository](https://github.com/oriram/splinter)에 있는 모델의 구조가 조금 달라서 구현되어 있는 코드(finetuning/modeling.py)를 통해 사용해야 합니다.

사용하기 다소 불편하더라도 다른 분들께 조금이라도 도움이 될까 싶어 올립니다.

[여기](https://drive.google.com/drive/folders/1_tzp1N32Us18InlEw0ycV7hNqdrY6pbe?usp=drive_link)에서 모델과 데이터를 받으실 수 있습니다.

감사합니다:)

## About Splinter

[Splinter](https://arxiv.org/abs/2101.00438)는 Question Answering에 특화된 BERT 기반 모델입니다.

데이터의 수가 적을 때에도 다른 모델에 비해 더 좋은 성능을 보여줍니다.

![fig2](https://github.com/YunSoungKim/KoSplinter/assets/82452117/eaab23f2-d62b-4aac-b61c-0d675287194c)

[Splinter](https://arxiv.org/abs/2101.00438)는 recurring span selection 사전학습방법을 사용합니다.

위 그림과 같이 동일한 반복 구간 집합마다 하나의 구간을 남겨두고 나머지 모든 구간을 [QUESTION] 토큰으로 마스킹하는 방법입니다.

![fig3](https://github.com/YunSoungKim/KoSplinter/assets/82452117/33ab154c-e730-4b8e-9626-fe86ba1fce33)

[Splinter](https://arxiv.org/abs/2101.00438)는 Question Answering task에서 Fine-tuning을 할 때 위 그림처럼 질문 뒤에 [QUESTION] 토큰을 추가합니다.

사전학습과 Fine-tuning에서의 학습방법이 동일한 Splinter는 Question Answering에서 좋은 성능을 보여주며 데이터가 적을 때에 다른 모델들에 비해 더 좋은 성능을 보여줍니다.

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

- 약 40GB를 사용했습니다.

- 마스킹할 때 [Splinter](https://arxiv.org/abs/2101.00438)에 있는 구문에 대한 4가지 정의를 사용하고 추가로 이전 말뭉치들에서 사용한 구간은 다시 사용하지 않았습니다.

- [KoELECTRA](https://github.com/monologg/KoELECTRA)의 vocabulary를 사용했습니다.

- Params : 110M

- Train steps는 4.5M이며 그 외 Hyperparameter는 [Splinter](https://arxiv.org/abs/2101.00438) 논문과 동일합니다.

- Google Cloud Platform의 TPU v2-8을 이용하여 학습했습니다. 약 8일 정도 소요되었습니다.

## Fine-tuning

- KorQuAD 1.0을 사용했습니다.

- 데이터가 적을 때에도 성능을 확인하기 위해서 Training set에서 크기가 각각 16, 32, 64, 128, 256, 512, 1024인 표본을 무작위로 추출하는 것을 3번 반복했습니다. 그 후 동일한 크기의 Training set으로 학습한 모델을 Dev set으로 평가한 평균 점수를 계산하였습니다.

- 비교 모델로는 [KoELECTRA-Base-v3](https://github.com/monologg/KoELECTRA)를 사용하였습니다.

- 전체 Training set에 대한 KoELECTRA의 성능은 https://github.com/monologg/KoELECTRA 에 있는 성능 결과를 가져왔습니다.

| Model                |       | 16    | 32    | 64    | 128   | 256   | 512   | 1024  | Full  |
|:---------------------|:------|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
| `KoSplinter`         |   EM  | 33.67 | 49.71 | 59.60 | 67.49 | 72.51 | 76.86 | 79.47 | 86.16 |
|                      |   F1  | 46.22 | 63.38 | 72.77 | 80.64 | 84.48 | 87.88 | 89.91 | 94.34 |
| `KoELECTRA-Base-v3`  |   EM  | 10.24 | 20.70 | 34.23 | 51.50 | 60.55 | 67.90 | 74.19 | 84.83 |
|                      |   F1  | 20.54 | 36.19 | 47.97 | 66.11 | 74.23 | 80.39 | 85.22 | 93.45 |


