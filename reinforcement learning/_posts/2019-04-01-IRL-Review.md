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

----------

## [Algorithms for Inverse Reinforcement learning, Andrew Y.Ng , Russell, 2000]

### Abstract

- 이 논문은 MDP에서의 _inverse reinforcement learning(IRL)_ 문제를 다룸.
  -  __Optimal behavior를 관측하여 제공된 reward function을 추출하는 문제__.
  - IRL은 natural system에 의해 최적화가 진행되는 reward function을 알아내(ascertaining)거나, 숙련된(skilled) behavior를 얻어내기 위한 견습(apprenticeship)에 유용.
  
  - 모든 reward function의 집합에 주어지는 __policy가 optimal이라는 특성__ 을 부여(characterize).
    - _Then_, 아래와 같은 3가지 알고리즘 유도.

      1.  Policy를 알고 있는 경우에 대해 다루며, finite state space의 tabulated reward function을 다룸.
      2.  Policy를 알고 있는 경우에 대해 다루며, potentially infinite state space(=large state space)에 대해서 reward function의 linear functional approximation을 다룸.
      3.  관측된 trajectory들의 finite set을 통해서만 policy를 알 수 있는 현실 문제에 대해서 다룸.

> 위의 3가지 경우에서 핵심 이슈는 _degeneracy_:  관측되는 policy가 optimal인 reward function들의 large set의 존재 유무(existence). 
> > Degeneracy를 제거하기 위해서, __sub-optimal policy들에 대해 observed policy을 최대한 구별짓는(differentiate) reward function을 선택__ 하는 몇가지 natual heuristics를 제안(따라서, 효율적으로 풀 수 있는 __linear programming__ formulation으로 품). 
> 
> > _(comment) degeneracy를 제거는 유일 해를 찾기 위한 과정처럼 보임. 몇 가지 constraints(여기서 말하는 휴리스틱 방법)를 추가하여 우리가 관측하는 policy(reward fucntion)가 유일한 optimal policy이기를 바람._

- 저자는 단순한 discrete/finite(위에서 1번째 case) 및 continuous/infinite state(위에서 2번째 case) 문제에 대해 실험.


----------


### Introduction

-  IRL 문제는 _(Russell, 1998)_ 와 같이 비공식적으로 아래와 같이 특징되어(characterize)질 수 있다: 
   -  __Given__ :
      1.  다양한 상황에서, time에 따른 _agent's behavior_ 측정 값.
      2.  필요에 따라, agent에 대한 _sensory input_ 측정 값.
      3.  가능하다면, 환경 모델.
   - __Determine__:
     - 최적화되는 reward function.


- 기존에는 "expert"의 _policy_ 를 학습.
  - __저자는 expert's reward function을 복구하고 desirable behavior를 생성을 제안__
  - RL은 policy보다 reward function이 더욱 _succinct, robust, transferable_ 하다는 전제에 기반.


- 이 논문에서는, 머신러닝관점에서 IRL 문제를 다룸(경제학, 제어에서도 IRL 문제를 풀려는 시도가 존재)
  - _Section 2_. MDP 및  IRL 정의
    - model 및 policy가 완전히 주어진 상태에 focus.
  - _Section 3_. 주어진 policy가 최적가 최적인 모든 reward function들의 집합에 대해 다룸
    - __Problem__: 많은 _degenerate solution_ (e.g., reward function = 0) 들을 포함한 집합에 대해 시연.
    - __Solution__: Sub-optimal policy들과 observed policy의 차이를 최대화하는 __reward function을 식별(identify)하는 heuristics__ 를 통해 문제 해결.
    - _LP(Linear Programming)_ 를 사용하여 discrete case에서 효과적으로 진행.
  - _Section 4._ large or infinite state space에 대해 다룸.
    - Reward function의 tabular representation은 infeasible.
    - 고정된 basis function 및 임의의 linear combination으로 fitting된 reward function을 표현한다면, IRL 문제는 LP로 분류하고 효율적으로 풀 수 있음.
  - _Section 5._ 관측되는 trajectory들의 finite set을 통해서만 policy를 알 수 있는 현실적인 경우에 대해 다룸.
    - 단순히 iterative 알고리즘으로 해결.
  - _Section 6._ 위 세가지 알고리즘 적용.
    - Discrete, continous stochastic navigation 문제. "mountain-car" 문제
    - __모든 경우에 observed behavior를 잘 "설명"할 수 있는 reward function을 복구__ 할 수 있다.
  - _Section 7._  요약 및 후속 연구 방향 소개

----------

### Notation and Problem Formulation

- Notation, definition, basic theorems for MDPs에 대해 소개.
- 우리가 앞으로 다룰 IRL 문제에 대해 정의.


#### Markov Decision Processes

- (finite) MDP: tuple $$(S, A, {P_{sa}}, \gamma, R)$$, where
  - _S_: N개 __states__ 의 finite set.
  - $$A = {a_1, ..., a_k}$$: _k_ __actions__ 의 집합.
  - $$P_{sa}(\cdot)$$ : state _s_ 에서 action _a_ 를 취하는 state __transition probabilities__.
  - $$\gamma \in [0, 1)$$ : __discount factor__.
  - _R_ : $$S \mapsto \mathbb{R}$$ : __reinforcement function__, $$R_{max}$$에 의한 절대 값으로 bounded.

- 설명(exposition)을 단순화 하기 위해서, R(s,a)을 R(s)로 reward 표기, extension은 trivial.
- __Policy__: map $$\pi: S \mapsto A$$ 으로 정의
- __Value function__:  policy $$\pi$$ 에 대해 $$s_1$$에서 평가 될 때 다음과 같다.
  
$$
\begin{aligned}
V ^{\pi} (s_1) = E[R(s_1) + \gamma R(s_2) + \gamma ^2 R(s_3) + \cdots | \pi]
\end{aligned}
$$

where expection is over the distribution of the state sequence $$(s_1, s_2, ...)$$ we pass through when we execute the policy $$\pi$$ strating from $$s_1$$. 


- __Q-function__: 

$$
\begin{aligned}
Q ^{\pi} (s,a) = R(s) + \gamma E_{s' ~ P_{sa}(\cdot)} [V ^{\pi} (s')]
\end{aligned}
$$

(where the notation s' ~ P_{sa}(\cdot) means the expectation is with respect to s' distributed according to P_{sa} (\cdot)). 

----------












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