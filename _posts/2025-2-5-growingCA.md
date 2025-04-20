---
layout: single # post
classes: wide
title: "Growing cellular automata"  # 페이지 타이틀
post-order: 4                               # (내 커스텀 변수) 같은 카테고리 내 정렬 순서
use_math: true
categories:
  - paperreview
---

[Growing Neural Cellular Automata][paperlink]

[paperlink]:https://distill.pub/2020/growing-ca/

위 논문을 살펴본다. 

# summary
생물의 형태형성(morphogenesis)를 모티브로 한 분산화된 자가 조직화 ai 에 대한 연구이다. 
life game은 단순한 몇 가지 규칙들만으로 복잡한 패턴을 만들어낸다.
여기에 규칙 대신 신경망을 이용해 인접한 세포의 정보만으로 복잡한 패턴을 기억하도록 하였다.

위 사이트에 방문하면 그림 일부를 지웠을때 복구해내는 프로그램을 볼 수 있다.





# discussion
그림의 일부를 지웠을때 이를 복구하는 일만 한다면, 그냥 원본 이미지를 저장해 두었다가 복구하면 되지 않냐는 지적이 있었다.
이미지 압축으로 보더라도 다른 효율적인 방법들이 있고, 굳이 인공지능을 이용해 복구해야 할 이유가 없기 때문이다.
이 한 task만 본다면 큰 의미는 없다.
하지만 각각의 분산화된 유닛이 자신과 인접한 cell의 정보만 이용하여 복잡한 패턴을 재생성할 수 있으며 여기에 인공지능을 활용한다는 점에서 의미가 있다.


