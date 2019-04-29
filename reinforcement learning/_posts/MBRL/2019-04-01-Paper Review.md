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


--------


### Reinforcement Learning from Demonstration

-   두 개의 task는 demonstration에서의 학습을 살펴보는 basis.
    -   Nonlinear task: pendulem swing-up with limited torque
    -   (Approximately) Linear task: cart-pole
    -   두 task에서, learner는 one-step reward $$r$$이 주어지고, continuous state 및 action 문제로 formulation.
    -   각각의 목표는 __infinite horizon discounted reward를 최소화하는 policy를 찾는 것__

여기서, 왼쪽 eqn: continous time formulation, 오른쪽 eqn: discrete time version. 
x: n-차원 state vector, u: m차원 입력 vector.

$$
\begin{aligned}
V(x(t)) = \int^{\infty}_t e^{-\frac{(s-t)}{\tau}} r(x(s),u(s)) ds \quad or \quad V(x(t)) = \sum^{\infty}_{i=t} \gamma ^{i-t} r(x(i), u(i))
\end{aligned}
$$

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



##### V-Learning

-   Swing-Up에 대해 demonstration의 benefit을 평가하기 위해서, _Doya's(1996)_ 에서 제안된 __continous TD(CDT) 학습으로 V-learning을 구현__.
    -   V-function 및 dynamics model은 nonlinear function approximator(Receptive Field Weighted Regression-RFWR))에 의해 점차적으로 학습됨.
        -   _NN의 발전이 크지않아 approximator를 다른 방법으로 사용한 것 같음_
    -   _Doya's_ 방법과 다른 것은 policy $$\pi$$ (__"actor"__ as in _Barto_ Barto et al. (1983))의 __model를 학습하기 위해 CTD에서 제안된 optimal action을 사용__ (RFWR으로 표현됨).

-   다음의 학습 condition들은 경험적으로 실험:
    -   a) _Scratch_  : 처음부터 value function _V_, model _f_, actor $$\pi$$ 의 trial by trial learning.
    -   b) _Primed Actor_ : demonstration에서의 $$\pi$$의 초기 training 이후, trial by trial learning.
    -   c) _Primed Model_: demonstration에서 _f_ 의 초기 training 이후, trial by trial learning.
    -   d) _Primed Actor & Model_ : b) 및 c)에서의 $$\pi$$ 와 _f_의 priming 이후, trial by trial learning.


<figure>
  <img alt="An image with a caption" src="/assets/img/Paper/LearnFromDemo/1.png" class="lead"   style="width:320px; height=:600px"/>
</figure>

-   Fig. 2는 Swing-up 학습 결과
    -   각 trial은 60초 지속
    -    Time $$T_{up}$$은 각 trial 동안 $$\theta \in [-\pi/2, \pi/2] 에서 pole이 보내는 시간.
-    a) 와 c)를 비교: Demonstration에서의 pole model 학습은 가속화되지 않음.
     -    당연한 결과: V-function을 학습하는 것은 model을 학습하는 것보다 상당히 복잡하고, 이것은 학습 과정은 V-function learning에 의해 우선시(dominate)된다.
     -    흥미롭게도, demonstration에서 action를 priming은 초기 performance(condition a vs. b)에서 상당한 효과를 가진다.
-   시스템은 pendulum을 pump up 시키는 올바른 방법을 알고 있지만 __upright position에서 pendulum을 balancing하기 위해서는, 결국 처음부터 학습하는 것처럼 동일한 시간이 소요__.
    -   이 행동은 이론적으로 __V-function이 전체 state-action space가 densely explore 되어지면 유일하게 정확하게 근사__ 되어질 수 있기 때문.
    -   _전체 state-action space에 대한 정보를 알아야 최적의 해를 구할 수 있음_
    -   만약 demonstration이 전체 state space의 많은 부분을 cover하는 경우에는, V-learning은 이익을 볼 수 있을것이라 예상.
-   V-function만을 prime거나 다른 함수들을 가지고 조합하는 demonstration을 사용하는 것도 조사.
    -   이 결과들은 정량적으로 Fig. 2와 동일:
        -   만약 policy가 priming에 포함되어 있다면, learning traces는 b), d)와 같고 다른 경우는 a), c)
    -   다시말하지만, 이것은 전체적으로 놀라운 결과는 아니다.
        -   V-function을 근사시키는 것은 단순히 $$\pi, f$$에 관한 supervised learning이 아닌, (2)(consistency equation)의 validity(타당성)를 확인하기 위한 iterative 절차를 요구하고 복잡한 nonstationary funtion approximation 과정에 해당.
        -   Demonstration으로부터 data가 제한되면, 일반적으로 좋은 value function을 근사는 불가능.


##### Model-Based V-Learning

-   __model _f_ 를 학습하는 것은, 이를 더욱 강력하게 사용가능__.
    -   [certainty equivalence의 원리](https://en.wikipedia.org/wiki/Stochastic_control#cite_note-Chow-2)에 따르면, _f_ 는 real world를 대체할 수 있고 real world와 interaction대신에 "mental simulations"에서 planning이 동작될 수 있음.
    -   RL에서, 이 idea는 원래 discrete state-action space에 대한 Sutton's(1990) DYNA 알고리즘에 의해 제안.
    -   여기서 DYNA, DYNA-CTD의 continuous 버전이 얼마나 learning from demonstration에서 도움이 될 수 있는지를 탐구(explore)할 것.
    -   Section 2.1.1(V-Learning)에서 CTD와 비교하여 얻은 유일한 차이점은 모든 real trial 이후에, DYNA-CTD는 __지금까지 획득한 dynamics 모델이 실제 pole dynamics를 대체하는 다섯 번의 "mental trials"을 수행.__
    -   두 개의 learning conditions:
        -   _Scratch_: 처음부터 _V_, model _f_, policy $$\pi$$의 trial by trial learning.
        -   _Primed Model_: demonstraion으로부터 _f_의 초기 training 이후, trial by traial learning.
    -   Fig. 3.는 이전 section에서 V-learning과 대조적.
        -   __Learning from demonstraion은 상당한 차이를 보임__
            -   Stable balancing을 가지는 좋은 swing-up을 성취하기 위해서는 단지 demonstration 이후 2-3 traial만 필요, $$T_{up}$$ > 45s.



<figure>
  <img alt="An image with a caption" src="/assets/img/Paper/LearnFromDemo/2.png" class="lead"   style="width:320px; height=:600px"/>
</figure>

> Note that
> > 처음부터 학습하는 것도 Fig. 2.보다 상당히 빠름.



#### The Nonlinear task: CART-POLE BALANCING

-   Swing-Up task에 대한 demonstration으로부터 RL를 적용하는 것은 시기상조.
    -   __Nonlinear function approximation을 가진 RL은 아직 적절한게 없음__(yet to obtain appropriate scientific understanding).
    -   따라서, 이 section에서 좀 더 쉬운 task(cart-pole balancer)로 변경.
    -   이 task는 pole이 upright position에 가까울 때, approximately linear.
    -   이 문제는 LQR(_Dyer & McReynolds, 1970_)의 맥락에서 DP literature에서 잘 연구됨.



#### Q-Learning

-   V-learning과 대조적으로, Q-learning(_Watkins, 1989; Singh & Sutton, 1996_)은 value function보다 더욱 복잡, Q(x, u), state 및 command(input)에 의존.
-   consistency equation (2)와 유사한 Q-learning:


$$
\begin{aligned}
Q(x(t), u(t)) = r(x(t), u(t)) + \gamma argmin_{u(t+1)} (Q(x(t+1), u(t+1))  \qquad(3)
\end{aligned}
$$

-   매 state __x__ 마다, reward function (1)하에서 optimal action이고 Q를 최소화하는 action __u__ 를 선택.
    -   장점ㅇㄴㅇ










