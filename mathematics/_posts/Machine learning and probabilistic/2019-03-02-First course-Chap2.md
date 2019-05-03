---
layout: post
title:  "Machine leaning"
categories: machine_learning
tags: 
comments: true
---

# A First course in Probability: Chapter2 확률의 공리

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
    - 영사건(null event): 어떠한 결과도 포함하지 않음, $$\emptyset$$
      - 만일 EF = $$\emptyset$$이면, E와 F는 _상호배반(mutually exclusive)
    - 임의의 사건 E에 대해 E에 속하지 않는 표본공간 S의 모든 결과로 구성된 새로운 사건: $$E^c$$, E의 여사건(complement)

- 합사건, 교사건, 여사건에 대한 세 가지 기본 연산들 사이의 유용한 관계: 드모르간 법칙( _DeMorgan's laws_ )

### 확률의 공리

- 사건의 확률을 정의하는 한가지 방법: 그 사건의 __상대도수__ 에 의한 것
  - 표본공간이 S인 어떤 실험이 정확히 동일한 조건하에서 반복적으로 실행된다고 가정. 표본공간 S의 각 사건 E에 대해 n(E)를 그 실험을 처음 n번 반복할 때 사건 E가 발생한 횟수로 정의.
  - 사건 E의 확률 P(E)의 정의:

$$
\begin{aligned}
P(E) = lim _{n \rightarrow \infty} \frac{n(E)}{n}
\end{aligned} 
$$

- 즉, P(E)는 사건 E가 발생한 횟수의 (극한)비율로 정의. 
  - 따라서, P(E)는 E의 극한 상대도수


> Note
> > 실제로는 극한비율을 얻지 못함
> > 일정한 극한 값에 수렴한다는 가정을 하거나 __공리__ (axiom)를 언급하여 이러한 문제점 해결(확률론에서 자명한 해결방법, 명백한 공리를 가정하고 나서 일정한 극한 도수가 어떤 의미에서 존재함을 증명하는 것)

- 확률의 세 가지 공리
  - 공리 1: $$0 \geq P(E) \geq 1$$
  - 공리 2: $$P(S) = 1$$
  - 공리 3: 임의의 상호배반인 일련의 사건들 $$E_1, E_2, ...$$ 즉, $$i \neq j$$ 일 때 $$E_i E_j = \emptyset$$ 인 사건들)에 대해 아래의 식이 성립. 
    - P(E)를 사건 E의 확률(probability)

$$
\begin{aligned}
P( \bigcap_{i=1}^{\infty} E_i) = \sum ^{\infty} _{i=1} P(E_i)
\end{aligned} 
$$

### 균등확률 결과를 갖는 표본공간

- 많은 실험에서 표본 공간에 속하는 모든 결과가 균등하게 발생한다고 가정.
  - 즉, 표본 공간 S가 유한 집합 S= {1, 2, .., N}인 실험을 생각

$$
\begin{aligned}
P({1}) = P({2}) = \cdots = P({N}) 
\end{aligned} 
$$

으로 가정하는 것이 자연스럽기 때문에 공리 2, 3으로 부터

$$
\begin{aligned}
P({i}) = \frac{1}{N} \qquad i =1,2,...,N
\end{aligned} 
$$

이 식과 공리 3으로부터 임의의 사건 E에 대해

$$
\begin{aligned}
P(E) = \frac{E에 속하는 결과의 개수}{S에 속하는 결과의 개수}
\end{aligned} 
$$



### 연속집합함수인 학률

일련의 사건들이 만일 다음을 만족하면 __증가하는 사건열__ 이라 한다.

$$
\begin{aligned}
E_1 \subset E_2 \subset \cdots \subset E_n \subset E_{n+1} \subset \cdots
\end{aligned} 
$$

반면에 만일 다음을 만족한다면 __감소하는 사건열__ 이라 한다

$$
\begin{aligned}
E_1 \supset E_2 \supset \cdots \supset E_n \supset E_{n+1} \supset \cdots
\end{aligned} 
$$

만일 $${E_n, n \leq 1}$$ 이 증가하는 사건열이면 $$lim _{n\rightarrow \infty} E_n$$ 을 다음과 같이 정의


$$
\begin{aligned}
lim _{n\rightarrow \infty} E_n = \bigcup_{i=1}^{\infty} E_i
\end{aligned} 
$$

마찬가지로, 만일 $${E_n, n \leq 1}$$ 이 감소하는 사건열이면 $$lim _{n\rightarrow \infty} E_n$$ 을 다음과 같이 정의


$$
\begin{aligned}
lim _{n\rightarrow \infty} E_n = \bigcap_{i=1}^{\infty} E_i
\end{aligned} 
$$


### 신뢰의 측도인 확률

- 이제까지 주어진 실험에 대한 사건의 확률을 실험이 계속 반복될 때 스 사건이 얼마나 자주 발생하는 지의 측도로 해석
- 그러나 역시 확률 용어의 또 다른 용도
  - e.g., ~ 가능성은 90%이다, ~확률은 0.4이다
- 가장 간단하고 자연스러운 해석은 확률을 각각 언급한 내용에서 개인적인 신뢰의 정도에 관한 측도로 적용
  - 즉, 개별적인 신뢰의 정도애 관한 측도로서의 확률의 해석을 종종 __확률의 개인적(personal) 또는 주관적(subjective) 개념__ 이라 한다.


------------

> Reference:
- [A First course in Probability](https://www.google.com/search?q=A+First+course+in+Probability&sa=X&biw=1920&bih=1001&tbm=isch&source=iu&ictx=1&fir=P9oQKe0gkGhTuM%253A%252Cf5mWCrLP6qSw-M%252C%252Fm%252F059d4pd&vet=1&usg=AI4_-kSMsZZbulXdZWW0E_hWZUuqeKinHA&ved=2ahUKEwj1zuq4zP_hAhVIzbwKHWybAS0Q_B0wCnoECAsQEQ#imgrc=P9oQKe0gkGhTuM:)