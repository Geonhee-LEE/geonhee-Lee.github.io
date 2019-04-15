---
layout: post
title:  "robotics_and_control"
categories: robotics_and_control
tags: robotics
comments: true
---

# Robot Dynamics & control: Lecture 4 - Inverse Kinematics

## Table of Contents
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## Introduction
- This chapter is concerned with the inverse problem of __finding the joint variables__ in terms of the __end-effector position and orientation__.
- After formulating the general I.K probelm, we study the principle of __kinematic coupling__ that simplifies the I.K. by decoupling the position and orientation.
- We describe a __geometric approach__ for solving the position problem, while we exploit the __Euler angle parameterizaiton__ to solve the orientation problem.

$$
\begin{aligned}
\begin{bmatrix} 
\theta_1 \\ 
\vdots \\
\theta_n \\
d_1 \\
\vdots \\
d_n \\
\end{bmatrix} 
=
f(x,  y , z, \psi,  \theta , \phi)
\end{aligned} 
$$

----------------

## The General I.K. Problem 

$$
\begin{aligned}
H = (x,  y , z, \psi,  \theta , \phi)
=
\begin{bmatrix} 
R & o \\
0 & 1 \\
\end{bmatrix} 
\in SE(3)
\end{aligned} 
$$

with $$R \in SO(3)$$, find (one or all) solutions of the equation

$$
\begin{aligned}
T ^0 _n (q_1, \cdots, q_n) = H
\end{aligned} 
$$

where

$$
\begin{aligned}
T ^0 _n (q_1, \cdots, q_n) = A_1(q_1) \cdots A_n(q_n).
\end{aligned} 
$$

> Our task is to find the values for the joint variables $$q_1, ..., q_n$$ so that $$ T^0_n (q_1, ..., q_n) = H$$

$$
\begin{aligned} 
T ^0 _n (q_1, \cdots, q_n) &= H \\
\begin{bmatrix} 
T_11 &  T_12 & T_13 & T_14\\
T_21 & T_22 & T_23 & T_24 \\
T_31 & T_32 & T_33 & T_34 \\
T_41 & T_42 & T_43 & T_44 \\
\end{bmatrix}
&= 
\begin{bmatrix} 
 c_{\phi}c_{\theta} & -s_{\phi}c_{\psi}+c_{\phi}s_{\theta}s_{\psi} & s_{\phi}s_{\psi} + c_{\phi}s_{\theta}c_{\psi} & x\\
s_{\phi}c_{\theta}  & c_{\phi}c_{\psi}+s_{\phi}s_{\theta}s_{\psi} & -c_{\phi}s_{\psi} + s_{\phi}s_{\theta}c_{\psi} & z\\
-s_{\theta} & c_{\theta}s_{\psi} &  c_{\theta}c_{\psi} & z \\
0 & 0 &  0 & 1 \\
\end{bmatrix}
\end{aligned} 
$$

- Given matrix: H, roll/pitch/yaw rotation w.r.t inertial frame

$$\rightarrow$$ translation w.r.t inertial frame frame $$\rightarrow$$ roll/pitch/yaw rotation w.r.t current frame.

- The above equation results in 12 nonlinear equations in n unknown variables, which can be written as

$$
\begin{aligned} 
T ^_{ij} (q_1, \cdots, q_n) &= h_{ij}, i=1,2,3, j=1,..,4 \\
\end{aligned} 
$$

where $$T_{ij}, h_{ij}$$ refer to the 12 nontrivial entries of $$T^0_n$$ and $$H$$.


















------------

> Reference:
- [SEOULTECH - HRRLAB](http://hrrlab.com)
- [SEOULTECH - Robot Dynamics & Control, Lecture slides](http://hrrlab.com/)