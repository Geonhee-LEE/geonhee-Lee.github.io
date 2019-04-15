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
T_{11} &  T_{12} & T_{13} & T_{14}\\
T_{21} & T_{22} & T_{23} & T_{24} \\
T_{31} & T_{32} & T_{33} & T_{34} \\
T_{41} & T_{42} & T_{43} & T_{44} \\
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
T_{ij} (q_1, \cdots, q_n) = h_{ij}, i=1,2,3, j=1,..,4 
\end{aligned} 
$$

where $$T_{ij}, h_{ij}$$ refer to the 12 nontrivial entries of $$T^0_n$$ and $$H$$.

### Example

- Recall the Stanford manipualtor.
- Suppose that the desired position and orientation of the final frame are given by


<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/lec4/1.png" class="lead"   style="width:320px; height=:240px"/>
</figure>

- To find the corresponding variables $$\theta_1, ..., \theta_6$$, we must solve the following simultaneous set of nonlinear trigonometric eq.

<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/lec4/2.png" class="lead"   style="width:320px; height=:480px"/>
</figure>

> Note
> > It is too difficult to solve directly in closed form. 
> >
> > Therefore, it is necessary to develop efficient and systematic techniques. 












------------

> Reference:
- [SEOULTECH - HRRLAB](http://hrrlab.com)
- [SEOULTECH - Robot Dynamics & Control, Lecture slides](http://hrrlab.com/)