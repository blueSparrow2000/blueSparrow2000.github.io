---
layout: single # post
classes: wide
title: "Quantum machine learning"  # 페이지 타이틀
post-order: 4                               # (내 커스텀 변수) 같은 카테고리 내 정렬 순서
use_math: true
categories:
  - paperreview
---

[Quantum Machine Learning][paperlink]

[paperlink]:https://arxiv.org/abs/1611.09347

위 논문을 살펴본다. 

# summary
양자컴퓨터가 인공지능에 끼치는 영향은 어떻게 될까?
양자컴퓨터가 잘 푸는 문제와 현재의 컴퓨터로도 충분히 잘 풀리는 문제가 있다. 인공지능의 거대행렬 연산을 생각해보면 양자컴퓨터가 나타난다고 해서 속도가 크게 향상되지는 않을 것 같다.
오히려 양자컴퓨터용 연산을 수행하는데 더 많은 시간이 들지는 않을까?
이 문제에 관한 논문이 있다. 결론만 말하면, 양자컴퓨터를 활용할 경우 전통적인 기계학습의 속도를 개선할 수 있다.
다만, 양자컴퓨터가 다룰 수 있게 데이터를 변형하는데, 답을 얻으려면 다시 변환하는 과정을 수행해야 하며, 이것이 시간복잡도를 크게 잡아먹는다.
양자컴퓨터의 계산은 데이터의 평균에 대한 계산을 수행하는 것과 같아서, 다시 데이터를 얻으려면 복원과정을 거쳐야 한다.

양자컴퓨터는 양자 결맞음이나 얽힘 현상을 이용해 quantum speedup을 만든다.
예를들어, 양자컴퓨터에 Grover's algorithm을 사용하면 사용하면 정렬되지 않은 데이터베이스에서 특정 데이터를 찾는 search에 O(N)이 아닌 O($\sqrt{n}$) 이 걸린다.
PCA나 SVM도 expomential speedup이 가능하다. 
또한 Ax=b 꼴의 선형방정식을 풀때 사용하는 양자 알고리즘인 HHL을 적용하면 성능을 더욱 개선할 수 있다.




#discussion


