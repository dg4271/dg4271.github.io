---
layout: post
title: Probabilistic Graphical Models 1 Note
excerpt: Coursera의 PGM 1 강의 내용을 정리한 노트
#Probabilistic Graphical Models 1

##Overview and Motivation
Model: Declarative representation
Probability Theory
- Declarative(Stand alone) representation with clear semantics
- Powerful reasoning patterns

Graphical Models
- Bayesian networks
    + Random variables들의 관계를 보여주는 directed graph
    + Medical diagnosis에 쓰임
- Markov networks
    + Random variables들의 관계를 보여주는 undirected graph 
    + Image segmentation에 쓰임

- Graphical Representation을 쓰는 이유
    + 직관적이고 간결한 데이터 구조
    + General-purpose algorithms을 사용하여 효과적으로 reasoning 가능
    + Sparse parameterization: 아주 적은 수의 파라미터를 사용하여 probability distribution을 효과적으로 표현함

- Image segmentation
- Textual Information Extraction
- Multi-sensor Integration: Traffic
등을 모두 PGM으로 효과적으로 표현할 수 있는 것

###Overview
Representation
- Directed and undirected
- Temporal and plate models
Inference(reasoning)
- Exact and approximate
- Decision making
Learning
- parameters and structure
- With and without complete data

###Factors
$$f(x)=






