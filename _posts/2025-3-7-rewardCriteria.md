---
layout: single # post
title: "Reward criteria impact on RL"  # 페이지 타이틀
post-order: 4                               # (내 커스텀 변수) 같은 카테고리 내 정렬 순서
use_math: true
categories:
  - paperreview
---

[Reward criteria impact on the performance of reinforcement learning agent for autonomous navigation][paperlink]

[paperlink]:https://www.sciencedirect.com/science/article/pii/S1568494622004586

2021년에 쓰인 위 논문을 살펴본다. 

# summary
강화학습을 효과적으로 하기 위해서는 보상(reward)을 적절히 설정해야 한다. 보상은 해결하고자 하는 문제에 따라 양수로 줄수도, 음수로 줄 수도 있다.
양으로 지우친 보상과 음으로 지우친 보상 중 어떤 보상을 택하는게 학습에 더 유리할까?
이를 알아보고자 간단한 환경에서 실험을 한 논문이다.



# discussion
한 문제에 대해 위와 같은 결과를 얻었다고 일반화할수는 없다. 환경이나 model이 달라져도 비슷한 결과를 얻는지 확인해보는 연구가 필요하다.
다른 수많은 atari game들 또는 다른 분야의 문제를 해결할때도 음의 보상이 더 좋은지 알아봐야 한다.
또한, model free off policy인 DQN 뿐만 아니라 PPO나 Dreamer같은 다른 접근법응 취하는 방법이나 복잡한 모델에서도 비슷한 결과를 얻을 수 있는지 실험해야 한다.

음의 보상이 더 좋은 이유가 무엇일지도 근본적으로 해결해 보아야 할 문제이다.
더 빠른 수렴이 가능했던 이유는 DQN의 state 초기화 방식 때문일수도 있다. 다른 state가 0으로 초기화 되었을때, 보상이 대부분 음수라면 방문하지 않은 state를 더 방문하게 되므로 exploration을 촉진한다. 따라서 기본적으로 탐색에 유리하므로 더 빠르게 최적의 해를 찾아낼 수 있었을 것이다.
다만 이는 초기화를 어떻게 하냐에 따라 달라질 수 있기에 초기화를 음수를 포함한 랜덤값으로 했을때에도 (xavier initialization 등) 탐색에 유리한지 실험해 봐야한다.



