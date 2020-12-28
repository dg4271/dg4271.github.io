---
layout: post
title: CS224U Distributed Word Representations
date:   2019-09-08 23:54:05 +0900
author: dg4271
categories: Natural Language Understanding
---

# CS224U Distributed Word Representations
> Stanford 대학의 Bill MacCartney, Christopher Potts가 강의하는 Natural Language Understanding 수업인 CS224U (2019 Spring)을 온라인으로 듣고 정리한다.
>
>2주차 Distributed Word Representations


## Slides

![](/assets/img/CS224U_2_2.png)
![](/assets/img/CS224U_2_1.png)

### Designs
![](/assets/img/CS224U_2_3.png)
* 자연어 처리를 위한 verctor-space model 설계의 다양한 선택지들을 아주 잘 정리한 슬라이드
* GloVe나 word2vec은 설계 과정에서 하나의 통합 솔루션으로 사용할 수 있음
<br>
![](/assets/img/CS224U_2_4.png)
* Window n: focal word 기준으로 좌우 n개의 단어들
* co-occurrence matrix 설계시 고려할 점들
<br>

### Vector Comparisons
![](/assets/img/CS224U_2_5.png)
* cosine 계산에는 length normalization 부분이 있기 때문에 vector 길이에 영향 받지 않음
![](/assets/img/CS224U_2_6.png)
* Matching coefficient: binary vector를 생각하면 두 벡터가 모두 1인 원소가 많을 수록 score가 높음 
<br>
![](/assets/img/CS224U_2_7.png)
![](/assets/img/CS224U_2_8.png)
<br>

### Basic reweighting
![](/assets/img/CS224U_2_9.png)
![](/assets/img/CS224U_2_10.png)
* 예상하는(expected) 셀의 값보다 실제 관측(observed) 값이 클 수록 중요성을 더 확대(amplify)해야 함<br>
![](/assets/img/CS224U_2_11.png)
* 위 개념을 log-space에 적용한 개념이 PMI
* PMI는 GloVe의 핵심적인 아이디어

```python
def observed_over_expected(df):
    col_totals = df.sum(axis=0)
    total = col_totals.sum()
    row_totals = df.sum(axis=1)
    expected = np.outer(row_totals, col_totals) / total
    oe = df / expected
    return oe


def pmi(df, positive=True):
    df = observed_over_expected(df)
    # Silence distracting warnings about log(0):
    with np.errstate(divide='ignore'):
        df = np.log(df)
    df[np.isinf(df)] = 0.0  # log(0) = 0
    if positive:
        df[df < 0] = 0.0
    return df
```

pmi의 구현은 위와 같이 할 수 있다. (cs224u 실습자료 vsm.py 참조)

### Glove
![](/assets/img/CS224U_2_12.png)
![](/assets/img/CS224U_2_13.png)
* Glove objective
![](/assets/img/CS224U_2_14.png)
* 서로 co-occurence가 없는 두 단어 gnarly, wicked는 서로 awesome과 terrible에 대한 co-occurence가 비슷하다. 두 단어는 glove objective를 사용하여 비슷한 vector representation을 갖도록 학습된다. 


### word2vec
* word2vec의 기본적인 개념
* 학습 효율적으로 하기 위한 negative sampling
![](/assets/img/CS224U_2_15.png)
![](/assets/img/CS224U_2_16.png)


### Retrofitting
![](/assets/img/CS224U_2_17.png)
* Retrofitting(개조?)
* Word2vec, glove 같은 ditributional representation은 주어진 corpora에서의 단어들의 분포만 고려한다.
* Retrofitting은 기존에 잘 정의된 Structured resources(특히 WordNet 같은 semantic lexicons, 또는 knowledge graph)를 활용해 representation을 개선하려는 방법이다.

![](/assets/img/CS224U_2_18.png)
* retrofitting 모델은 주어진 VSM 벡터 q_i_hat을 학습하여 새로운 q_i을 얻는 목적을 갖는다.
* 이 때, graph neighbor 정보를 활용하는 부분이 목적 함수에 존재한다.
* 관련하여 최신 연구인 conceptNet5.5 [[paper](https://www.aaai.org/ocs/index.php/AAAI/AAAI17/paper/viewPaper/14972)]가 괜찮아 보임