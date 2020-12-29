---
title: CS224U Supervised Sentiment Analysis
date:   2019-10-09 23:54:05 +0900
categories:
- Natural Language Understanding
toc: true
toc_sticky: true
---

# CS224U Supervised sentiment analysis
> Stanford 대학의 Bill MacCartney, Christopher Potts가 강의하는 Natural Language Understanding 수업인 CS224U (2019 Spring)을 온라인으로 듣고 정리한다.
>
>3번째 주제 Supervised Sentiment Analysis

## Overview
![](/assets/img/CS224U_3_1.png)
![](/assets/img/CS224U_3_2.png)
* 감정 분석이 무엇인가에 대해서 먼저 생각해 볼 수 있는 예시들.
* 긍정/부정 단 두개로 나눌 수 있는 것인가? 중립은? 그 중간의 감정들은?
* 그리고 객관적으로 긍정인(부정인) 문장이 있는가? 문맥에 따라 감정은 어떻게 달라지는가? 등을 생각해 볼 수 있었다.


## General practical tips for sentiment analysis
* 감정 분석을 하는데 있어서 실용적인 팁들을 소개한다.
![](/assets/img/CS224U_3_3.png)
![](/assets/img/CS224U_3_4.png)
* 감정이 잘 드러나도록 tokenizing 하는 것이 중요하다.
![](/assets/img/CS224U_3_5.png)
* stemming 방법도 신중하게 선택해야 한다.
* 위의 예처럼 porter stemmer는 전혀 다른 감정을 보이는 두 단어를 동일하게 stemming 한다.
![](/assets/img/CS224U_3_6.png)
![](/assets/img/CS224U_3_7.png)
* wordnet stemmer가 감정 분석에 활용하기에 좋은 선택
![](/assets/img/CS224U_3_8.png)
![](/assets/img/CS224U_3_9.png)
![](/assets/img/CS224U_3_10.png)
* negation marking도 효과적인 preprocessing 기술이다.



## The Stanford Sentiment Treebank (SST)
* Traeebank 형태로 감정 분석을 수행하는 socher의 연구
* 크라우드소싱을 활용해 1만건 정도의 문장에 레이블링을 하였다.
![](/assets/img/CS224U_3_11.png)
![](/assets/img/CS224U_3_12.png)
![](/assets/img/CS224U_3_13.png)

## sst code

## Methods
* 모델 검증을 위한 방법?
* 우선 모델은 일반적으로 hyperparameter를 갖고 이는 모델의 성능의 영향을 준다. 따라서 여러 hyperparameter를 실험해보면서 최적의 것을 찾아야 한다.
* 그리고 두 모델을 비교할 때 있어서 통계적으로 신뢰성있는 방법을 사용하는 것이 좋다.
![](/assets/img/CS224U_3_14.png)
![](/assets/img/CS224U_3_15.png)
![](/assets/img/CS224U_3_16.png)


## Feature representation
* Feature representation 기법들
![](/assets/img/CS224U_3_17.png)
![](/assets/img/CS224U_3_18.png)
![](/assets/img/CS224U_3_19.png)

## RNN Classifier
* RNN 계열의 classifier 소개
![](/assets/img/CS224U_3_20.png)

## Tree Structured networks
* 트리 형태의 데이터에 적합한 네트워크
![](/assets/img/CS224U_3_21.png)
![](/assets/img/CS224U_3_22.png)
![](/assets/img/CS224U_3_23.png)
* root 뿐만 아니라 모든 노드에 대한 suvervision