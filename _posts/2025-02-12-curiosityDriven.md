---
layout: single # post
classes: wide
title: "Curiosity driven RL"  # 페이지 타이틀
post-order: 4                               # (내 커스텀 변수) 같은 카테고리 내 정렬 순서
use_math: true
categories:
  - paperreview
---

[Curiosity-driven Exploration by Self-supervised Prediction][paperlink]<sup>1<sup>

[paperlink]:https://arxiv.org/abs/1705.05363


[Exploration by Random Network Distillation][paperlink]<sup>2<sup>

[paperlink]:https://arxiv.org/abs/1810.12894


[BYOL-Explore: Exploration by Bootstrapped Prediction][paperlink]<sup>3<sup>

[paperlink]:https://arxiv.org/abs/2206.08332


[Curiosity in Hindsight][paperlink]<sup>4<sup>

[paperlink]:https://arxiv.org/abs/2211.10515


내적 동기 부여에 관한 강화학습 논문들을 살펴본다. 


# summary

## 강화학습 
강화학습은 주어진 환경에서 시행착오를 통해 누적보상을 최대화하도록 학습시키는 방식이다.
이 방법은 인간이 직접 가르쳐주지 않고 인공지능이 스스로 해결책을 찾는다는 점에서 매력적이다.
강화학습 시스템은 행동의 주체인 Agent와 주변 환경인 Environment로 구성되며, Agent는 '자신의 현재 state'와 '환경으로부터 받아들인 정보인 observation'에 따라 action을 결정한다.
Agent가 환경으로부터 얻는 가장 중요한 정보인 reward는 목적에 따라 사람이 설계해주어야 한다.
예를들어, 가장 효율적인 교통신호 제어기를 만들기 위해 자동차가 원활하게 움직이면 (막히지 않으면) 보상을 주는 식으로 환경을 설계할 수 있다.
이를통해 사람이 예상하지 못했던 방법이나, 사람이 생각한 방법보다 더 뛰어난 policy(행동)를 찾아낼 수 있다.

그러나 Agent를 잘 학습시키려면 보상을 잘 설계해야 하는 어려움이 따른다.
특히 보상이 복잡한 과정을 거쳐야 한번 주어지는 sparse reward 상황에서는 ＇그 보상에 이르기까지의 수많은 행동 중＇ 어떤 것이 보상을 얻는 데 필요했던 것인지 파악하기 어렵다.
즉, 어떤 행동이 보상을 얻는데 직접 관여했는지 파악하기 어렵기에 학습이 오래 걸리고, 심지어 진전되지 않는 단점이 있다.
문제는 대부분 현실의 문제가 이런 sparse reward 상황이라는 것이다.


## Intrinsic Curiosity Module (ICM)
이를 극복하기 위해 내적 동기부여 (intrinsic motivation)를 주자는 의견이 나왔으며, 대표적인 예시가 호기심(curiosity)이다. 
보상을 환경으로부터 주어지는 외적 보상과 Agent가 스스로 주는 내적 보상으로 나누어 보상이 듬성듬성 주어지는 상황을 극복한다. 

1번 논문에서는 호기심을 '예측값과 실제 값의 차이'로 정의한다. 
즉, 현재 상태와 그 상태에서의 행동을 입력받아 다음 상태를 예측하고, 실제 다음 상태와 차이를 비교하여 자신이 얼마나 다음 상태를 잘 예측했는지 보는 것이다. 
만약 그 차이가 크다면 아직 Agent가 그 환경을 제대로 알지 못한 것이므로, 호기심을 가지고 더 학습을 해보아야 한다고 판단한다.
이를 ICM (Intrinsic Curiosity Module)이라고 한다. 
환경으로부터 주어지는 보상을 최대화하면서 (exploitation) 내적인 동기부여를 통해 탐색을 (exploration) 촉진하는 균형잡힌 모델로 sparse reward 상황을 극복하는 방법이다.

<p align="center">
  <img src="https://github.com/user-attachments/assets/ebff8ca0-5857-4c7f-ae8b-fde9129ea9e1" width="100%" height="100%" alt="default" />
  <br>
  <em>ICM architecture</em>
</p>

## Random Network Distillation (RND)
ICM으로 잘 해결되지 않는 Monetzuma's revenge와 같은 문제를 단순한 해법으로도 성능을 크게 향상시킬 수 있음을 보여준 논문이다.   
ICM 과 마찬가지로 prediction error를 내적 보상(참신함,novelty)으로 사용한다. 내적 보상을 예측한 상태와 실제 상태의 차이가 아니라, 한 단계를 더 거쳐서 처리한다.    
관찰된 정보 (observation) 을 랜덤하게 초기화된 고정된 신경망에 넣어서 특정한 값을 뽑아낸다. 이 랜덤 신경망은 가중치가 변하지 않고 고정되어 있으며 agent의 학습이 영향을 미치지 않는다.   
다른 학습되는 신경망인 predictor는 같은 input을 받아서 random network(target net) 과의 차이를 구하며, 이 값의 MSE가 호기심으로 사용된다. 
또한, predictor가 target을 학습하는데에도 사용된다.   
이렇게 학습하면 내적 보상이 가져야 하는 세 가지 조건을 충족한다:   
1. 방문횟수가 많을수록 내적 보상이 감소해야 한다   
2. target은 predictor가 학습할 수 있는 구조이어야 한다         
(랜덤 네트워크가 아닌 그냥 난수생성기라면 predictor가 어떠한 규칙성을 학습할 수 없고, 다른 내적 보상을 줄이며 학습하다가 이전 내적보상이 다시 증가하게 될 수 있다)   
3. target의 output이 충분히 랜덤하여, predictor가 바로 모든 값을 예측하는 일이 거의 일어나지 않아야 한다   

내적 보상이 외적 보상에 비해 너무 크게 차이나지 않도록 보정하는 observation normalization을 수행하였다.

이렇게 내적 보상을 추가하는 신경망을 하나 추가한 것 외의 기본적인 구조는 PPO(proximal policy optimization)모델을 사용하였다.

rnd diagram img

이 논문의 저자가 실험해보며 찾은 효율적인 방법들이 몇가지 있다. RNN 구조가 CNN구조를 사용한 것보다 외적 보상의 고점이 높았고, CNN은 더 안정적이었다.   
또한, non-episodic 하게 학습한 것이 episodic(한 rollout이 끝나면 내적, 외적 보상 리셋) 하게 학습한 것보다 더 많은 방을 방문했다. Montezuma의 첫 스테이지는 총 24개의 방이 있으며 마지막 방에 도달하면 첫 스테이지를 클리어 한다. 따라서 방을 많이 방문할수록 잘 학습되었다고 볼 수 있다.   
저자는 새로운 방을 방문하는것은 모든 학습과정에 걸쳐서 일어나는 것이기 때문에 non episodic 학습이 더 적절하다고 판단하였다.  




## BYOL-Explore
BYOL은 주로 world model을 학습하는데 사용된다. 강화학습은 누적보상을 최대화하는 정책(policy)을 얻는걸 목표로 하므로, world model을 학습하는 동시에 좋은 탐색정책을 찾는 두마리 토끼를 동시에 잡는 방법이다.
ICM처럼 BYOL Agent가 예측한 world model과 실제 model의 차이가 큰 상태를 우선적으로 탐색하도록 가중치를 부여한다. 이 차이를 줄여나갈수록 world model이 잘 학습되었다고 볼 수 있다.
BYOL은 RNN을 기반으로 만들었으며, 크게 아래의 5가지 파트로 구성된다:    
1. action을 latent로 변환하는 encoder 
2. agent의 state representation을 구하는 closed loop RNN cell
3. world model prediction에 사용할 state representation을 구하는 open loop RNN cell
4. 위의 state를 통해 관측값을 예상하는 predictior
5. 실제 관찰된 observation을 의미있는 latent로 변환하는 EMA target encoder

<p align="center">
  <img src="https://github.com/user-attachments/assets/61aebb9c-d309-4e2a-8d58-0efb8740ccff" width="100%" height="100%" alt="default" />
  <br>
  <em>BYOL-Explore architecture</em>
</p>

4와 5의 차이를 uncertainty라 하며, 이 값이 curiosity에 해당한다. 

BYOL은 batch 단위로 학습하며, uncertainty의 non-stationarity를 방지하기 위해 batch의 std deviation을 나눠주는 reward normalization을 수행한다.   
또한, uncertainty를 바로 사용하지 않고 평균을 뺀 후 양의 값으로 clipping을 하여 사용함으로써 world model을 우선적으로 학습한 후 보상을 최대화하도록 만든다. 
이를 reward prioritization이라고 한다. 

BYOL은 기존의 RND나 ICM으로 해결하지 못한 문제를 해결할 수 있었으며, deterministic environment에서 더 높은 성능을 보였다.
특히 강화학습으로 해결하기 어려운 sparse reward 문제를 모아둔 DM-HARD-8 도 '인간이 클리어한 플레이 기록' 없이도 양의 보상을 얻을 수 있었다. 
그러나 아직 환경에 stochasity가 있거나 절차적으로 생성된 환경에서는 잘 학습되지 않는 문제가 있었다.


## Curiosity in Hindsight (Byol-Hindsight)
ICM 및 BYOL-Explore은 random noise가 있는 환경(stochastic environment)에 매우 취약하다는 한계가 있다. 
환경에 본질적으로 예측할 수 없는 요인이 있는 경우, 그 근방에서 몇 번 탐색하던 curiosity값이 크기 때문에 오히려 탐색을 방해하는 요인이 된다. (curiosity loop 현상) 
또한, 예측이 틀렸다 해도(큰 curiosity값) 그게 agent가 환경을 제대로 이해하지 못했다고 말할 수도 없다. 

예를들어 주사위를 굴려서 나온 눈을 맞추어야 하는 경우, '6이 나올 것이다'라고 한 예측이 틀렸더라도 'agent가 환경을 이해하지 못했다' 라고 말할 수는 없다. 
주사위의 눈은 확률에 따라 결정되는 것이며, 환경을 전부 이해하더라도 확률에 따라 틀릴 수도 있는 것이다. 
하지만 '6이 아닌 다른 면이 나왔다' 라는 정보를 알고 있다면 '6이 나올 것이다'라는 예측이 틀렸다는 것을 알 수 있다. 
즉, 사건이 일어난 후의 정보인 '회고변수'를 안다면 '다음 상태'가 결정된다. 
즉, agent가 환경을 이해했다는 것을 예측과 실제 값의 차이로 파악하기 위해서는 확률적인 요인을 배제한 후 비교해야 한다. 
회고변수를 알 수 있다면, 확률적 요인을 제거한 후 agent의 예측과 실제 값을 비교하여 얼마나 환경을 잘 이해했느를 따질 수 있다! 

4번 논문에서는 random noise가 있는 환경에서의 학습을 극복하기 위해 회고 변수를 도입하고 모델을 예측하는 새로운 보상구조를 제안한다. 
intrinsic reward = reconstruction error + invariance error    
reconstruction error는 Agent가 현재 state, action 그리고 이전의 회고변수로부터 다음 state를 얼마나 잘 예측하느냐로, 기존의 curiosity와 유사한 개념이다. 
invariance error는 회고변수가 순수한 noise만 포함하도록, 즉 회고변수가 Agent의 상태와 행동에 얼마나 무관한지를 측정한다.

모델은 최신 모델인 Byol-Explore를 개선한 Byol-Hindsight를 사용하였다.
<p align="center">
  <img src="https://github.com/user-attachments/assets/9b00eebe-969e-44ef-af14-173c15b1262e" width="100%" height="100%" alt="default" />
  <br>
  <em>BYOL-Hindsight architecture</em>
</p>

# discussion
Sparse reward 인 상황의 대표적인 문제 Montezuma's revenge를 굉장히 잘 해결하여 신기하였다. 
이전까지만 해도 sparse reward는 강화학습에서 매우 까다로운 문제로 여겨지고 많은 방법들이 좋지 않은 성능을 보여주었다.
그러나 내적 동기부여를 활용해 이를 극복할 수 있다는 실마리가 보이는 것 같다. 
향후 물리법칙을 실험과 관찰로 스스로 알아내는 Newton AI를 구현할 때 생각해 보면 좋을 시스템인것 같다. 



* Structural Causal Model (SCM)
[Counterfactually-Guided Policy Search][paperlink]<sup>5<sup>

[paperlink]:https://arxiv.org/abs/1811.06272
