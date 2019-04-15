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
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/lec4/1.png" class="lead"   style="width:240px; height=:480px"/>
</figure>

- To find the corresponding variables $$\theta_1, ..., \theta_6$$, we must solve the following simultaneous set of nonlinear trigonometric eq.

<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/lec4/2.png" class="lead"   style="width:480px; height=:560px"/>
</figure>

> Note
> > It is too difficult to solve directly in closed form. 
> >
> > Therefore, it is necessary to develop efficient and systematic techniques. 
> >
> > The inverse kinematics problem may or may not have a solution.
> >
> > Even if a solution exist, it may or may not be unique.


- Inverse kinematics solution:
  
1. __A closed form solution__
2. __A numerical solution__

- A closed form solution means following explicit relationship

$$
\begin{aligned} 
q_k = f_k (h_{11}, ..., h_{34}), k=1,...,n
\end{aligned} 
$$

- Advantages of closed form solution.

1.  Closed form solution is more __accurate and faster__ than numerical solution.
2.  Closed form solution allows one to develop rules for __choosing a particular solution__ among several multiple solutions.
    - It means that you can get the desired pose by selecting the sign, numerical method is impossible.


## Kinematic Decoupling

- For manipulators having 6 joints with the last three joint intersecting at a point, it is possible to decouple the I.K. problem into two simper problems;
  - __Inverse position kinematics__
  - __Inverse orientation kinematics__
- Let us suppose that there are exactly 6 degrees-of-freedom and that the last 3 joint axes intersect at a point $$o_c$$.

$$
\begin{aligned} 
R^0_6 (q_1, ..., q_6) =R \\
o^0_6 (q_1,..., q_6) = o
\end{aligned} 
$$

> $$o$$ and $$R$$ are __desired position and orientation of the tool frame__ with respect to the world coordinate system.

- Assumption: axes $$z_3, z_4$$, and $$z_5$$ intersect at $$o_c$$ and hence the origins $$o_3, o_4$$ and $$o_5$$ will be at the wrist center $$o_c$$.
  - Motion of __final 3 links__ about these axes __will not change the position of $$o_c$$__
  - Thus, the __position of the wrist__ center is thus __function of only the first 3 joint variables__ ($$\thata_1, \theta_2$$ and $$d_3$$).

<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/lec4/3.png" class="lead"   style="width:240px; height=:480px"/>
</figure>

- The origin of the tool frame, $$o$$:

$$
\begin{aligned} 
o &= o^0_c + R^0_5 
\begin{bmatrix} 
0\\
0 \\
d_6\\
\end{bmatrix} \rightarrow
o^0_c &= o - R^0_5 
\begin{bmatrix} 
0\\
0 \\
d_6\\
\end{bmatrix} \rightarrow
o^0_c &= o - R 
\begin{bmatrix} 
0\\
0 \\
d_6\\
\end{bmatrix} 
\end{aligned} 
$$

> Third columns of $$R^0_6$$ and $$R^0_5$$ are the same!

$$
\begin{aligned} 
\begin{bmatrix} 
x_c\\
y_c\\
z_c\\
\end{bmatrix} = 
\begin{bmatrix} 
o_x - d_6 r_{13}\\
o_y - d_6 r_{23} \\
o_z - d_6 r_{33} \\
\end{bmatrix}
\quad
where \quad R = 
\begin{bmatrix} 
 r_{11} &  r_{12} &  r_{13}\\
 r_{21} &  r_{22} &  r_{23}\\
 r_{31} &  r_{32} &  r_{33}\\
\end{bmatrix}
\end{aligned} 
$$

> By using the above eq., we may __find the values of the first 3 joint variables__.
> 
> This determines the __orientation transformation $$R^0_3$$__ which depends only on these first 3 joint variables.

- Orientation of the end-effector relative to the frame $$o_3 x_3 y_3 z_3$$

$$
\begin{aligned} 
& R = R^0_6 = R^0_3 R^3_6 \\ 
\\
& R^3_6 = (R^0_3)^{-1} R = (R^0_3)^T R 
\end{aligned} 
$$

- $$R$$: given
- $$R^0_3$$: can be calculated once the first 3 joint variables are known.
- $$R^3_6$$: unknown

> Then, the final 3 joint variables can be found as a set of Euler angles corresponding to $$R^3_6$$.


- Summary
  - For this class of manipulators the determination of the I.K. can be summarized by the following algorithm.

__Step 1__: Find $$q_1, q_2, q_3$$ such that the wrist center $$o_c$$ has coordinates given by

$$
\begin{aligned} 
o^0_c &= o - R^0_5 
\begin{bmatrix} 
0\\
0 \\
d_6\\
\end{bmatrix} = o - R 
\begin{bmatrix} 
0\\
0 \\
d_6\\
\end{bmatrix} 
\end{aligned} 
$$

__Step 2__:  Using the joint variables determined in Step 1, evaluate $$R^0_3$$.

__Step 3__: Find a set of Euler angles corresponding to the rotation matrix

$$
\begin{aligned} 
& R^3_6 = (R^0_3)^{-1} R = (R^0_3)^T R .
\end{aligned} 
$$

-----------

## Inverse Position: A Geometric Approach

- We can use a geometic approach to find the variables $$q_1, q_2, q_3$$ corresponding to $$o^0_c$$.
- Two reasons for the __geometric approach__:
  - Most manipulator designs are __kinematically simple__ (without joint offset), usually consisting of 1 of the 5 basic configurations with a spherical wrist.
  - There are few techniques that can handle the general I.K. problem for arbitrary configurations.
- The general idea of the geometric approach is to solve for joint variable $$q_i$$ __by projecting the manipulator onto the $$x_{i-1} - y_{i-1}$$ plane__ and solving a simple trigonometry problem.

### Articulated Configuration

Consider the elbow manipulator shown in Fig. 4.2, with the components of denoted $$o^0_c$$ by $$x_c, y_c, z_c$$.
We project $$o_c$$ onto the $$x_0 - y_0$$ plane.

<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/lec4/4.png" class="lead"   style="width:320px; height=:240px"/>
</figure>

From this projection

$$
\begin{aligned} 
\theta_1 = Atan2 (y_c, x_c)
\end{aligned} 
$$

Note that a second valid solution for $$\theta_1$$ is 

$$
\begin{aligned} 
\theta_1 = \pi + Atan2 (y_c, x_c)
\end{aligned} 
$$

This second solution leads to different solutions for $$\theta_2$$ and $$\theta_3$$.

The above solution is valid unless $$x_c = y_c = 0$$. In this case, the manipulator is in a singular configuration shown in Fig. 4.5, and there are thus __infinitely many solutions__ for $$\theta_1$$.

<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/lec4/5.png" class="lead"   style="width:320px; height=:240px"/>
</figure>


If there is an offset d, then the wrist center cannot intersect $$z_0$$. 
In this case, there will be only 2 solution for $$\theta_1 \rightarrow$$ __These correspond to the so-called left arm and right arm configuration__.

<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/lec4/6.png" class="lead"   style="width:320px; height=:240px"/>
</figure>

####  Case 1) Left arm configuration:

<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/lec4/7.png" class="lead"   style="width:320px; height=:240px"/>
</figure>

Geometrically, 

$$
\begin{aligned} 
& \theta_1 = \phi - \alpha \\
& where, \phi = Atan2(y_c, x_c), \alpha = Atan2(d, \sqrt{r^2 - d^2}) = Atan2(d, \sqrt{x_c^2 + y^2_c -d^2}) 
\end{aligned} 
$$

####  Case 2) Right arm configuration:

<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/lec4/8.png" class="lead"   style="width:320px; height=:240px"/>
</figure>

Geometrically, 

$$
\begin{aligned} 
& \theta_1 = \alpha + \beta \\
& where, \alpha = Atan2(y_c, x_c), \beta = \gamma + \pi = Atan2(d, \sqrt{r^2 - d^2}) + \pi = Atan2(- d, - \sqrt{r^2 +  -d^2}) 
\end{aligned} 
$$


------------

> Reference:
- [SEOULTECH - HRRLAB](http://hrrlab.com)
- [SEOULTECH - Robot Dynamics & Control, Lecture slides](http://hrrlab.com/)