---
layout: post
title:  "Introduction to Robotics - Dynamics"
date:   2025-01-22 11:05:25
categories: mediator feature
tags: featured
image: /assets/article_images/2025-01-22-Introduction-to-robotics/boston-dynamics-spot.jpg
mathjax: true
---
> Referenced from [Introduction to Robotics @ Princeton](https://irom-lab.princeton.edu/intro-to-robotics/)

# Dynamics of Quadrotor
<br>
In this page we will be learning about techniques for making a quadrotor hover. We will first start off with easy 2D example, moving toward to 3D examples. Now, let's get started!

## Quadrotor constrained to move along y-axis
![Simple y-axis contstrained Quadrotor](/assets/article_images/2025-01-22-Introduction-to-robotics/image.jpg)

Let's say we have a drone like above. It has one Degree of Freedom(DoF) Drone because it can only move to one direction, y direction (up and down). 

> DoF: minimum number of coordinate we can specify the configuration of the system. 

We can define the equation of motion(EoM) of the system through Newton's Second Law:

$ m\ddot{y} = F_1 + F_2 - mg $

where $F_1$ and $F_2$ are forces from the propeller. Then, we use symbols $ \triangleq or \doteq $ to define equations simpler. 

So, Define: $ u_1 \triangleq F_1 + F_2 $. 

Thus, we can conclude the EoM as below:

$ \ddot{y} = \frac{u_1}{m} - g \cdots (1)$

## Quadrotor constrained to x and y-axis
![2 Dim 3DoF Drone](/assets/article_images/2025-01-22-Introduction-to-robotics/image copy.jpg)

Since the drone now can go in two dimensions, three parameters $x, y, \theta$ are used for EoM. 

\\[ \ddot{x} = -\frac{u_1}{m} \cdot \sin{\theta} \\]
\\[ \ddot{y} = \frac{u_1}{m} \cdot \cos{\theta} - g \\]
\\[ \ddot{\theta} = \frac{(F_2 - F_1) \cdot L}{I} \\]

*$I$ : Moment of Inertia (Geometry & mass distribution)

In this equation we can define: $ u_2 \triangleq (F_2 - F_1) \cdot L$. Which is net moment/torque from propellers. 
Thus we can conclude:

\\[ \ddot{x} = -\frac{u_1}{m} \cdot \sin{\theta} \cdots (2a)\\]
\\[ \ddot{y} = \frac{u_1}{m} \cdot \cos{\theta} - g \cdots (2b)\\]
\\[ \ddot{\theta} = \frac{u_2}{I} \cdots (2c)\\]

Now that we have 2nd order equations, we now can find first order diff by defining:
$v_x \triangleq \dot{x}, v_y \triangleq \dot{y}, \omega \triangleq \dot{\theta}$

Thus, if we change this into the **First Ordinary Differential Equation** we get:
\\[ \dot{x} = v_x \\]
\\[ \dot{y} = v_y \\]
\\[ \theta = \omega \\]
\\[ \dot{v_x} = -\frac{u_1}{m} \cdot \sin{\theta} \\]
\\[ \dot{v_y} = \frac{u_1}{m} \cdot \cos{\theta} - g \\]
\\[ \dot{\omega} = \frac{u_2}{I} \\]

Define State:
\\[ \bar{x} = [x, y, \theta, v_x, v_y, \omega]^T\\]
\\[ \bar{u} = [u_q, u_w]^T \\]

$$ \dot{\bar{x}} = \frac{d}{dx} \begin{bmatrix} x \\ y \\ \theta \\ v_x \\ v_y \\ \omega \end{bmatrix} = \begin{bmatrix} v_x \\ v_y \\ \omega \\ -\frac{u_1}{m}\sin{\theta} \\ \frac{u_1}{m}\cos{\theta} -g \\ \frac{u_2}{I} \end{bmatrix}$$

Finally we can write as **General Form**:
\\[ \dot{\bar{x}} = f(\bar{x}, \bar{v}) \\]

## Motor Model
>Direct Contorl: Propeller 

According to the Newton Third Law:
\\[ F_{AB} = -F_{BA} \\]
Thus we can write Thrust equation as:
\\[ F = k_f \cdot (rotation speed of prop)^2 \\]
*$k_f$ : constant of **thrust coefficient**

`Thrust Coefficient` is dependent on the geometry of propeller

We can attain $k_f$ empirically by measuring slope from **thrust** and the **propeller speed**. 

3 Physical Parameters are: mass (m), Inertia(I), $k_f$

## 3D Quadrotor
<br>
3D Quadrotor has 6 DoF: Position (x, y, z) and orientation (Euler Angles)
> Euler Angles are angles that are attached to the drone

![Axes for quadrotor; source: Bitcraze.io](/assets/article_images/2025-01-22-Introduction-to-robotics/image copy 2.jpg)

Here $\bar{b}_x$ : forward, $\bar{b}_y$ : left, $\bar{b}_x$ : up. 

This is the way we get roll($\phi$), pitch($\theta$), yaw($\psi$)
1. Align with I
2. Rotate B about $\bar{e}_x$ by $\phi$
3. Rotate B about $\bar{e}_y$ by $\theta$
4. Rotate B about $\bar{e}_z$ by $\psi$

Also, there are 12 convention of Euler Angles:
1. Space 1-2-3 convention 
2. Space 3-2-3 convention
3. Body 3-2-3 convention

> Here `Space` is calculated by fixed axes, and `Body` is by relative axes. This means for Body 3-2-3 convention you first move z axis - move y axis - move z axis again, each moving from the axis in which it has been moved. 

**It is important to choose which convention to use in each project**

#### States for 3D Quadrotor
<br>
$\dot{\bar{x}} = f(\bar{x}, \bar{u})$
*where $\bar{x}$ is state and $\bar{u}$ is control input

State:

$$ \bar{x} = \begin{bmatrix} x \\ y \\ z \\ \phi \\ \theta \\ \psi \\ \dot{x} \\ \dot{y} \\ \dot{z} \\ p \\ q \\ r \end{bmatrix} $$

Control Input:
We can use: $F_1, F_2, F_3, F_4$
But we normally use this for convenience:

$$ \bar{u} = \begin{cases} 
    F_{tot} & \text{: Total thrust (produced by 4 propeller)} 
    \\ M_1 & \text{: Moment about }b_x
    \\ M_2 & \text{: Moment about }b_y
    \\ M_3 & \text{: Moment about }b_z
    \end{cases}
$$

