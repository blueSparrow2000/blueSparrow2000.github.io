---
layout: single # post
classes: wide
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
이를 알아보고자 Gazebo simulator의 navigation task에서 실험을 해보았다.

강화학습 모델은 DQN을 사용하였으며, action은 5가지 agent의 각속도 변화이다. agent는 해당 방향으로 일정한 속력으로 이동한다.
state는 24방향 LiDAR 센서값과 가장 가까운 장애물까지의 거리과 각도, 목표지점까지의 거리와 각도 총 28개의 값을 입력으로 받는다.

기준이 되는 보상(Baseline)은 목표 도달시 100, 장애물 충돌시 -50이라는 sparse reward를 준다.
수정한 보상은 양/음 비율에 따라 3가지로 나뉜다. 보상 함수의 형태도 서로 다르다. 선형적으로 다르기만 하다면 학습에 차이가 없여야 하므로 유의미한 결과를 얻기 힘들었을 것이다.     
balance class (50:50)     
positive skewed class (72:28)     
negative skewed class (22:78)     

학습에 유리하다는 것엔 두 가지 판단 척도를 두었다. 
1. 더 빠르게 최적해로 수렴 (목표에 안정적으로 도달할때까지 epoch가 더 적음)
2. 누적 보상(cumulated reward)의 변동(variation)이 더 적음 (안정적으로 학습)

가장 빠르게 수렴한 것은 baseline이었고, 음의 보상이 뒤따랐다.
변동이 가장 적은 것은 음의 보상을 더 많이 준 경우로, 두가지 평가에서 모두 음의 보상 비율이 높을때가 다른 경우보다 학습에 더 유리하였다.

<p align="center">
  <img src="https://github.com/user-attachments/assets/798c441f-68ca-4847-a56e-5a44baede129" width="80%" height="80%" alt="default" />
  <br>
  <em>DQN Setting</em>
</p>



# discussion
한 문제에 대해 위와 같은 결과를 얻었다고 일반화할수는 없다. 환경이나 model이 달라져도 비슷한 결과를 얻는지 확인해보는 연구가 필요하다.
다른 수많은 atari game들 또는 다른 분야의 문제를 해결할때도 음의 보상이 더 좋은지 알아봐야 한다.
또한, model free off policy인 DQN 뿐만 아니라 PPO나 Dreamer같은 다른 접근법응 취하는 방법이나 복잡한 모델에서도 비슷한 결과를 얻을 수 있는지 실험해야 한다.

음의 보상이 더 좋은 이유가 무엇일지도 근본적으로 해결해 보아야 할 문제이다.
더 빠른 수렴이 가능했던 이유는 DQN의 state 초기화 방식 때문일수도 있다. 다른 state가 0으로 초기화 되었을때, 보상이 대부분 음수라면 방문하지 않은 state를 더 방문하게 되므로 exploration을 촉진한다. 따라서 기본적으로 탐색에 유리하므로 더 빠르게 최적의 해를 찾아낼 수 있었을 것이다.
다만 이는 초기화를 어떻게 하냐에 따라 달라질 수 있기에 초기화를 음수를 포함한 랜덤값으로 했을때에도 (xavier initialization 등) 탐색에 유리한지 실험해 봐야한다.



