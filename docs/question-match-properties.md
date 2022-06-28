---
title: Question - find a function with given properties
usemathjax: true
theme: minima
---



> Ask the user to enter a function defined by algebraic expression such that certain properties are in place. Use graphics to plot the surface with corresponding properties before it is evaluated formally.
>
> Property example: partial derivatives at all points with coordinates (t,t) are positive.

## Question description
Random x, y coordinates are generated, the user is asked to provide a function that will result in positive output for all first derivatives of the given function at the generated coordinates. The user may choose to draw the provided function by clicking on the 'Draw Function' button.

Although simple, this question demonstrates how STACK and JSXGraph can be complementary. STACK works to provide the user with input feedback in realtime. For example the user may type xy instead of x*y, stack will highlight this error. This is useful as it omits the need for string manipulation in JS, saves time.

+ [Moodle XML](questions-majd testing-Question 2 define function with certain properties-20220628-0956 (1).xml)



## Question Improvements (Upcoming)
- Random property questions, for example sometimes the question demands the user provde a function where partial derivatives at all points with coordinates (t,t) are positive sometimes **NEGATIVE** ..and more and more similar random questions.
- When user submits/checks answer, they are provided with graphs of Fx, Fy, and Fxy (partial derivatives). The graphs will be highlighted in green or red depending on the result of the answer.


## Question CODE

### Question Variables

### Question Text

### Specific feedback





