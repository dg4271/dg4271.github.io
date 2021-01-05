---
title: Dense Passage Retrieval for Open-Domain Question Answering
categories:
- Question Answering
toc: true
toc_sticky: true
use_math: true
---

# Dense Passage Retrieval for Open-Domain Question Answering
> paper: [https://arxiv.org/pdf/2004.04906.pdf](https://arxiv.org/pdf/2004.04906.pdf)<br>
> code: [https://github.com/facebookresearch/DPR](https://github.com/facebookresearch/DPR)<br>
> hugging face: [https://huggingface.co/transformers/model_doc/dpr.html](https://huggingface.co/transformers/model_doc/dpr.html)<br>


## Introduction
질문과 문서가 주어졌을 때 질문에 대한 답을 문서에서 찾아주는 기계 독해 태스크는 BERT의 등장 이후로 사람을 뛰어넘는 성능을 보여주고 있다. 이후 문서의 입력 없이 질의만을 입력하여 문서를 직접 찾고 기계 독해 모델로 답을 찾는 질의 응답을 하고자하는 시도들이 자연스레 등장하게 되었다. 이러한 Open-domain quesiton answering의 연구들은 Retriever와 Reader를 결합한 two-stage framework를 갖는다. 초기 연구들은 sparse vector space model인 TF-IDF와 BM25등을 이용한 retriever를 사용한다. 이번에 소개하는 연구인 DPR은 dense vector를 사용한 retirever를 제안하며 기존 sparse retriever보다 top-20 passage retrieval accuracy를 9%~19% 개선하며 open-QA에서도 SOTA 성능을 보여주었다.

기존 sparse vector model은 동의어 검색에 취약하다. DPR 논문에서 ***"Who is the bad guy in lord of the rings?"*** 라는 질문에 답을 ***"Sala Baker is best known for portraying the villain Sauron in the Lord of the Rings trilogy."*** 라는 passage에서 찾는 상황을 예로 들며 "bad guy"와 "villain" 간의 의미적 유사성을 vector model이 판단할 수 있어야 함을 설명했다. Dense representation의 장점은 이것 말고도 task-specific representation으로 학습 가능하다는 점을 들었다. 단점은 학습에 대량의 question-context pair 데이터 셋이 필요하다는 것과, 계산 비용이 상대적으로 크다는 것이 있다. 후자의 경우에는 최근의 maximum inner product search(MIPS) 연구들의 방법들을 사용하면 효과적인 dense vector search를 할 수 있다고 한다.

Dense retrieval 방법은 open-domain QA 태스크에서 sparse retrieval 방법보다 효과적이지 못하다가, [ORQA]() 연구에서 inverse cloze task(ICT)를 제안하고 이용하면서 더 효과적인 방법으로 자리잡기 시작하였다. DPR 논문은 ICT 사전학습보다 더 개선된 dense embedding 모델을 제안하려는 연구이다.

DPR에서 제안하는 방법은 embedding이 question과 relevant passage의 vector간의 inner product를 maximizing하도록 학습한다는 개념이다. DPR의 contribution 2가지이다. 첫번째는 추가적인 pretraning 없이 기존 모델에 question & passage encoder를 fine-tuning 하는 것으로 성능을 올릴 수 있음을 보인 것. 두번쨰는 retirever의 성능을 높이는 것이 open-domain QA의 최종 accuracy를 올릴 수 있음을 보인 것 (당연한 거 아닌가?)


## Dense Passage Retriever (DPR)
DPR의 목적은 기계 독해 모델이 질문에 대한 답을 찾을 수 있는 ***k***개의 문서를 잘 찾을 수 있게 전체 ***M*** 개의 text passage를 low-dimensional, continuous space에 색인하는 것이다. (DPR 논문에서의 M은 2100만, k는 20-100)
DPR은 question embedding과 passage embedding 간의 유사도를 구하는 것을 간단하게 내적으로 사용한다. 다른 여러가지 방법과 비교해봤더니 간단한 내적이 가장 좋았음을 부록에서 증명한다. DPR 논문에서는 Encoders로 BERT를 사용하였다. (base, un-cased, d=768)

$sim(q, p) = E_{Q}(q)^{T} E_{P}(p)$

위 유사도 함수를 ranking function으로 이용한 학습을 위해서 아래와 같이 negative log likelihood loss function을 정의한다.

$L(q_{i}, p_{i}^{+}, p_{i+1}^{-},... ,p_{i,n}^{-}) = -log\frac{e^{sim(q_{i}, p_{i}^{+})}}{e^{sim(q_{i}, p_{i}^{+})} + \sum_{j=1}^{n}e^{sim(q_{i}, p_{i, j}^{-})} } $

Negative sampling 방법은 3가지를 사용해봤다. (1) Random: 랜덤 passage, (2) BM25: question token으로 matching 되지만, answer는 존재하지 않는 passage, (3) Gold: 다른 question의 positive passage. DPR에서 가장 좋았던 방법은 gold passage를 same mini-batch에서 찾고 BM25 negative passage를 하나 뽑아 추가하는 것이었다. 여기서 in-batch negative가 효율적이었음 


## 실험 및 평가
### Retireval 성능 평가
BM25+DPR은 BM25와 DPR로 top-2000 passage를 검각각 검색하고, $BM25(q,p) + \lambda\cdot sim(q,p)$로 reranking
![](/assets/img/DPR_01.png)<br>
Squad의 성능차이가 크지 않은데, 이는 데이터셋을 만들 때 애초에 passage를 보고 질문/답변을 만들었기에 lexical overlap이 많기 때문이라 설명함

### End-to-end QA 성능 평가
![](/assets/img/DPR_01.png)

## 결론
* BERT encoder를 사용해 question & passage를 효과적으로 embedding하면 sparse representation 방법 이상의 retrieval 성능을 얻을 수 있음
* 이는 end-to-end QA 성능 향상에 기여함