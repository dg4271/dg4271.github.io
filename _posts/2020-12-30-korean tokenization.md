---
title: korean tokenization
date:   2020-12-29 23:54:05 +0900
categories:
- Natural Language Processing
toc: true
toc_sticky: true
---

# 한국어 언어 모델과 토큰화 방법
> ㅇ
>
> ㅇ


## 관련 연구 조사
* 한국어 기계 독해를 위한 언어 모델의 효과적 토큰화 방법 탐구, samsung research, HCLT 2019
  * 기계 독해 모델에 최적인 토큰화 방법을 찾기 위해 음절, 어절, 형태소, BPE 등에 대한 성능 비교를 진행
  * 실험 결과, 어휘집 축소 효과 및 언어 모델의 퍼플렉시티 관점에서는 음절 단위 토큰화가 우수한 성능을 보임
  * 반면 토큰 자체의 의미 내포 능력이 중요한 기계 독해 과업의 경우 형태소 단위의 토큰화가 우수한 성능을 보임
  * BPE와 형태소 분절을 동시에 이용하는 방식이 가장 우수한 성능을 보임
    * 형태소 분석을 먼저 수행하고, BPE 적용 (최대 결합 규칙 수 50,000)
  * ![](/assets/img/tokenization_01.png) 
  * BPE만 사용한 경우 기계 독해 모델의 EM이 낮음.
    * "이는 조사나 문장 부호가 통상적으로 명사구나 서술어와 함께 등장하는 경우가 많아 공기 통계가 높아지게 되고, 이로 인해 BPE와 같은 통계 기반의 토큰화 방법론으로는 분절이 어려워지기 때문이라고 사료된다."


## 서브워드 토크나이저(Subword Tokenizer)
* OOV 문제를 완화하기 위해 단어를 더 작고 의미있는 여러 서브워드로 나누는 것
* OOV 문제 뿐만 아니라, 희귀 단어, 신조어 등의 문제도 완화할 것을 기대할 수 있음
* 참고
  * https://wikidocs.net/86649
### Byte Pair Encoding (BPE)
* BPE는 기본적으로 가장 많이 등장한 글자 쌍(Byte pair)을 찾아 하나로 병합하는 것을 반복하여, 문자열을 압축하는 알고리즘
* [논문](https://arxiv.org/pdf/1508.07909.pdf)에서 자연어 처리에 BPE를 도입하였음
* 모든 단어들을 글자(chracter) 단위로 분리한 후, 정해진 수 만큼 반복하면서 가장 빈도수가 높은 유니그램 쌍을 하나의 유니그램으로 통합함
  * 이 과정에서 생성되는 유니그램들을 vocalbulary를 계속해서 업데이트 함
### Wordpiece Model
* [구글의 논문](https://arxiv.org/pdf/1609.08144.pdf)에서 wordpiece model을 사용함
* BPE가 빈도수에 기반하여 가장 많이 등장한 쌍을 병합하는 것과는 다르게, 병합되었을 때 코퍼스의 likelihood를 가장 높도록 만드는 쌍을 병합함 
* BERT를 훈련하기 위해 사용되기도 하였음
  * huggingface의 [tokenizers](https://huggingface.co/docs/tokenizers/python/latest/pipeline.html?highlight=wordpiece#all-together-a-bert-tokenizer-from-scratch) 라이브러리에 구현되어 있음
* 예시
  * 원래 문장: Jet makers feud over seat width with big orders at stake
  * WPM 수행 후: _J et _makers _fe ud _over _seat _width _with _big _orders _at _stake
  * WPM 문장을 다시 복원하는 방법: 모든 띄어쓰기를 제거하고, 언더바를 띄어쓰기로 치환 
### SentencePiece
* BPE, Unigram Language Model Tokenizer를 구현한 코드
  * [paper](https://arxiv.org/pdf/1808.06226.pdf), [github](https://github.com/google/sentencepiece)
  * SentencePiece를 개발한 사람은 mecab과 cabocha를 만들기도 했음..
* 사전 토큰화 작업(pretokenization) 없이 raw data에 대해 바로 토큰화를 수행함 (언어에 종속되지 않음)  
### SubwordTextEncoder
* Tensorflow를 통해 사용할 수 있는 Wordpiece model 기반 서브워드 토크나이저
  * [Tensorflow API](https://www.tensorflow.org/datasets/api_docs/python/tfds/deprecated/text/SubwordTextEncoder)