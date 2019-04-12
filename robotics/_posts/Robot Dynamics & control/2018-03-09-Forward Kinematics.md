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









------------

> Reference:
- [SEOULTECH - HRRLAB](http://hrrlab.com)
- [SEOULTECH - Robot Dynamics & Control, Lecture slides](http://hrrlab.com/)