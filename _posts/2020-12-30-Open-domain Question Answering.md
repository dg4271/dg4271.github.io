---
title: Open-domain Question Answering
date:   2020-12-29 23:54:05 +0900
categories:
- Question Answering
toc: true
toc_sticky: true
---

# 한국어 오픈 도메인 질의 응답 시스템 구현
> 기계 독해 분야에서 BERT를 시작으로한 transformer 계열의 언어 모델들의 사람 이상의 성능을 보여주었다. 이후로 딥러닝 기반의 오픈 도메인 질의 응답 시스템의 연구가 아주 활발하게 진행되고 있다.
>
> 이번 글에서는 한국어로 동작하는 오픈 도메인 질의 응답 시스템을 구현하는 과정을 정리하였다.


## 관련 연구 조사
* 기계 독해를 이용한 웹 기반 오픈 도메인 한국어 질의응답, KaKao, HCLT 2019
  * [blog](https://tech.kakaoenterprise.com/71)
  * 전체 시스템은 질의 분석기, 검색 질의 생성기, 검색 결과 정제기, 후보/최종 정답 추출기로 구성되어 있음
  * ![](/assets/img/ODQA_01.png)
  * 질의 분석기는 키워드 추출, 답변 타입을 분류기, 육하원칙 분류기로 구성
  * 검색 질의 생성기는 동사-명사 변환기, 질의 확장기로 구성
  * 검색 결과 정제기는 Chunk 추출기, 중복 제거기로 구성
  * Retrievel로는 기 존재하는 검색 엔진을 사용하여 실시간으로 최대 1,500개의 문서를 찾음
  * Reader로는 아래 구조 사용
    * ![](/assets/img/ODQA_02.png)
    * 실행 시간의 문제로, 임베딩 레이어에서는 한글 Glove 임베딩 벡터를 자모 단위 임베딩 벡터와 같이 사용
       * 한국어 대화 엔진에서의 문장 분류, HCLT 2018
* KorQuAD를 활용한 한국어 오픈도메인 질의응답 시스템, 부산대학교, HCLT 2019
  * Retriever로는 iverted index 방법과 BM25를 적용
  * Reader로는 BERT 모델을 사용하고, 앞단에 검색한 문서에 대해 쿼리와 단락의 유사도를 확인하여 랭킹을 부여하는 단락 선별기가 존재
* REALM을 이용한 한국어 오픈도메인 질의 응답, 전북대학교, HCLT 2020
  * 최근 문서와 질문을 dense vector로 인코딩하는 학습 가능한 딥러닝 기반의 Dense Retrieval 연구가 활발히 이루어짐
  * ORQA에서 ICR(Inverse cloze task)를 통해 BERT 인코더를 기반으로 문서와 쿼리 인코더의 사전학습 방법을 제안함
    * (ORQA) Latent retrieval for weakly supervised open domain question answering, arXiv 2019
  * REALM에서 ICT 사전학습 된 문서, 쿼리 인코더에 추가적으로 언어모델을 더해 사전 학습하고, 이를 활용해 오픈 도메인 질의 응답 성능을 개선할 수 있음을 보임
    * REALM: Retrieval-augmented language model pre-training, arXiv 2020
  * 실제 REALM 논문에서 64개 TPU를 사용해 학습한 것과 비교하여, 가용 가능한 GPU의 한계로 Dense retriever의 학습에 DistilRoBERTa를 사용함 
  * 파인 튜닝한 ORQA, REALM 모델 모두 검색 성능이 BM25를 뛰어넘었음
    * ![](/assets/img/ODQA_03.png)



## 구현



## 실험 및 평가



## 결론



## 참고 링크