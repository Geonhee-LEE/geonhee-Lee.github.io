---
layout: post
title:  "robotics_and_control"
categories: robotics_and_control
tags: robotics
comments: true
---

# Robot Dynamics & control: Lecture 2 - Rigid Motions and Homogeneous Transforms

### Table of Contents
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

### Introduction
- Robot kinematocs is concerned with the establishment of various coordinate systems to represent the __positions and orientations__ of rigid objects and with __transformations__ among these coordinate frames.
- __Homogeneous transformations__ combine the operations of rotation and translation into _single matrix multiplication_ and this is used to derive _the forward kinematic equation_.
  - i.e., Expression of the transformation about relationship of position and orientation.


------

### Representing Positions
- In robotics, it is necessary to specify a coordinate frame in order to assign coordinates of a __point__.
- While a point correspond to a specific location in space, a __vector__ specifies a direction and a magnitude.

$$
\begin{aligned}
v ^0 _1 = 
\begin{bmatrix} 5  \\ 6 \end{bmatrix}

\end{aligned} 
$$

------

### Representing Rotations

- Rotation in the plane
  - __Orientation matrix__ that specifies the coordinate vectors for the axes of frame $$o_1 x_1 y_1$$ wutg respect to coordinate frame $$o_0 x_0 y_0$$.
$$
\begin{aligned}
R ^0 _1 
&= 
\begin{bmatrix} x^0_1  & y^0 _1 \end{bmatrix}
&=
\begin{bmatrix} 
cos \theta  & - sin \theta \\
sin \theta & cos \theta
\end{bmatrix}
\end{aligned} 
$$
  - Alternative approach using the __dot product__ of two unit vectors
    - Physical meaning: projected vector of $$x_1$$ onto $$x_0$$.
$$
\begin{aligned}
R ^0 _1 
&= 
\begin{bmatrix} x^0_1  & y^0 _1 \end{bmatrix}
&=
\begin{bmatrix} 
x_1 \cdot  x_0  & y_1 \cdot  x_0  \\
x_0 \cdot  y_0  & y_1 \cdot  y_0  
\end{bmatrix}
&=
{(R^1 _0)}^{-1}
\end{aligned} 
$$

> Note: the column vectors are of __unit length__ and __mutually orthogonal__.

- Rotations in 3 dimensions
  - The projection technique scales nicely to the 3 dimensional case
$$
\begin{aligned}
R ^0 _1 
&= 
\begin{bmatrix} x^0_1  & y^0 _1 & z^0_1\end{bmatrix}
&=
\begin{bmatrix} 
x_1 \cdot  x_0  & y_1 \cdot  x_0  & z_1 \cdot x_0  \\
x_1 \cdot  y_0  & y_1 \cdot  y_0  & z_1 \cdot y_0  \\
x_1 \cdot  z_0  & y_1 \cdot  z_0  & z_1 \cdot z_0  \\
\end{bmatrix}
\in SO(3)
\end{aligned} 
$$

- Ex. 2.1
  - Suppose the frame $$o_1 x_1 y_1 z_1$$  is rotated through an angle about the $$z_0$$ axis, and it is desired to find the resulting transformation matrix $$R^0_1$$.
  - Note that by convention the positive sense for the angle is given by the right hand rule;
    - That is, a positive rotation of degrees about the z-axis would advance a right-hand threaded screw along the positive z-axis.

<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/lec2/1.png" class="lead"   style="width:640px; height=:480px"/>
</figure>

$$
\begin{aligned}
& x_1 \cdot x_0  = cos \theta & y_1 \cdot x_0 = - sin \theta \\
& x_1 \cdot x_0  = cos \theta & y _1 \cdot x_0 = - sin \theta\\
& z_0 \cdot z_1 = 1
\end{aligned} 
$$

Thus, 

$$
\begin{aligned} R^0_1
&=
\begin{bmatrix} 
cos \theta  & - sin \theta  & 0  \\
sin \theta & cos \theta &  0  \\
0  & 0  & 1  \\
\end{bmatrix}
\in SO(3)
\end{aligned} 
$$

- The Basic Rotation matrices

$$
\begin{aligned} R_{z, \theta}
&=
\begin{bmatrix} 
cos \theta  & - sin \theta  & 0  \\
sin \theta & cos \theta &  0  \\
0  & 0  & 1  \\
\end{bmatrix}, 
R_{x, \theta} = 
\begin{bmatrix} 
1  & 0  & 0  \\
0 & cos \theta &  - sin \theta  \\
0  & sin \theta & cos \theta  \\
\end{bmatrix}
R_{y, \theta} = 
\begin{bmatrix} 
cos \theta  & 0 &  sin \theta   \\
0 & 1 & 0 \\
- sin \theta  & 0  & cos \theta   \\
\end{bmatrix}
\end{aligned} 
$$

$$
\begin{aligned} 
& R_{z, 0} = I \\
& R_{z, \theta}  R_{z, \phi} = R_{z, \theta+\phi}  \\
& R^{-1}_{z, \theta} = R_{z, -\theta} 
\end{aligned} 
$$

------

### Rotational Transformations

- We wish to determine the coordinates of p relative to a fixed reference frame $$o_0 x_0 y_0 z_0$$.

$$
\begin{aligned} 
& p = ux_1 + vy_1 + wz_1
\end{aligned} 
$$

- Projection onto the coordinate axes of the frame $$o_0 x_0 y_0 z_0$$.
  - Project the vector $$p$$ onto each axis of reference frame $$o$$.

$$
\begin{aligned} p^0
&=
\begin{bmatrix} 
p \cdot x_0\\
p \cdot y_0\\
p \cdot z_0  \\
\end{bmatrix}
\end{aligned} 
$$

<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/lec2/2.png" class="lead"   style="width:640px; height=:480px"/>
</figure>

-  Combining these two equations


$$
\begin{aligned} p^0
&=
\begin{bmatrix} 
(ux_1 + vy_1 + wz_1) \cdot x_0\\
(ux_1 + vy_1 + wz_1) \cdot y_0\\
(ux_1 + vy_1 + wz_1) \cdot z_0  \\
\end{bmatrix} \\
&= 
\begin{bmatrix} 
x_1 \cdot x_0 & y_1 \cdot x_0 & z_1 \cdot x_0\\
x_1 \cdot y_0 & y_1 \cdot x_0 & z_1 \cdot x_0\\
x_1 \cdot z_0 & y_1 \cdot x_0 & z_1 \cdot x_0\\
\end{bmatrix}
\begin{bmatrix} 
u\\
v\\
w\\
\end{bmatrix}
\end{aligned} 
$$

Finally, 

$$
\begin{aligned} p^0
&= R^0_1 p^1 
\end{aligned} 
$$

- The rotation matrix can be used not only to present the __orientation__ of coordinate frame $$o_1 x_1 y_1 z_1$$ with respect to frame $$o_0 x_0 y_0 z_0$$, but also to __transform the coordinates__ of a point from one frame to another.


------

### Composition of Rotations

#### Rotation with respect to the __current coordinate frame__
  
- Suppose we now add a third coordinate frame $$o_2 x_2 y_2 z_2$$ related to the frames $$o_0 x_0 y_0 z_0$$ and  $$o_1 x_1 y_1 z_1$$ by rotational transformations.

$$
\begin{aligned} 
& p^0 &= R^0_1 p^1 \\
& p^1 &= R^1_2 p^2 \\
& \therefore p^0 &= R^0_1 R^1_2 p^2 &= R^0_2 p^2
\end{aligned} 
$$

  
- Suppose initially that all 3 of the coordinate frames coincide.
- We __first rotate__ the frame $$o_1 x_1 y_1 z_1$$ related to $$o_0 x_0 y_0 z_0$$ according to the __transformation $$R ^1 _2$$__.
- Then, with the frame $$o_1 x_1 y_1 z_1$$ and $$o_2 x_2 y_2 z_2$$ coincident, we __rotate__ $$o_2 x_2 y_2 z_2$$ relative $$o_1 x_1 y_1 z_1$$ to according to the __transformation $$R ^1 _2$$__.
- In each case, rotation occurs with respect to the __current frame__.

> Note: 
> > It is important to remember that __the order__ in which a sequence of rotations are carried out __is crucial__.
> > Rotation matix has difference results according to the order of sequence of roation.

#### Rotation with respect to the __fixed coordinate frame__

- Many times it is desirable to perform __a sequence of rotations, each about a given fixed coordinate frame__, rather than about successive current frames. 
































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


#####  Spherical Configuration(RRP)
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
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/9.png" class="lead" style="width:480px; height=:240px" />
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


### Law of Cosines

$$
\begin{aligned} 
  & cos \theta_2 = \frac{x^2 + y^2 - a_1 ^2 - a^2 _2}{2 a_1 a_2} &\cong D  \\
  & \therefore  \theta _2 = cos ^{-1}(D)  
\end{aligned} 
$$

-  Not better way.
-  This can not distinguish the elbow up and down.

$$
\begin{aligned} 
  & sin ^2 \theta  _2 + cos ^2 \theta_2 =  1 \rightarrow sin \theta _2 = \pm  \sqrt{1 - D^2} \\
  &  \therefore  \theta _2 = tan ^{-1} (\frac{1 - D^2}{D})
\end{aligned} 
$$

- __Signs determine__ the elbow up and down.

### Inverse kinematic equations.

<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/12.png" class="lead"   style="width:240px; height=:180px"/>
</figure>

$$
\begin{aligned} 
  & \theta  _2 = tan ^{-1} \frac{\pm \sqrt{1 - D ^2}}{D} \\
  & \theta  _1 = tan ^{-1} (\frac{y}{x}) - tan ^{-1}(\frac{a_2 sin \theta _2}{a_1 + a_2 cos \theta_2}) 
\end{aligned} 
$$

- For verification, we can check using the Forward Kinematics(__cross-check__)

### Another way - Closed form

- __Closed form__: $$ \theta_1, \theta_2 $$ is expressed with x, y using forward kinematics.

  - x, y를 제곱하여 $$cos \theta_2, sin \theta_2$$을 얻은 후, $$\theta_2$$를 구한다.

$$
\begin{aligned} 
  & x = a_1 cos \theta_1 + a_2 cos(\theta _1 + \theta _2)  \\
  & y = a_1 sin \theta_1 + a_2 sin(\theta _1 + \theta _2)  \\
  & \qquad \qquad \qquad \triangledown  \\
  \\
  x^2 
    &= a^2 _1 cos ^2 \theta_1 + a^2_2   cos^2 (\theta_1 + \theta_2 ) + 2 a_1 a_2 cos \theta_1 cos(\theta_1 + \theta_2)\\
    & = a^2 _1 cos ^2 \theta_1 + a^2_2   cos^2 (\theta_1 + \theta_2 ) +  a_1 a_2 (cos (2 \theta_1 + \theta_2) + cos\theta_2)\\
  y^2 &= a^2 _1 sin ^2 \theta_1 + a^2_2   sin^2 (\theta_1 + \theta_2 ) + 2 a_1 a_2 sin \theta_1 sin(\theta_1 + \theta_2)\\
    & = a^2 _1 sin ^2 \theta_1 + a^2_2   sin^2 (\theta_1 + \theta_2 ) -  a_1 a_2 (cos (2 \theta_1 + \theta_2) - cos\theta_2)\\
    & \qquad \qquad \qquad   \triangledown  \\
  \\
  & x^2 +y^2  = a^2_1 +a^2 _2 + 2a_1 a_2 cos \theta_2 \\
  & \qquad \qquad \qquad  \triangledown  \\
  \\
  & cos \theta_2 = \frac{x^2 + y^2 - a_1 ^2 - a^2 _2}{2 a_1 a_2} \quad \cong D \\
  & sin \theta_2 = \pm \sqrt{1 - D^2} \\
  \\
  & \therefore \theta_2 = tan ^{-1} (\frac{\pm \sqrt{1-D^2}}{D})
\end{aligned} 
$$

  - 아래의 식(1)을 얻기 위해 $$cos \theta_1, sin\theta_2$$를 각각 곱하고, 아래의 식(2)을 얻기 위해 $$sin \theta_1, cos \theta_1$$를 각각 곱한다.
  - 이 후 x, y를 곱하여 더하rh 빼서 $$cos \theta_1,sin \theta_1$$를 얻어 $$\theta_1$$ 을 구한다.

$$
\begin{aligned} 
  & x = a_1 cos \theta_1 + a_2 cos(\theta _1 + \theta _2)  \\
  & y = a_1 sin \theta_1 + a_2 sin(\theta _1 + \theta _2)  \\
  \\
  & x cos \theta_1  = a_1 cos ^2 \theta_2 + a_2 cos \theta_1 (cos \theta_1 cos\theta_2 - sin \theta_1 sin \theta_2) \\
  & y sin \theta_1  = a_1 sin ^2 \theta_2 + a_2 sin \theta_1 (sin \theta_1 cos\theta_2 - cos \theta_1 sin \theta_2) \\
  & \qquad \qquad \qquad \triangledown  \\
  & x cos \theta_1 + y sin \theta_1 = a_1 + cos \theta_2 \qquad(1)\\
  \\
  \\
  & x sin \theta_1  = a_1 sin \theta_1 cos  \theta_1 + a_2 sin \theta_1 (cos \theta_1 cos\theta_2 - sin \theta_1 sin \theta_2) \\
  & y cos \theta_1  = a_1 sin \theta_1 cos \theta_1 + a_2 cos \theta_1 (sin \theta_1 cos\theta_2 - cos \theta_1 sin \theta_2) \\
  & \qquad \qquad \qquad \triangledown  \\
  & x sin \theta_1 - y cos \theta_1 = -a_2 sin \theta_2 \qquad(2)\\
  \\
  \\
  & (x^2 + y^2 )cos \theta_1 = x(a_1 + a_2 cos \theta_2) +y a_2 sin \theta_2  \qquad \because x \cdot (1) - y \cdot (2)  \\
  & \qquad \qquad \qquad \triangledown  \\
  & cos \theta_1 = \frac{x(a_1 + a_2 cos \theta_2) +y a_2 sin \theta_2 }{(x^2 + y^2 )}  \\
  \\
  \\
  & (x^2 + y^2 )sin \theta_1 = y(a_1 + a_2 cos \theta_2) -x  a_2 sin \theta_2  \qquad \because y \cdot (1) + x \cdot (2)  \\
  & \qquad \qquad \qquad \triangledown  \\
  & sin \theta_1 = \frac{y(a_1 + a_2 cos \theta_2) -x a_2 sin \theta_2 }{(x^2 + y^2 )}  \\
  \\
  & \therefore \theta_1 = tan ^{-1} (\frac{y(a_1 + a_2 cos \theta_2) - x a_2 sin \theta_2}{x(a_1 + a_2 cos \theta_2) + y a_2 sin \theta_2})

\end{aligned} 
$$

### Another way - Numerical Solution
-  In contrast to the closed form(geometry solution), it absolutely __needs a forward kinematics__.


<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/13.png" class="lead"   style="width:480px; height=:640px"/>
</figure>



-------

## Problem 3: Velocity Kinematics

- In order to __follow a contour__ at constant velocity, or at any prescribed velocity, we must know the __relationship between the velocity of the tool(end-effector) and the joint velocities__.
  - we can __differentiate the equations__ to obtain
  
$$
\begin{aligned} 
  & \dot{x} = - a_1 \dot{\theta_1} sin \theta_1 - a_2 (\dot{\theta_1} + \dot{\theta_2}) sin(\theta _1 + \theta _2)  \\
  & \dot{y} = a_1 \dot{\theta_1} cos \theta_1 + a_2 (\dot{\theta_1} + \dot{\theta_2}) cos(\theta _1 + \theta _2)  \\
\end{aligned} 
$$

If $$ x = \begin{bmatrix} x  \\ y \end{bmatrix} $$ and $$ \theta = \begin{bmatrix} \theta_1  \\ \theta_2 \end{bmatrix} $$,

$$
\begin{aligned}  \dot{x} 
& = J \dot{\theta} \\
& =
\begin{bmatrix} 
  \frac{\partial x}{\partial \theta_1} &  \frac{\partial x}{\partial \theta_2} \\
  \frac{\partial y}{\partial \theta_1} & \frac{\partial y}{\partial \theta_2}  \\
\end{bmatrix} 
\begin{bmatrix} \theta_1  \\ \theta_2 \end{bmatrix} \\
& =
\begin{bmatrix} 
  -a_1 sin \theta_1 - a_2 sin (\theta_1 + \theta_2) &  - a_2 sin(\theta_1 + \theta_2) \\
  a_1 cos\theta_1 + a_2 cos (\theta_1 + \theta_2) & a_2 cos(\theta_1 + \theta_2)  \\
\end{bmatrix} 
\begin{bmatrix} \theta_1  \\ \theta_2 \end{bmatrix} \\
\end{aligned} 
$$

- where, J is Jacobian.


- Using inverse Jacobian give

$$
\begin{aligned} 
\dot{\theta} &= J ^{-1} \dot{x} \\

\begin{bmatrix} \dot{\theta_1}  \\ \dot{\theta_2} \end{bmatrix}
&=
\frac{1}{a_1 a_2 sin \theta_2}
\begin{bmatrix} 
  a_2 cos (\theta_1 + \theta_2) &   a_2 sin(\theta_1 + \theta_2) \\
  -a_1 cos\theta_1 - a_2 cos (\theta_1 + \theta_2) & -a_1 sin \theta_1 - a_2 sin(\theta_1 + \theta_2)  \\
\end{bmatrix} 
\begin{bmatrix} \dot{x}  \\ \dot{y} \end{bmatrix} \\
\end{aligned} 
$$

- __Singular Configuration__:
  - Where there is __no inverse Jacobian__.
  - At singular configuration, the manipulator cannot move in certain dirctions.

$$ 
\begin{aligned} 
   Det \quad J = a_1 a_2 sin \theta_2 = 0 \\
  \therefore \theta_2 = 0 \quad or \quad \pi
\end{aligned} 
$$

-------

## Problem 4: Path Planning and Trajectory Generation

### Path planning
- Determines a path in task space to mode  the robot to a goal position while avoiding collision with objects in its workspace, __without time considerations,__, that is, without considering velocities and accelerations.

### Trajectory Generation:
- Determine the __time history__ of the manipulator along a given path.

-------

## Problem 5: Dynamics

- Relationship between __motion and forces(Equation of motion)__.
- How much force is required to achieve the given motion?
  - __Rigid body dynamics__: Dynamics of target object which has __no strain or deformation__ in the body.
  
$$ 
\begin{aligned} 
\qquad \qquad \qquad  M(q) \ddot{q} + C(q, \dot{q}) \dot{q} + G(q) = \tau 
\end{aligned} 
$$

- $$M$$: Inertia matrix.
- $$C$$: Centrigufal and Coriolis matrix.
- $$G$$: Gravity matrix.
- $$q$$: Generalized coordinate(angle or position)
- $$\tau$$ : Generalized force(torque or force)


- __Inverse dynamics__: Computes the __required joint torques or forces__ that lead to the given robot motion.
- __Forward dynamics__: Computes the robot motion __from the joint torques or forces applied__.

### Example 1: Three link-revolute arm

<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/14.png" class="lead"   style="width:480px; height=:320px"/>
</figure>
<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/15.png" class="lead"   style="width:480px; height=:320px"/>
</figure>

-------

## Problem 6: Position Control

- The control prolem for robot manipulator is the problem of determining the time history of __joint inputs (joint forces or torques or inputs to the actuators, for example, voltage)__ required to cause the end-effector to execute a desired motion.
- There are many control techniques and methodologies.
  - An important thing is that control methods depends on __hardware/software and application__
    - Cartesian manipulator vs. Elbow type manupulator
    - DC motor with reduction gear vs. High torgue DC motor without gwar (High tech. for interaction)
    - Point-to-point path vs. Continous path
    - More complicated hardward, the more advanced control methods.


### Example: Independent Joint Position Control

- Features:
  - Simplest type of control strategy.
  - Each axis is controlled as a SISO(Single Input/Single Outpue) system.
  - Any coupling effects due to the motion of the other links is either ignored or treated as a disturbance.
  - Objectives: tracking and disturbance rejection.
    - Pose에 따라 moment가 달라져 disturbance가 달라지지만 높은 기어비 때문에 무시.
  
<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/16.png" class="lead"   style="width:180px; height=:120px"/>
</figure>
<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/17.png" class="lead"   style="width:480px; height=:240px"/>
</figure>
<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/18.png" class="lead"   style="width:640px; height=:240px"/>
</figure>

- Each joint has to follow the desired joint angle accurately!

-------

## Problem 7: Force Control

-  Why Force Control?:
   -  Pure position control is not adequate for task which involve extensive contact with the environment.
      -  e.g., assembly, grinding, deburring
   - Need to control the force as well (slight deviation of the end effector would caues either to loose contact or to press too strongly).
     - We can use the Hybrid control(Position + Force control)
   - A force control strategy is one that modifies position trajectories based on the sensed forses.

<figure>
  <img alt="An image with a caption" src="/assets/img/Robot_dynamics/19.png" class="lead"   style="width:640px; height=:480px"/>
</figure>

>  __The end-effector forces are related to the joint torques!!__


------------

> Reference:
- [SEOULTECH - HRRLAB](http://hrrlab.com)
- [SEOULTECH - Robot Dynamics & Control, Lecture slides](http://hrrlab.com/)