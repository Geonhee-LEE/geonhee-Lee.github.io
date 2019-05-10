---
layout: post
title:  "Machine leaning"
categories: machine_learning
tags: 
comments: true
---

# A First course in Probability: Chapter3 조건부 확률과 독립

## Table of Contents
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}


-------------


### 서론

-  확률론에서 가장 중요한 개념 중의 하나인 __조건부 확률__ 을 소개.
   - 이 개념의 중요성:
     - $$1.$$ 실험의 결과에 관한 어떤 부분적인 정보를 이용할 수 있을 때 확률을 계산하는 데 종종 관심이 있다(이러한 경우에 구하는 확률이 __조건부 확률__)
     - $$2.$$ 부분적인 정보를 전혀 이용할 수 없더라도 조건부 확률은 종종 구하려는 확률을 더욱 쉽게 계산하기 위하여 이용.

$$
\begin{aligned}
P(E \mid F)
\end{aligned} 
$$


-------


### 조건부 확률

- 방금 구한 확률을 F가 발생했다는 조건이 주어졌을 때 E가 발생할 __조건부 확률(conditional probability)__ 이라 하며 다음과 같이 나타낸다.
  
> 정의
> > 만일 P(F) $$>$$ 0 이면 다음과 같이 정의한다

$$
\begin{aligned}
P(E|F) = \frac{P(EF)}{P(F)} \qquad (2.1)
\end{aligned} 
$$


- 위 식(2.1)의 양변에 P(F)를 곱하면 다음 식을 얻는다. 
  - E와 F가 동시에 발생할 확률은 F가 발생할 확률과 F가 발생했다는 조건이 주어졌을 때 E의 조건부 확률을 곱한 것임을 의미.
  - 아래의 식은 교사건의 확률을 계산하는데 유용

$$
\begin{aligned}
P(EF) = P(F)P(E \mid F) \qquad (2.2)
\end{aligned} 
$$

- 임의의 개수의 사건들에 대해 교집합의 확률에 관한 식을 제공하는 식(2.2)를 일반화시킨 다음 식을 가끔 __곱셈법칙(multiplication rule)__ 이라 한다.

> 곱셈법칙
> >  $$P(E_1 E_2 E_3 \cdots E_n) = P(E_1)P(E_2 \mid E_1)P(E_3 \mid E_1 E_2)\cdots P(E_n \mid E_1 \cdots E_{n-1})$$

-------

### 베이즈 공식

- E와 F를 사건.
- E는 다음과 같이 나타낼 수 있다
  - 왜냐하면, 한 결과가 E에 포함되기 위해서는 그것이 E와 F에 모두 포함되거나 E에는 포함되지만 F에는 포함되지 않거나 둘중 하나이기 때문.
  

$$
\begin{aligned}
E = EF \bigcup EF^C
\end{aligned} 
$$

-  EF와 $$EF^C$$ 는 명백히 상호배반이므로 공리 3에 의해 다음을 얻는다.

$$
\begin{aligned}
P(E) &= P(EF) + P(EF^C) \\
&= P(E \mid F) P(F) + P(E \mid F^C) P(F^C) \\
&= P(E \mid F) P(F) + P(E \mid F^C) [1-P(F)] \qquad (3.1)
\end{aligned} 
$$

- 식 (3.1)은 사건 E의 확률은 F가 발생했다는 조건이 주어졌을 때 E의 조건부 확률과 F가 발생하지 않았다는 조건이 주어졌을 때 E의 조건부 확률의 가중평균으로 각 조건부 확률의 가중치가 조건으로 주어진 사건이 발생할 확률임을 의미.
  - 이 식은 먼저 __어떤 다른 사건의 발생 여부를 조건으로 하여 사건의 확률을 결정할__  수 있기 때문에 매우 유용한 공식.
  - 즉, 직접 사건의 확률을 계산하기는 어렵지만 어떤 다른 사건의 발생 여부를 알면 그것을 계산하는 것이 간단한 예가 많이 있다.


- 새로운 증거가 나타날 때 어떤 가설에 대한 확률의 변화는 이 가설의 __오즈__ 또는 __승산__ (odds)에 있어서의 변화로 간편하게 표현될 수 있다.
  
> 정의
> > 사건 A의 오즈는 다음과 같이 정의
> 
> >  $$\frac{P(A)}{P(A^C)} = \frac{P(A)}{1-P(A)}$$
> 
> > 즉, 사건 A의 오즈는 사건 a가 발생할 가능성이 사건 A가 발생하지 않을 가능성보다 얼마나 더 큰지를 설명.

예를 들어, 만일 P(A) = 2/3이면, P(A) = 2P($$A^C$$)이므로 오즈는 2. 만일 오즈가 $$\alpha$$이면, 그 가설을 지지하는 데에 있어 승산이 '$$\alpha$$ 대 1'이라고 흔히 말한다.

- 이제 그것이 참일 확률이 P(H)인 새로운 가설 H를  생각
  - 새로운 증거 E가 나타났다고 하면, 증거E가 주어졌을 때 H가 참일 조건부 확률과 H가 참이 아닐 조건부 확률은 다음과 같다.

$$
\begin{aligned}
P(H \mid E) = \frac{P(E \mid H)P(H)}{P(E)} \qquad P(H^C \mid E) = \frac{P(E \mid H^C) P(H^C)}{P(E)}
\end{aligned} 
$$

- 그러므로 __증거 E가 나타난 후 새로운 오즈__ 는 다음과 같다.

$$
\begin{aligned}
\frac{P(H \mid E)}{ P(H^C \mid E) } = \frac{P(H)}{P(H^C)} \frac{P(E \mid H)}{P(E \mid H^C)} \qquad (3.3)
\end{aligned} 
$$


-------

### 독립사건

- 앞의 예제들은 F가 주어졌을 때 E의 조건부 확률 P(E$$\mid$$F)는 일반적으로 E의 비조건부 확률 P(E)와 같지 않음을 보였다.
  - 즉, F가 발생했다는 것을 아는 것은 일반적으로 E의 발생 가능성을 변화.
  - 특별한 경우로 P(E$$\mid$$F)가 P(E)와 같아지는 경우, E와 F는 __독립__.
  
$$
\begin{aligned}
 P(EF) = P(E)P(F) \qquad (4.1)
\end{aligned} 
$$

이면 E와 F는 __독립__.

  
-------

### 확률 $$P(\cdot \mid F)$$

- 조건부 확률은 일반적인 확률의 성질을 모두 만족
  - $$0 \leq P(E \mid F) \leq 1$$
  - $$P(S \mid F) = 1$$
  - 상호배반인 사건에서 성립하는 것.


- 확률론에서 중요한 개념은 사건들의 __조건부 독립__.
  - F가 발생했다고 할 때 $$E_1$$이 발생할 조건부 확률이 $$E_2$$의 발생 여부에 관한 정보에 의해 변화되지 않는다면, 사건 $$E_1, E_2$$는 사건 F가 주어졌을 때 __조건부 독립(conditionally independent)__
  - 더 공식적으로 만일 다음을 만족하면 사건 $$E_1, E_2$$는 F가 주어졌을 때 조건부 독립.
  - 조건부 독립의 개념을 3개 이상의 사건에 쉽게 확장.

$$
\begin{aligned}
P(E_1 \mid E_2 F) = P(E_1 \mid F) \qquad (5.11)\\
P(E_1 E_2 \mid F) = P(E_1 \mid F) P(E_2 \mid F) \qquad (5.12)
\end{aligned} 
$$

- 순차적인 정보의 갱신
  - 초기확률(사전확률, prior probability)들이 P($$H_i), \sum^{n} _{i=1} P(H_i) = 1$$인 n개의 상호배반이고 전체를 이루는 가능한 가설이 있다고 가정.
  - 이제 만일 사건 E가 발생했다는 정보를 들으면, $$H_i$$ 가 참인 가설일 조건부 확률(사후확률, posterior probability)은 다음과 같다.

$$
\begin{aligned}
P(H_i \mid E ) = \frac{P(E \mid H_i) P(H_i)}{\sum _j P(E_1 \mid H_j) P(H_j)} \qquad (5.13)
\end{aligned} 
$$



------------

> Reference:
- [A First course in Probability](https://www.google.com/search?q=A+First+course+in+Probability&sa=X&biw=1920&bih=1001&tbm=isch&source=iu&ictx=1&fir=P9oQKe0gkGhTuM%253A%252Cf5mWCrLP6qSw-M%252C%252Fm%252F059d4pd&vet=1&usg=AI4_-kSMsZZbulXdZWW0E_hWZUuqeKinHA&ved=2ahUKEwj1zuq4zP_hAhVIzbwKHWybAS0Q_B0wCnoECAsQEQ#imgrc=P9oQKe0gkGhTuM:)와