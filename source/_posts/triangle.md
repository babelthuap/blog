---
title: Is That Point Inside My Triangle?
date: 2016-02-11 09:33:01
tags:
  - geometry
  - javascript
  - vectors
---

Today I'll start with a deceptively simple problem: *Given a triangle $\triangle ABC$ and a point $P$, determine whether $P$ lies within $\triangle ABC$.* This seems like a very fundamental problem -- and it is! Understand this, and you're well on your way to understanding real-time computer graphics. More on that later.

## Setting up the problem

Let's talk in math first. Later, we can figure out how to implement an algorithm using JavaScript. We first need to make sure we know exactly what the problem is asking. Let's assume that the points $A$, $B$, $C$, and $P$ all lie in the same plane, $\mathbb{R}^2$. We'll give them $x, y$ coordinates as follows:

{% math %}
\begin{aligned}
A &= (a_x, a_y) \\
B &= (b_x, b_y) \qquad \text{and} \qquad P = (p_x, p_y) \\
C &= (c_x, c_y)
\end{aligned}
{% endmath %}

There are two questions. First, are $A$, $B$, and $C$ collinear? If they are, then $P$ cannot possibly lie *inside* the "triangle" described. (Checking this first might save some computation later on.) With that out of the way, we'll get straight to determining whether $P$ is inside $\triangle ABC$.

## Are $A$, $B$, and $C$ collinear?

A simple way to answer this question is to determine if the line $\overline{AB}$ has the same *slope* as the line $\overline{AC}$. If they do have the same slope, then, because they both pass though $A$, they must be the same line! We have

{% math %}
\begin{aligned}
\text{slope of } \overline{AB} &= \frac{\Delta y}{\Delta x} = \frac{b_y - a_y}{b_x - a_x} \\
\text{slope of } \overline{AC} &= \frac{\Delta y}{\Delta x} = \frac{c_y - a_y}{c_x - a_x}
\end{aligned}
{% endmath %}

This leads to the following useful result: $A$, $B$, and $C$ collinear if and only if

{% math %}
\begin{aligned}
\text{slope of } \overline{AB} &= \text{slope of } \overline{AC} \\
\frac{b_y - a_y}{b_x - a_x} &= \frac{c_y - a_y}{c_x - a_x}
\end{aligned} \\
\boxed{(b_y - a_y)(c_x - a_x) = (c_y - a_y)(b_x - a_x)\;}
{% endmath %}

That looks great! We can easily turn that into code. (With a little thought, it's easy to see that this handles the case where $a_x = b_x = c_x$ and gives no false positives.)

## Is $P$ inside $\triangle ABC$?

There are many different ways to do this. My favorite method uses cross products. Let $\vec{a}$ be the vector with its base at $P$ and head at $A$. Define $\vec{b}$ and $\vec{c}$ similarly. Intuitively (using the [right-hand rule](https://en.wikipedia.org/wiki/Right-hand_rule#Cross_products)), **if $\vec{a} \times \vec{b}$ and $\vec{b} \times \vec{c}$ and $\vec{c} \times \vec{a}$ all point in the same direction, then $P$ is in $\triangle ABC$.** Let's make that more specific.

First, here are specific values for $\vec{a}$, $\vec{b}$, and $\vec{c}$:

{% math %}
\begin{aligned}
\vec{a} &= \vec{A} - \vec{P} = \langle a_x - p_x,\; a_y - p_y \rangle \\
\vec{b} &= \vec{B} - \vec{P} = \langle b_x - p_x,\; b_y - p_y \rangle \\
\vec{c} &= \vec{C} - \vec{P} = \langle c_x - p_x,\; c_y - p_y \rangle \\
\end{aligned}
{% endmath %}

Now, recall that the cross product of vectors $\vec{u} = \langle u_x, u_y, u_z \rangle$ and $\vec{v} = \langle v_x, v_y, v_z \rangle$ is

{% math %}
\begin{aligned}
\vec{u} \times \vec{v} = \langle u_yv_z - u_zv_y,\; u_zv_x - u_xv_z,\; u_xv_y - u_yv_x \rangle
\end{aligned}
{% endmath %}

Our vectors $\vec{a}$, $\vec{b}$, and $\vec{c}$ all lie in the $xy$-plane, meaning they all have a zero $z$-component. Looking at the formula above, this means that the cross products we're interested in ($\vec{a} \times \vec{b}$ and $\vec{b} \times \vec{c}$ and $\vec{c} \times \vec{a}$) are only going to have nonzero values in their $z$-components.

We now have the final piece of the puzzle: **$P$ lies inside $\triangle ABC$ if and only if $(\vec{a} \times \vec{b})_z$ and $(\vec{b} \times \vec{c})_z$ and $(\vec{c} \times \vec{a})_z$ are either all positive or all negative**. We can compute, e.g., $(\vec{a} \times \vec{b})_z$ as follows:

{% math %}
\begin{aligned}
(\vec{a} \times \vec{b})_z &= \vec{a}_x \vec{b}_y - \vec{a}_y \vec{b}_x \\
&= (a_x - p_x)(b_y - p_y) - (a_y - p_y)(b_x - p_x)
\end{aligned}
{% endmath %}

## Turn it into code!

We've done all the hard thinking already; all that remains is careful bookkeeping.

```javascript
function crossProductZ(u_x, u_y, v_x, v_y) {
  return u_x * v_y - u_y * v_x;
}

function isPinABC(a_x, a_y, b_x, b_y, c_x, c_y, p_x, p_y) {
  // determine if A, B, and C are collinear
  if ((b_y - a_y) * (c_x - a_x) === (c_y - a_y) * (b_x - a_x)) {
    return false;
  }

  // calculate all those cross products
  var axb = crossProductZ( a_x - p_x, a_y - p_y,  b_x - p_x, b_y - p_y );
  var bxc = crossProductZ( b_x - p_x, b_y - p_y,  c_x - p_x, c_y - p_y );
  var cxa = crossProductZ( c_x - p_x, c_y - p_y,  a_x - p_x, a_y - p_y );

  return (axb > 0 && bxc > 0 && cxa > 0) || (axb < 0 && bxc < 0 && cxa < 0);
}

```

## What does this have to do with computer graphics?

A lot, actually!
