---
layout: single # post
title: "Guiding diffusion"  # 페이지 타이틀
post-order: 4                               # (내 커스텀 변수) 같은 카테고리 내 정렬 순서
use_math: true
categories:
  - paperreview
---

[Guiding a Diffusion Model with a Bad Version of Itself][paperlink]

[paperlink]:https://arxiv.org/abs/2406.02507

위 논문을 살펴본다. 

# summary
이 논문에서는 확산모델을 학습시키는 방법 중 auto guidance라는 방법을 제시한다.
기존의 방법은 unconditional model을 사용하여 guidance 를 수행하였는데, 고화질의 이미지를 얻을 순 있어도 이미지의 다양성이 희생되었다.
이 논문에서는 확산 모델을 학습시키기 위해 더 작고 덜 학습된 확산 모델을 만들고, 테스트 이미지에 대해 둘의 결과의 차이만큼 큰 확산 모델에 가중치를 추가로 부여해 강화시켰다.
이를 통해 고품질의 이미지를 생성하면서도 이미지 다양성을 훼손시키지 않도록 한 연구이다.



# discussion
엔비디아의 연구자들이 어떤 연구를 수행하는지 볼 수 있었다. 
역시나 그래픽 관련 인공지능을 연구한다.

