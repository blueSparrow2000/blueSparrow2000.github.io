---
layout: single # post
classes: wide
title: "Transformer"  # 페이지 타이틀
post-order: 4                               # (내 커스텀 변수) 같은 카테고리 내 정렬 순서
use_math: true
categories:
  - paperreview
---

[Attention Is All You Need][paperlink]

[paperlink]:https://arxiv.org/abs/1706.03762

위 논문을 살펴본다. 3 blue 1 brown의 transformer 영상을 참조하였다.

# summary

트랜스포머의 핵심 구조는 self attention layer와 MLP layer 두가지로 구성되어 있다.

입력으로 문자열이 주어지고, 이를 통해 다른 문자열을 생성하고자 한다고 하자. 문자열을 받아서 다음에 올 문자를 예측하는 식으로 생성하면 된다.

문자열은 컴퓨터가 조작하기 쉬운 숫자 벡터로 변환시킨다.

self attention은 주어진 문자열에서 문맥을 분석한다. 어떤 단어가 다른 단어랑 얼만큼 연관이 있는지 계산하여 각 단어의 관계를 바탕으로 단어벡터들을 수정한다. 
다른 단어와 연관된 정도를 계산하고 얻는 벡터가 Q, K, V 이다. Q는 문맥 정보에 대한 질의, K는 문맥정보에 대한 일종의 답변, V는 K에 해당하는 가중치 값으로 생각할 수 있다. 
아래 식을 통해 계산한 값이 attention이며, 이를 원래 단어 벡터에 더하여 문맥정보를 반영한다.

이어지는 MLP은 명제 같은 의미정보를 학습하였으며, 단어가 통과할때 각 의미들과 연관된 정도를 계산하여 다음 단어를 예측한다.
학습된 여러개의 의미 행렬에 단어를 각각 곱하여 의미적으로 가까운 정도를 계산하고, 활성화 함수를 거친 뒤, 각 의미에 대응되는 값 행렬을 곱하여 단어에 정보를 추가한다.

이 과정을 여러번 거친 후 다음에 올 단어를 최종적으로 예측한다.


여기서 self attention을 병렬로 여러개 수행하도록 만든게 Multi head attention이다.


# discussion




