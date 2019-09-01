---
title: 3D line data structure and point projection onto line
date: "2019-01-16T10:00:00Z"
template: "post"
draft: false
slug: "/posts/3d-line-data-structure-and-point-projection-onto-line/"
category: "programming"
tags:
  - "c++"
  - "geometry"
description: "3D point and line is basic data structure in geometry calculus. This post explains parametric form of 3D line and how to prject point onto line in C++ implementation."
socialImage: ""
---

This post explains parametric form of line and how to project a point onto a line.

## Parametric form of line

3D line is expressed 2 vector coefficients, root coordinate and direction vector.

$$
\vec{P(k)} = \vec{r} + k\vec{d}
$$

- $P(k)$ : 3D coordinate on a line defined with root point and its direction
- $r$ : 3D coordinate of origin point of the line
- $d$ : 3D vector defining line direction (normalized vector is suitable)

![3D line](/media/3d-line-data-structure-and-point-projection-onto-line/3d-line.png)

When norm of direction $\vec{d}$ is 1, parameter $k$ is equivalent to distance from $\vec{4}$ along $\vec{d}$.

## Projection

You need to calculate parameter value of projected on the line, then you can get position coordinate of projected point with the value and the line equation. The following is calculation steps:

- Calculate parameter of projected point on the line
- Calculate projected point coordinate on the line with the parameter

**Sample code in C++:**

`gist:08b58c431fe23338fd1836ec825354ad`
