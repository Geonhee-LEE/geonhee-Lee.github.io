---
layout: post
title:  "robotics_and_control"
categories: robotics_and_control
tags: robotics
comments: true
---

# Robot Dynamics & control: Lecture 3 - Forward Kinematics: The Denavit-Hartenberg Convention

### Table of Contents
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

### Introduction
- The forward kinematics problem is to determine the __position and orientation__ of the end-effector, given the values for the joint variables of the robot.
- The joint variables: angle between the links for revolute or rotational joint link extension for the prismatic or sliding joint.

$$
\begin{aligned}
\begin{bmatrix} 
x \\ 
y \\
z \\
\psi \\
\theta \\
\phi \\
\end{bmatrix} 
=
f(\theta_1,  \cdots , \theta_n, d_1 ,  \cdots , d_n)
\end{aligned} 
$$

- $$f$$: forward kinematics


### Kinematic Chains

- A robot manupulator __n__ joints will have __n+1__ links including ground.
- Joint number: __1 ~ n__, Link number: __0 ~ n__
- Joint i connects link i-1 to link i.
- Joint i is fixed at link i-1, so when joint i is actuated, link i moves.
- Link 0(the first link) is fixed and does not move when the joints are actuated.

<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/lec3/1.png" class="lead"   style="width:480px; height=:360px"/>
</figure>

- With the i^{th} joint, we associate a joint variable, denoted by q_i.
  - $$\theta_i$$: joint $$i$$ revolute
  - d_i: joint $$i$$ prismatic
- __$$o_i x_i y_i z_i$$ is attached to link i__ $$\rightarrow$$ when joint i is actuated, link i and its attached frame $$o_i x_i y_i z_i$$ experience a resulting motion.
- The frame $$o_0 x_0 y_0 z_0$$ which is attached to the robot base, is referred to as the __inertial frame__(=world coordinate).

<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/lec3/2.png" class="lead"   style="width:480px; height=:360px"/>
</figure>


- Suppose $$A_i$$ is the __homogeneous transformation matrix__ that expresses the position and orientation of $$o_i x_i y_i z_i$$ with respect to $$o_{i-1} x_{i-1} y_{i-1} z_{i-1}$$.
- The matrix $$A_i$$ is not constant but __varies__ as the configuration of the robot is changed, and $$A_i$$ is a function of only a single joint variable, namely $$q_i$$.

$$
\begin{aligned}
A_i = A_i (q_i)
\end{aligned} 
$$

- Homogeneous transformation matrix $$T^i_j$$ that expresses the __position and orientation__ of $$o_j x_j y_j z_j $$ with respect to $$o_i x_i y_i z_i$$ .

$$
\begin{aligned}
T^i_j = A_{i+1} A_{i+2} \cdot A_{j-1} A_{j} \qquad if \quad i < j \\
T^i_j = I \qquad  if \quad i = j \\
T^i_j = (T^j_i)^{-1} \qquad  if \quad i > j
\end{aligned} 
$$

- The homogeneous transformation matrix denoted by the position($$$o^0_n$) and orientation ($$R^0_n$$) of the __end-effector with respect to the inertial or base frame__:
  - This is __forward kinematics__!
  - But, it is possible to achieve a considerable amount of stream linking and simplication by introducing D-H representation.

$$
\begin{aligned} H &= 
\begin{bmatrix} 
R^0_n & o^0_n\\  
0 & 1 \\
\end{bmatrix} \\
&= T^0_n \\
&= A_1(q_i) \cdot A_n(q_n)
\end{aligned} 
$$



------------

> Reference:
- [SEOULTECH - HRRLAB](http://hrrlab.com)
- [SEOULTECH - Robot Dynamics & Control, Lecture slides](http://hrrlab.com/)