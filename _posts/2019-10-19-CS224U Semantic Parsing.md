---
title: CS224U Semantic Parsing
date:   2019-10-19 23:54:05 +0900
categories:
- Natural Language Understanding
toc: true
toc_sticky: true
---

# CS224U Semantic Parsing
> Stanford 대학의 Bill MacCartney, Christopher Potts가 강의하는 Natural Language Understanding 수업인 CS224U (2019 Spring)을 온라인으로 듣고 정리한다.
>
>7번째 주제 Semantic Parsing

## Overview
* Semantic parsing의 위키피디아 정의는 자연 언어를 컴퓨터가 처리하기 좋은 형태의 논리적 형태로 변환하는 작업이다.
* CS224U를 수강하는 이유이기도한 것이 Semantic parsing인데, 아주 흥미로운 주제이다. (Bill MacCartney 교수님도 본인이 제일 좋아하는 주제라고 한다.) 

## Challange
![](/assets/img/CS224U_7_1.png)
![](/assets/img/CS224U_7_2.png)
* Semantic parsing은 위의 2가지 challenge처럼, 언어의 복잡하고 모호한 특성 때문에 쉽지 않다.

![](/assets/img/CS224U_7_3.png)
* 과거 SHRDLU, CHAT-80과 같은 시스템이 깜짝 놀랄만한 NLU 결과를 보여주었지만, 이들의 coverage는 아주 좁고 미리 설정한 주제를 조금이라도 벗어나면 이해하지 못한다.
* 우리는 아주 다양한 주제에 대해 이해할 수 있는 NLU를 원하지만 현재 구글 마저도 부분적인 이해를 하고있다.


## Semantic Parsing
![](/assets/img/CS224U_7_4.png)
* Semantic parsing의 목적은 앞서 말했듯이 자연어를 formal meaning represenation으로 변환하는 것이다.
* 이 때 우리가 변환할 target output representation을 어떻게 정의할 것인가가 중요할 것이다.


![](/assets/img/CS224U_7_5.png)
![](/assets/img/CS224U_7_6.png)


## Sippy Cup
![](/assets/img/CS224U_7_7.png)
* CS224U에서는 SippyCup을 가지고 Semantic parsing을 실습해 본다.


![](/assets/img/CS224U_7_8.png)
* SippyCup semantic parser의 outline


![](/assets/img/CS224U_7_9.png)
* 먼저 Context-free grammar, 프로그래밍 언어는 보통 deterministic하지만 자언어에 대해서는 non-deterministic해야 함

![](/assets/img/CS224U_7_10.png)
* 2번째 parsing algorithm, bottop-up 방식이 top-down 방식에 비해 모호성을 일찍 발견하고 제거할 수 있으므로 효율적임
![](/assets/img/CS224U_7_11.png)
![](/assets/img/CS224U_7_12.png)
* 파싱을 수행할 떄, 모든 rule을 일일이 만드는 것이 아니라 적절한 역할을 수행하는 annotator를 만드는 것이 좋은 방법임

![](/assets/img/CS224U_7_13.png)
* 파싱 과정에서 언어의 모호성으로 인하여, 여러 방법 중 하나를 선택해야 하는 경우가 많다. 다라서 적절하게 scoring을 하여 결정하는 방법을 사용해야 한다.

![](/assets/img/CS224U_7_14.png)
* grammar rule을 자동으로 만들기 위한 전략
![](/assets/img/CS224U_7_15.png)
* 데이터, 특히 formal represenation 데이터를 만드는 것은 느리고, 아주 비싸다.
* 따라서 denotations 방법이 제안되었었음.