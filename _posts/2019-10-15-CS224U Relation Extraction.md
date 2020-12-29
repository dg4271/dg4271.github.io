---
title: CS224U Relation Extraction
date:   2019-10-16 23:54:05 +0900
categories:
- Natural Language Understanding
toc: true
---

# CS224U Relation Extraction
> Stanford 대학의 Bill MacCartney, Christopher Potts가 강의하는 Natural Language Understanding 수업인 CS224U (2019 Spring)을 온라인으로 듣고 정리한다.
>
>4번째 주제 Relation Extraction

## Overview
![](/assets/img/CS224U_4_1.png)
* Relation Extraction (RE)는 자연어 문서로 부터 relation triple을 추출해 내는 작업
* relation triple은 Knoweldge Base (KB)을 구성하는 하나의 단위이다.
* KB는 question answering 등의 application에 아주 유용하게 사용할 수 있다.
* Google, Microsoft, Apple 등의 기업들이 knowledge graph를 갖고 있고, 적극적으로 할용하고 있다. 


## Distant Supervision
* RE task의 supervised learning을 위해서는 labeled data가 아주 많이 필요하다. (label이 워낙 많기 때문)
* 그러나 labeled data은 언제나 귀한 것
* Distant Supervision은 기존 KB의 relation을 사용하여, 갖고 있는 corpus로 부터 해당 relation으로 labeled data를 자동 생성한다.
* 그렇게 함으로써 대량의 labeled data를 확보하여 학습에 사용할 수 있다. (물론 이러한 labeles data에는 postive와 negative가 공존함)
* 그러나 KB가 미리 존재해야 하며, 대량의 labeled data에는 noise가 있으므로 학습이 잘 안될 수 있는 단점이 있음
* 또한 기존 KB가 갖는 relation에 대해서만 distant supervision이 가능하기 때문에, 새로운 relation을 발견하는 용도로는 적합하지 않음