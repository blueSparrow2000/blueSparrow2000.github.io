---
layout: single # post
title: "Diffusion Models"  # 페이지 타이틀
post-order: 4                               # (내 커스텀 변수) 같은 카테고리 내 정렬 순서
# comments: true
categories:
  - paperreview
---

요즘 유행하는 확산모델의 배경지식을 알아본다. 노이즈, 노이즈, 노이즈...

다음 논문을 참고하였다.

[Diffusion Models in Vision: A Survey][paperlink]

[paperlink]:https://arxiv.org/abs/2209.04747


# Diffusion 

* 확산과 노이즈 비교 이미지

한 곳에 모인 기체 분자가 확산하면 금방 사방으로 퍼져 처음 상태를 알기 힘들게 된다. 
처음 상태를 예측하기 위해서는 매 시간간격마다 한 입자가 어느 위치에서 이동했을지를 알아야 한다.
마지막 상태에서 차근차근 입자의 이동거리를 빼주면 처음 상태를 계산할 수 있다.

확산 모델도 이와 같은 방식으로 원본 이미지를 복원하도록 훈련된다.
원래의 이미지에 노이즈를 단계적으로 추가하면서 형체를 알아볼 수 없는 노이지 이미지(noisy image)로 만든다. 그리고 신경망을 이용해 각 단계에서 노이즈값(또는 분포)가 얼마였는지를 학습한다. 노이지 이미지에서 예측한 노이즈를 계속 빼주다 보면 원본 이미지를 복원할 수 있다!

이렇게 얻은 확산모델은 사용자의 입력을 바탕으로 노이즈를 예측하여 원하는 이미지를 생성하는데 쓸 수도 있다.


# Models
확산모델의 기본 구조는 다음과 같다.

* 모식도

노이지 이미지와 확산 단계(diffusion step) 정보를 입력으로 받아 노이즈를 예측한다.

정확도를 높이기 위해 확산 모델에 분류기(classifier)를 연결한 모델도 나왔다.
매 단계마다 확산 모델이 원하는 출력을 만들어낼 수 있도록 분류기가 gradient로 피드백 해준다.

GLIDE라는 모델은 분류기 없이 텍스트 정보를 추가로 이용해 학습한다.

* 글라이드 모식도

텍스트는 트랜스포머가 처리하여 노이지 이미지와 함께 확산모델의 입력으로 들어간다.
확산모델이 작은 이미지를 생성하면, 또 다른 확산모델로 크게 확대시키는 구조이다. 
확산모델의 초창기에는 한번에 노이즈가 잘 제거된 큰 이미지를 만들어내지 못해 두 단계를 거쳤다.

Latent Diffusion Model(LDM)은 이 문제를 개선하였다.
인코더를 이용해 이미지를 잠재공간(latent space)으로 보낸 후 확산모델을 잠재공간에서 학습시킨다.
이렇게 하면 이미지를 픽셀 구조가 아닌 의미와 형태 단위로 파악할 수 있고 속도도 더 빠르다. 

LDM은 다른 포스트에서 따로 소개한다.



# Generic framework

확산모델의 훈련 과정은 노이즈를 추가하는 forward stage와 노이즈를 예측하는 reverse stage로 구성된다.
원본 이미지와 텍스트를 입력으로 받고, forward stage에서 추가된 노이즈를 잘 예측하도록 reverse stage에서 학습한다.
수학적으로는 노이즈를 추가하는 과정이 Markov process, 역과정이 Reverse markov process이다.

확산모델은 모델을 만드는 방식에 따라 크게 세 가지로 분류된다.
DDPM(denoising diffusion probabilistic model)은 latent variable을 이용해 확률분포를 예측하는 일종의 auto encoder방식이고, 유명한 LDM역시 이 방식을 이용한다. 
NCSN(noise conditioned score network)는 langervin dynamics라는 물리 역학을 바탕으로 확산을 모델링한다.
마지막으로 SDE(stochastic differential equation)은 위 두가지 방식을 일반화한 것으로, 미분방정식을 수치적으로 풀어서 해를 구하는 방식이다.

아래와 같이 세가지 방식은 forward stage와 reverse stage가 유사하다.


# Relation to other generic models

### VAE 
#### Variational Auto Encoder
확산모델도 VAE처럼 입력을 잠재공간으로 보낸다. 두 방식 모두 data likelihood의 하한(lower bound)을 손실함수로 사용한다.
두 모델의 가장 큰 차이점은 입력을 잠재공간으로 보내는 과정에 있다. 확산모델의 reverse stage는 VAE의 decoding처럼 잠재공간의 데이터를 원래 공간으로 보내도록 학습한다. 그러나 VAE의 encoding이 잠재공간으로 보내는 대응관계(mapping)을 학습하는것에 반해, 확산모델의 forward stage는 입력에 가우시안 노이즈를 추가하는 과정으로 mapping을 학습하지 않는다. 
즉, VAE의 latent space에는 데이터가 의미있는 정보를 가지고 있지만, 확산모델의 latent space에는 노이즈로 인해 파괴된 의미없는 정보가 있다.


### GAN
#### Generative Adversarial Network
GAN은 확산모델보다 학습이 더 어렵고, mode collapse가 발생할 위험이 있다. 확산모델은 반대로 학습과정이 안정적이고, 타 likelihood model처럼 더 다양한 출력이 나온다.
그러나 확산모델은 inference과정에서 GAN보다 시간이 많이 소요되는 단점이 있다.

