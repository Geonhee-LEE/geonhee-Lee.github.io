---
layout: post
title:  "robotics_and_control"
categories: robotics_and_control
tags: digital_motion_control
comments: true
---

# Digital Motion Control System - Lecture1: Review

## Table of Contents
{:.no_toc}
1. this unordered seed list will be replaced by toc as unordered list
{:toc}

## Review of Classical Control

### Introduction

An _n_ th order linear time-invariant(LTI) can be described by a linear constant coefficient differential equation of the form

$$
\begin{aligned}
y^{(n)} +a_{n-1} y^{(n-1)} + ... + a_1 \dot{y} + a_0 y = b_m u^{(m)} + b_{m-1} u^{(m-1) } +...+ b_1 \dot{u} + b_0 u
\end{aligned}
$$

where apprpriate initial conditions are specified for the output, _y(t)_ of the system. Laplace transforming the above equation, we get

$$
\begin{aligned}
Y(s) = \frac{N(s)}{D(s)} U(s) + \frac{IC(s)}{D(s)} = G(s)U(s) + \frac{IC(s)}{D(s)}
\end{aligned}
$$

where

$$
\begin{aligned}
G(s) = \frac{b_m s^m +b_{m-1} u^{m-1 + ... + b_1 s b_0}}{s^n + a_{n-1} s^{n-1} +...+ a_1 s a_0} = \frac{N(s)}{D(s)}
\end{aligned}
$$

__IC(s)__ : a polynimial in s, a complex variable, containing the terms arising from the initial conditions.

__The total response(Y(s))__: __sum__ of the contributions from the initial conditions and  the system input.


If the system is unforced(i.e., U(s) = 0), we call the resulting response the __zero-input response(ZIR)__ of the system. Likewise, if all initial conditions are zero(i.e., IC(s) = 0), the forcing function produces the __zero-state response(ZSR)__.

















<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/1.png" class="lead" data-width="240" data-height="180" />
</figure>


###  Tool(End-effector) Orientation
Rotation matrix:
$$
\begin{aligned}
\qquad \begin{bmatrix}
                 i_2  \cdot i_0  & j_2  \cdot i_0  \\
                 i_2  \cdot j_0  & j_2  \cdot j_0          
               \end{bmatrix}
&=
\begin{bmatrix}
                 cos(\theta_1 + \theta_2)  & -sin(\theta_1 + \theta_2) \\
                 sin(\theta_1 + \theta_2)   & cos(\theta_1 + \theta_2)          
               \end{bmatrix}
\end{aligned} 
 $$

 > Note: __Denavit-Hartenberg Convention & Homogeneous transformation__ is needed to express these relationship.



--------------




------------

> Reference:
- [SEOULTECH - Digital Motion Control System, Lecture material]()
- [DSP로 리니어모터 제어하기](http://www.kangcom.com/sub/view.asp?sku=200312230002)