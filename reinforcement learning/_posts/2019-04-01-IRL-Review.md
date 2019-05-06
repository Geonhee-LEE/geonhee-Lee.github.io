---
layout: post
title:  "RL"
categories: RL
tags: IRL
comments: true
---

# [PAPER-Review] Inverse Reinforcement learning


## Table of Contents
{:.no_toc}
1. this unordered seed list will be replaced by toc as unordered list
{:toc}


## [Algorithms for Inverse Reinforcement learning, Andrew Y.Ng , Russell, 2000]

### Abstract

- 이 논문은 MDP에서의 _inverse reinforcement learning(IRL)_ 문제를 다룸.
  -  __Optimal behavior를 관측하여 제공된 reward function을 추출하는 문제__.
  - IRL은 natural system에 의해 최적화가 진행되는 reward function을 알아내(ascertaining)거나, 숙련된(skilled) behavior를 얻어내기 위한 견습(apprenticeship)에 유용.
  
  - 모든 reward function의 집합에 주어지는 __policy가 optimal이라고 특성__ 을 부여(characterize).
    - Then, IRL에 대한 3 가지 알고리즘 유도.
  
1.  Policy를 알고 있는 경우에 대해 다루며, finite state space의 tabulated reward function을 다룸.
2.  Policy를 알고 있는 경우에 대해 다루며, potentially infinite state space(=large state space)에 대해서 reward function의 linear functional approximation을 다룸.
3.  관측된 trajectory들의 finite set을 통해서만 policy를 알 수 있는 현실 문제에 대해서 다룸.

> 위의 3가지 경우에서 핵심 이슈는 _degeneracy_: 관측되는 policy가 optimal인 reward function들의 large set의 존재 유무(existence). 
> > Degeneracy를 제거하기 위해서, __sub-optimal policy들에 대해 observed policy을 최대한 구별짓는(differentiate) reward function을 선택__ 하는 몇가지 natual heuristics를 제안(따라서, 효율적으로 풀 수 있는 __linear programming__ formulation으로 품). 
> > _(comment) degeneracy를 제거는 유일 해를 찾기 위한 과정처럼 보임. 몇 가지 constraints(여기서 말하는 휴리스틱 방법)를 추가하여 우리가 관측하는 policy(reward fucntion)가 유일한 optimal policy이기를 바람._

- 저자는 단순한 discrete/finite(위에서 1번째 case) 및 continuous/infinite state(위에서 2번째 case) 문제에 대해 실험.

### Introduction

-  IRL문제는 
































<figure>
  <img alt="An image with a caption" src="/assets/img/Paper/LearnFromDemo/6.png" class="lead"   style="width:180px; height:=480px"/>
</figure>

$$
\begin{aligned}
uml cos \theta + \ddot{\theta} m l^2 - mgl sin \theta, \ddot{x} u  \quad (7)
\end{aligned}
$$





## Reference

- [Algorithms]