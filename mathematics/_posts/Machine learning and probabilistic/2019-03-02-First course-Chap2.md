---
layout: post
title:  "Machine leaning"
categories: machine_learning
tags: 
comments: true
---

# A First course in Probabilistic: Chapter2 확률의 공리

## Table of Contents
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

-------------

### 서론

-  우선 사건에 대한 확률의 개념 소개.
   -  이 확률이 어떤 상황에서 어떻게 계산될 수 있는 지 보여줌
-  실험의 표본 공간, 사건의 개념 습득


### 표본공간과 사건

- 확실한 결과를 예측할 수 없는 실험.
  - 모든 가능한 결과의 집합은 알 수 있음.
  - 실험의 모든 가능한 결과의 집합 : __표본공간(Sample space) S__
  - 표본 공간의 임의의 부분집합 : __사건(event) E__
    - 사건 E와 사건 F의 합사건(union), E 혹은 F가 발생할때: E $$\bigcup$$ F
    - 사건 E와 사건 F의 교사건(intersection), E와 F가 모두 발생할때: E $$\bigcap$$ F
    - 영사건(null event): 어떠한 결과도 포함하지 않음, $$\\emptyset$$
      - 만일 EF = $$\emptyset$$이면, E와 F는 _상호배반(mutually exclusive)
    - 임의의 사건 E에 대해 E에 속하지 않는 표본공간 S의 모든 결과로 구성된 새로운 사건: $$E^c$$, E의 여사건(complement)

- 합사건, 교사건, 여사건에 대한 세 가지 기본 연산들 사이의 유용한 관계: 드모르간 법칙( _DeMorgan's laws_ )

### 확률의 공리



$$
\begin{aligned}
P(|\hat{\theta} - \theta ^* \geq \epsilon|) \leq 2 e ^{-2N\epsilon^2}
\end{aligned} 
$$

- Can you calculate the required number of trials, N?
  - To obtain $$\epsilon$$ = 0.1 with 0.01% case

> Note
> > This is  Probably Approximate Correct(PAC) learning



---------


## Maximum a Posteriori Estimation

### Incorporating Prior Knowledge
 
-  Merge the __previous knowledge__ to my trial
   -  $$P(\theta|D) = \frac{P(D|\theta)P(\theta)}{P(D)}$$
      -  Posterior = (Likelihood x Prior Knowledge) / Normalizing Constant
   -  Already dealt with $$P(D \mid \theta ) = \theta^ {a_H} (1- \theta) ^{a_T}$$
      -  $$\theta$$라고 가정하고 이러한 data가 관측될 확률. 
   -  $$P(\theta)$$ is part of __the pror knowledge__
      -  왜 압정의 앞/뒤가 이런 확률로 나올까?
      -  왜 사람이 이렇게 행동할까?
   -  P(D)는 많이 중요하지 않음
      -  사실 $$\theta$$에 대해 관심이 있음.
      -  여러번 시도를 해볼 수 있고, 요즘 data가 많음.
   - $$P( \theta \mid D)$$ : Poseterior
     - Then, it is the conclusion influenced by the __data__ and the __prior knowledge__? Yes, it will be our future prior knowledge!
     - 즉, data가 주어졌을 때 $$\theta$$가 사실일 확률을 표현

### More Formula from Bayes Viewpoint

- $$P(\theta|D) \propto P(D|\theta) P(\theta)$$
  - $$P(D|\theta) = \theta^{a_H} (1-\theta)^{a_T}$$
  - $$P(\theta)$$ = ????
    - 어떤 distribution(e.g. binomial distribution)에 의존하여 표현해야함.
    - 50%, 50% 사전 지식을 그대로 사용 불가.
  - 여러 가지 방법이 있으나, 하나의 방법:
    - __Beta distribution__, 특정범위내에서 [0,1]로 결합되어 있는 CDF로 표현하여 확률 성격을 잘 표현(아래그림참조)
    - $$P(\theta) = \frac{\theta ^{\alpha-1} (1-\theta)^{\beta -1 }}{B(\alpha, \beta)}, B(\alpha, \beta) = \frac{\Gamma(\alpha)\Gamma(\beta)}{\Gamma(\alpha+\beta)}, \Gamma (\alpha) = (\alpha -1 )!$$

<figure>
  <img alt="An image with a caption" src="/assets/img/ML/week1/1.png" class="lead" style="width:240px; height=:180px" />
</figure>

- Response is
  - $$P(\theta) \propto P(D|\theta) P(\theta) \propto \theta ^{a_H} (1-\theta)^{a_T }\theta ^{\alpha-1} (1-\theta)^{\beta -1 } = \theta ^{a_H + \alpha-1} (1-\theta)^{a_T + \beta -1 }$$
  
### Maximum a Posterior Estimation

- Goal: the most probable and more approximate $$\theta$$
- Previously in MLE, we found $$\theta$$ from $$\hat{\theta} = argmax_\theta P(D\mid \theta)$$ 
  - $$ P(D| \theta) = \theta ^{a_H} (1-\theta)^{a_T }$$ 
  - $$\hat{\theta} = argmax_\theta P(D| \theta)$$
- Now in MAP, we find $$\theta$$ from $$\hat{\theta} = argmax_\theta P(\theta \mid D)$$ 
  - $$ P( \theta|D) \propto \theta ^{a_H + \alpha-1} (1-\theta)^{a_T + \beta -1 }$$ 
  - $$\hat{\theta} = \frac{ \theta ^{a_H + \alpha-1} }{ \theta ^{a_H + \alpha + a_T + \beta -2} }$$
- The calculation is __same__ because anyhow it is the maximization.


> Note
> > Likelihood에 대해 maximization을 하는 것(MLE)이 아니고, posterior에 관하여 maximization(MAP).

### Conclusion from Anecdote

- If $$a_H$$ and $$a_T$$ become big, $$\alpha$$ and $$\beta$$ becomes nothing..
  - But, $$\alpha$$ and $$\beta$$  are important if I don't give you more trials. 
  - $$\alpha$$ and $$\beta$$을 선택함에 따라 결과가 달라짐.

<figure>
  <img alt="An image with a caption" src="/assets/img/ML/week1/2.png" class="lead" style="width:240px; height=:125px" />
  <img alt="An image with a caption" src="/assets/img/ML/week1/3.png" class="lead" style="width:240px; height=:120px" />
</figure>

---------

## Basics

### Probability

- Philosophically, Either of the two
  - Objectivists assign numbers __to describe states__ of events, i.e. counting
  - Subjectivists assign numbers by your __own belief__ to events, i.e. betting
- Mathematically
  - A function with the below characteristics

<figure>
  <img alt="An image with a caption" src="/assets/img/ML/week1/4.png" class="lead" style="width:240px; height=:125px" />
</figure>

$$
\begin{aligned}
P(E) & \in R \\
P(E) & \leq 0 \\
P(\Omega) & = 1 \\
P(E_1 \cup E_2 \cup  \cdots) &= \sum^{\infty}_{i=1} P(E_i) 
\end{aligned} 
$$


### Conditional Probability

- We often do not handle the universe, $$\Omega$$
- Somehow, we always make conditions
  - Assuming that the parameters are X, Y, Z, ...
  - Assuming that the events in the scope of X, Y, Z, ...


<figure>
  <img alt="An image with a caption" src="/assets/img/ML/week1/5.png" class="lead" style="width:240px; height=:125px" />
</figure>

- $$P(A \mid B) = \frac{P(A\cap B)}{P(B)}$$
  - The __conditional probability__ of A given B
- Some handy formular 
  - $$P(B \mid A) = \frac{P(A \mid B) P(B)}{P(A)}$$
    - Nice to see that we can __switch the condition and the target event__
  - __Poseterior = (Likelihood X Prior Knowledge) / Normalizing  Constant__
  - $$P(A) = \sum_{n} P(A \mid B_n) P(B _n)$$
    - Nice to see that we can __recover the target__ event by adding the whole conditional probs and priors


### Probability Distribution

- Probability distribution assigns
  - A probability to a __subset of the potential events__ of a random trial, experiment, survey, etc.
- A function mapping an event to a probability
  - Because we call it a probability, the probability should keep its own characteristics(or axiomes)
  - An event can be 
    - A continuous nemeric value from surveys, trials, experiments ...
    - A discrete categorical value from surveys, trials, experiments ...
- For example,
  - f: a probability distribution function
  - x: a continous value
  - f(x): assigned probs
$$
\begin{aligned}
f(x) = \frac{e^{- \frac{1}{2} x^2}}{\sqrt{2 \pi}}
\end{aligned} 
$$

<figure>
  <img alt="An image with a caption" src="/assets/img/ML/week1/6.png" class="lead" style="width:480px; height=:240px" />
</figure>

### Normal Distribution

- Very commonly observed distribution
  - Continuous numerical value

- $$f(x; \mu, \sigma) = \frac{1}{\sigma \sqrt{2\pi}} e ^{- \frac{(x - \mu)^2}{2 \sigma ^2}}$$
- Notation: $$N(\mu, \sigma ^2)$$
- Mean: $$\mu$$
- Variance: $$\sigma^2$$

<figure>
  <img alt="An image with a caption" src="/assets/img/ML/week1/7.png" class="lead" style="width:240px; height=:125px" />
</figure>

### Beta Distribution

- Supports a closed interval 
  - Continuous numerical value
  - [0,1]
  - Very nice characteristic
  - Why?
    - Matches up the characteristics of probs
- f($$\theta$$; \$$\alpha, \beta$$) = $$P \frac{\theta ^{\alpha-1} (1-\theta)^{\beta -1 }}{B(\alpha, \beta)}, B(\alpha, \beta) = \frac{\Gamma(\alpha)\Gamma(\beta)}{\Gamma(\alpha+\beta)}, \Gamma (\alpha) = (\alpha -1 )!$$
- Notation: Beta($$\alpha, \beta$$)
- Mean: $$\frac{\alpha}{\alpha + \beta}$$
- Variance: $$\frac{\alpha \beta}{(\alpha + \beta)^2 (\alpha + \beta +1)}$$

<figure>
  <img alt="An image with a caption" src="/assets/img/ML/week1/8.png" class="lead" style="width:240px; height=:125px" />
</figure>

### Binomial Distribution

- Simplest distribution for discrete values
  - Bernoulli trial, yes or no
  - 0 or 1
  - Selection, switch ...
- $$f(\theta; n, p) = \binom{n}{k} p^k (1-p)^{n-k}, \binom{n}{k} = \frac{n!}{k! (n-k)!}$$
- Notation: $$B(n,p)$$
- Mean: $$np$$
- Variance: $$np(1-p)$$

<figure>
  <img alt="An image with a caption" src="/assets/img/ML/week1/9.png" class="lead" style="width:240px; height=:125px" />
</figure>

### Multinomial Distribution

- The generalization of the binomial distribution
  - e.g., Choose A, B, C, D, E,....,Z

- $$f(x_1, ..., x_k; n, p_1, ..., p_k) = \frac{n!}{x_1! ... x_k !} p_1^{x_1} \cdot p_k^{x_k}$$
- Notation: Multi(P), $$P=< p_1, ..., p_k>$$
- Mean: $$E(x_i) = np_i$$
- Variance: Var$$(x_i) = np_i (1-p_i)$$

## Summary

- 압정 던지는 시도를 통해서 다음번에 무엇이 나올 지 확률을 계산하는 것은 단순히 (횟수/시도) 되는 것이 아니라, 이항분포 가정-> MLE or MAP(사전지식이용) -> optimization(derivative) 과정을 통해 도출되는 것임을 이해했다.

- MAP는 likelihood를 최대화 하는 MLE 과정에 추가적으로 사전 지식을 사용하기 위해 베이즈 룰을 이용하여 posteriori를 계산하는데 적용이 되었으며, 사전 지식은 Beta 분포(0~1 범위를 가지는 연속분포)라고 가정하여 계산할 수 있었다. 
  - 그러나, 결과적으로 시도 횟수가 많아 지게 되면 MLE, MAP는 동일한 결과를 얻을 수 있다.


------------

> Reference:
- [KAIST - Industrial & Systems Engineering Dept](https://aai.kaist.ac.kr/xe2/)
- [기계 학습, Machine Learning, AAILAB KAIST](https://www.youtube.com/playlist?list=PLbhbGI_ppZISMV4tAWHlytBqNq1-lb8bz)