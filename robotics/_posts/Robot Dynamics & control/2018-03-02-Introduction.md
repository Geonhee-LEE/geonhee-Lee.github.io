---
layout: post
title:  "robotics_and_control"
categories: robotics_and_control
tags: robotics
comments: true
---

# Robot Dynamics & control: Lecture 1 - Introduction

### Table of Contents
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

### Components & Structure of Robots:
- Robot manipulators are composed of links connected by __joints__ into a __kinematic chain__.
  -   __Revolute__ joint: allows relative rotation between two links.
  -   __Prismatic__ joint: allows a linear relative motion between two links.


<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/1.png" class="lead" data-width="240" data-height="180" />
</figure>


#### Degrees of Freedom
  - The number of joints determines the DOF of the manipulator.
  - A typical manipulator should possess at least six independent DOF:
    - Three for positioning + Three for orientation.
  - With fewer than six DOF the arm cannot reach every point in its work encironment with arbitray orientation.


#### Workspace
- The total volume swept out by the end-effector as the manipulator executes all possible motions.
- __Reachable workspace__ : entire set of points reachable by the manipulator
- __Dextrous workspace__: subset of the reachable workspace that the manipulator can reach with an arbitrary orientation of the end-effector. 

#### Common Kinematic Arrangements

#####  Articulated configuration(RRR)
- This configuration provides for relatively large freedom of movement in a compact space
- The links and joints are analogous to human joint.

<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/2.png" class="lead" data-width="180" data-height="120" />
</figure>


#####  Spherical Configuration
- The third joint of the articulated robot is replaced by a prismatic joint.
- The 'Stanford-arm' is an example of a spherical manupulator.

<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/3.png" class="lead" data-width="240" data-height="180" />
</figure>

##### SCARA Configuration(RRP)
- The so-called SCARA(Selective Compliant Articulated Robot for Assembly) has $$z_0, z_1, z_2$$ prallel.
- Ideal for table top assembly such as pick and place task. 

<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/4.png" class="lead" data-width="240" data-height="180" />
</figure>

##### Cylindrical Configuration(RPP)
- The first joint is revolute, while the second and third joints are prismatic.
- This is often used in materials transfer task.

<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/5.png" class="lead" data-width="240" data-height="180" />
</figure>

##### Cartesian Configuration(PPP)
- The configuration is useful for table-top assembly applications.
- This is often used in pick and place operations.

<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/6.png" class="lead" data-width="240" data-height="180" />
</figure>

##### Parallel manupulator
- The configuration forms a closed chain by using several independent kinematic chains connecting the vase to the end-effector.
- The closed chain results in greater structural rigidity.
- This robot generally have __much structural rigidity__ than serial link robots.

<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/8.png" class="lead" style="width:240px; height=:180px" />
</figure>

#### Wrists and End-Effectors 
- The wrist of a manipulator refers to the joints in the kinematic chain berween the arm and hand.
- It is increasingly common to design manipulators with __spherical wrists__, by which we mean wrists whose three joint axes intersect at common point.
- 6 DOF = __3 DOF for wrist + 3DOF for arm__
- The arm and wrist assemblies are used primarily __for positioning the end-effector__(=hand)
- The simplest type of end-effector are grippers.

<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/9.png" class="lead" style="width:480; height=:240" />
</figure>


-------

## Problem 1: Forward Kinematics

- The first problem is to describe __position and orientation of the tool__.
- Determination of the position and orientation of the end-effector(or tool) in terms of joint variables(angle or displacement).

<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/10.png" class="lead"   style="width:240px; height=:180px"/>
</figure>


### Forward kinematic equations 

####  Tool(End-effector) position 
$$
\begin{aligned}
\qquad   x = a_1 cos \theta_1 + a_2 cos( \theta_1 + \theta_2) \\
\qquad   y = a_1 sin \theta_1 + a_2 sin( \theta_1 + \theta_2)
\end{aligned} 
 $$
####  Tool(End-effector) Orientation
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



-------


## Problem 2: Inverse Kinematics

- In order to command the robot to move to arbitrary position, we need the joint variables __in terms of x and y coordinates__.
  - we may need the forward kinematic equations in advance.

- Since the forward kinematic equations are nonlinear, a solution may not be easy.
- There may be __many solutions or infinite number of solution__.
  - we can impose constraints.

<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/11.png" class="lead"   style="width:240px; height=:180px"/>
</figure>


### Law of Cosines:

$$
\begin{aligned} 

cos \theta_2 
&= 
\frac{x^2 + y^2 - a_1 ^2 - a^2 _2}{2 a_1 a_2}
&\cong 
D \\ 
\therefore  \theta _2 = cos ^{-1}(D)  \rightarrow 
\iffalse 
Not better way.
 This can not distinguish the elbow up and down
\fi

\end{aligned} 
$$


> Reference:
- [SEOULTECH - HRRLAB](http://hrrlab.com)
- [SEOULTECH - Robot Dynamics & Control, Lecture slides](http://hrrlab.com/)