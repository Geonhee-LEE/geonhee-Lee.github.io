---
layout: post
title:  "RL"
categories: RL
tags: MBRL
comments: true
---

# [PAPER-Review] Model-based Reinforcement learning

## Learning From Demonstration Stefan, Stefan Schaal, 1997

### Abstract

-   1997년도에는 사전지식 없이 처음(scratch)부터 task를 학습하는 것은 어려운 일.
    -   그러나, 사람은 드물게 처음부터 학습을 시도.

-   Control를 학습하기 위해, 이 논문은 __RL의 맥락에서 demonstration으로부터 어떻게 학습을 적용되는 지를 조사__
    -   Demonstration들이 학습을 가속화하는 가능한 area인 task dynamics의 model, Q-function, Value-function, policy를 준비하는 것을 고려. (__model, value function등을 사용하여 학습을 가속화__ 하겠다.)
-   일반적인 nonlinear learning 문제에서 오직 MBRL만 demonstration이후에 상당한 속도향상을 보여주지만, LQR 문제의 경우(선형)에는 모든 method들은 demonstration으로부터 이익을 본다.
-   이 논문에서 시연하는 복잡한 anthromorphic robot arm에서 pole balancing 구현에서는, __실제 signal processing의 복잡함에 직면할때 MBRL은 LQR 문제에 대해 가장 큰 robustness 를 제공__.

----


### Introduction

-   $$1.$$ Inductive supervised learning 방법은 정교함의 높은 수준에 도달.
    -   Nature에 대한 사전 지식들이나 data set이 주어지면, error criterion를 최소화하여 data로부터 structure를 추출하는 알고리즘의 host가 존재.
-   그러나, Control를 학습하는 방법에서는 task를 학습하는 것은 잘 정의되지 않음
    -   여기서, __목표는 policy를 학습하는 것__.
    -   i.e., task를 성취하기 위한 dynamical system으로 유도하는 거나 인지한 state에 반응하여 적절한 행동을 하는 것.
    -   Task는 일반적으로 임의의 performance index의 관점에서 묘사되기 때문에, 지도하는 방법으로 controller를 학습하는 __data를 직접적 학습하는 것은 존재하지 않음__
        -   더욱 문제가 되는 것은, __performance index는 task의 long term 행동에 걸쳐 정의__ 가 되며 현재 performance에 대해 과거의 행동이 얼마나 좋은지(credit) 안좋은지(blame)를 결정하는 temporal credit assignment 문제가 발생.
        -   위와같은 setting(RL에 대해서는 일반적)에서, 처음부터 task를 학습하는 것은  __good policy를 찾기 위해서__ state-action space의 exploration의 time-consuming amount(소요시간)을 상당하게 요구.


-   $$2.$$ 반대로 사전 지식 없이 학습하는 것은 인간이나 동물이 거의 취하는 않는 접근
    -   New task에 접근하는 방법에 대한 knowledge는 이전에 학습된 task에서 transfer 가능 ang/or teacher의 performance로 추출.
    -   New task를 빠르게 성취하기 위해 다양한 정보로 control을 하는 것은 아직 open questions.
    -   이 논문: __Demonstration에서 학습을 focus 할 것__

-   $$3.$$ Demonstration에서 학습하는 것: "programming by demonstration", "imitation learning" teaching  by showing"으로 알려짐.
    -   목표: 전문가에 의해 assembly task를 로봇하게 단독으로 보여주는 automatic programming process을 시간이 많이 소요되는 로봇의 manual programming을 대체하고자 함.
    -   이 논문: __RL과 demonstration으로부터 learning이 어떻게 이익이 되는지에 집중__
    -   RL을 2개 카테고리로 분류    
        -   Section2) Nonlinear task에 대한 RL
        -   Section3) (Approximately) linear task에 대한 RL
    -   Q-learning, value-function learning, model-based RL와 같은 method는 demonstration으로부터 data에서 이익을 볼 수 있는 방법을 조사.
        -   Section 2.3, example task, pole balancing은 이를 학습하기 위해 실제 anthropomorphic을 사용.
        -   더욱 복잡한 상황에서 demonstration으로부터 학습의 적용가능성 재고려.



### Reinforcement Learning from Demonstration

-   두 개의 task는 demonstration에서의 학습을 살펴보는 basis.
    -   Nonlinear task: pendulem swing-up with limited torque
    -   (Approximately) Linear task: cart-pole
    -   두 task에서, learner는 one-step reward $$r$$이 주어지고, continuous state 및 action 문제로 formulation.
    -   각각의 목표는 __infinite horizon discounted reward를 최소화하는 policy를 찾는 것__

$$
\begin{aligned}
V(x(t)) = \int^{\infty}_t e^{-\frac{(s-t)}{\tau}} r(x(s),u(s)) ds \quad or \quad V(x(t)) = \sum^{\infty}_{i=t} \gamma ^{i-t} r(x(i), u(i))
\end{aligned}
$$

여기서, 왼쪽 eqn: continous time formulation, 오른쪽 eqn: discrete time version. 
x: n-차원 state vector, u: m차원 입력 vector.

-   Swing-Up에 대해, teacher는 다른 initial condition에서 시작한 5 개의 성공적 시도를 제공한다고 가정.
    -   각각의 시도는 60Hz로 샘플링되는 data vector $$(\theta, \dot{\theta}, \tau)$$의 time series로 구성
-   Cart-Pole에 대해, 성공적인 balancing의 30초 demonstration을 가지며, data vector $$(x, \dot{x}, \theta, \dot{\theta}, F)$$


> 어떻게 RL을 가속화하기 위해 demonstration이 사용되는 방법은?

#### The Nonlinear task: Swing-up

-   Swing-up에 task에 대해서 Value fucntion(V-function) (_Dyer & McReynolds, 1970_)을 학습하는 기반으로 하는 RL을 적용하고, 대안적인 방법으로 Q-learning(_Watkins, 1989_)은 continous state-action space에 대해 아직 limited research를 받음.
    -   V-function은 scalar reward value $$V(x(t))$$  각 state에 대해 assign하고 다음 consistency equation을 만족:

$$
\begin{aligned}
V(x(t)) = argmin_{u(t)} (r(x(t), u(t)) + \gamma V(x(t+1))) \qquad(2)
\end{aligned}
$$

-   이 방정식은 discrete state-action system; continuous formulation은 (Doya(1996))참조
    -   Optimal policy: $$u = \pi(x)$$, (2)를 만족하는 state x에서 action u를 선택.
    -   이 계산은 subsequent state x(t+1)의 knowledge를 포함하는 optimization step를 포함.
        -   따라서, __controlled system 의 dynamics의 model($$x(t+1) = f(x(t), u(t))$$)__ 이 필요. 
    -   Demonstration에서의 학습하는 관점에서는, V-function 학습은 demonstration에서 준비(가공, primed)될 수 있는 3가지 후보를 제공.
        -   Value function $$V(x)$$
        -   Policy $$\pi(x)$$
        -   Model $$f(x,u)$$