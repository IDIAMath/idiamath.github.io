---
title: JSXGraph Sandbox
---

# Read Some Properties From A Graph

I split the following question into two problems for now
due to a problem with creating 3 PRT's,
although i could create 2 PRT's as you can tell in part 2.

## Question Interpretation

> Given a a surface defined by $z=f(x,y)$, where exact expression for f is unknown to a user, ask the user to select a point on it where partial derivatives are positive/negative/zero. Given the same, ask the user to select local maxima/minima.

I interpret this as: find a point on the functions where one of the three derivatives gives a positive number, another gives a negative and the third gives 0. For example, you could have a point that gives Fxy=1, Fx = 0, Fy=-1, that would be a solution.

It can also be interpreted as: select a point on the function where ALL partial derivatives would give a negative positive or 0. If so, this would be very similar to the provided solution and it would be an easy task change the solution to accommodate the following interpretation.

## Part 1

+ [Moodle XML](questions-majd testing-Question#1 part 1 unknown F-20220626-1354.xml)

Example functions:
$$F(x,y) = x*y^2-x$$

The answer would be for example `[ any_negative_number, 1]`


## Part 2

+ [Moodle XML](questions-majd testing-Question#1 part 2 local minmax-20220626-1355.xml)

Example functions:
$$F(x,y)=2*x^4+2*y^4-8*x*y+12$$

The answer for local min would be: `[[-1,-1],[1,1]]`

For local max there is no local max so the answer would be just two square parenthesis: []


## Question Problems

+ May provide inaccurate feedback, when the provided answer is half wrong (part 2).
+ 'select' a point on graph there is a multitude of problems not thought through with this idea.
+ Does not support trig functions, or any predefined functions such as log, ln, e...



# Give an example of a function with certain properties