---
layout: single # post
title: "Curiosity driven RL"  # 페이지 타이틀
post-order: 4                               # (내 커스텀 변수) 같은 카테고리 내 정렬 순서
use_math: true
categories:
  - paperreview
---

[Curiosity-driven Exploration by Self-supervised Prediction][paperlink]^1

[paperlink]:https://arxiv.org/abs/1705.05363

[BYOL-Explore: Exploration by Bootstrapped Prediction][paperlink]^2

[paperlink]:https://arxiv.org/abs/2206.08332


[Curiosity in Hindsight][paperlink]^3

[paperlink]:https://arxiv.org/abs/2211.10515


내적 동기 부여에 관한 강화학습 논문들을 살펴본다. 


# summary
강화학습은 주어진 환경에서 시행착오를 통해 누적보상을 최대화 하도록 학습시키는 방식이다.     
이 방법은 인간이 직접 가르쳐주지 않고 인공지능이 스스로 해결책을 찾는다는 점에서 매력적이다.    
강화학습 시스템은 행동의 주체인 Agent와 주변 환경인 Environment로 구성되며, Agent는 자신의 state와 observation을 통해 action을 결정한다. 
Agent가 환경으로부터 얻는 가장 중요한 정보인 reward는 목적에 따라 사람이 설계해주어야 한다. 
예를들어, 가장 효율적인 교통신호 제어기를 만들기 위해 자동차가 원활하게 움직이면 (막히지 않으면) 보상을 주는 식으로 환경을 설계할 수 있다. 
이를통해 사람이 예상하지 못했던 방법이나, 사람이 생각한 방법보다 더 뛰어난 policy(행동)을 찾아낼 수 있다.

그러나 Agent를 잘 학습시키려면 보상을 잘 설계해야 하는 어려움이 따른다. 
특히 보상이 복잡한 과정을 거쳐야 한번 주어지는 sparse reward 상황에서는 '그 보상에 이르기까지의 수많은 행동 중' 어떤 것이 보상을 얻는데 필요했던 것인지 파악하기 어렵다. 
즉, 어떤 행동이 보상을 얻는데 직접적으로 관여했는지 파악하기 어렵기에 학습이 오래 걸리고, 심지어 진전이 되지 않는 단점이 있다. 
문제는 대부분의  현실의 문제가 이런 sparse reward 상황이라는 것이다. 

이를 극복하기 위해 내적 동기부여 (intrinsic motivation)를 주자는 의견이 나왔으며, 대표적인 예시가 호기심(curiosity)이다. 
보상을 환경으로부터 주어지는 외적 보상과 Agent가 스스로에게 주는 내적 보상으로 나누어 보상이 듬성듬성 주어지는 상황을 극복한다. 
1) 논문에서는 호기심을 '예측값과 실제값의 차이'라고 정의한다.






# discussion




* Structural Causal Model (SCM)
[Counterfactually-Guided Policy Search][paperlink]

[paperlink]:https://arxiv.org/abs/1811.06272
