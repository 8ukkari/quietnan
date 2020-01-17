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

```cpp
#pragma once
#include <string>
#include <sstream>
#include <tuple>

#include "../Eigen/Core"


class Line
{
    Eigen::Vector3d root_;
    Eigen::Vector3d dir_;

public:
    Line()
        : root_(0, 0, 0)
        , dir_(0, 0, 1)
    {}


    Line(const Eigen::Vector3d& o, const Eigen::Vector3d& d)
        : root_(o)
        , dir_(d.normalized())
    {}


    Eigen::Vector3d position(double param) const { return root_ + param * dir_; }


    Eigen::Vector3d& root() { return root_; }
    const Eigen::Vector3d& root() const { return root_; }


    Eigen::Vector3d& dir() { return dir_; }
    const Eigen::Vector3d& dir() const { return dir_; }
};


std::tuple<Eigen::Vector3d, double> project(const Line& line, const Eigen::Vector3d& point)
{
    auto param = (point - line.root()).dot(line.dir());
    return std::make_tuple(line.position(param), param);
}


inline std::string str(const Eigen::Vector3d& vec)
{
    std::stringstream ss;
    ss << vec.x() << ", " << vec.y() << ", " << vec.z();
    return ss.str();
}


int main(int argc, char* argv[])
{
    auto line = Line(Eigen::Vector3d(0, 0, 0), Eigen::Vector3d(1, 1, 0));
    auto point = Eigen::Vector3d(1, 0, 0);
    auto pos_param = project(line, point);
    std::cout << "line root = (" << str(line.root()) << ")\n";
    std::cout << "line dir  = (" << str(line.dir()) << ")\n";
    std::cout << "point to be projected = (" << str(point) << ")\n";
    std::cout << "projected point = (" << str(std::get<0>(pos_param)) << ")\n";
    std::cout << "parameter on projected point on line = " << std::get<1>(pos_param) << "\n";
    return 0;
}
```
