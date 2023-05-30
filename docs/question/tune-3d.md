---
title: Question - Tune 3D
usemathjax: true
---

# Question

![Screenshot - Tune 3D](tune-3D.png)

The student is given a plot of a function of the form

$$ f(x,y) = by^2 + axy + cx^2$$

where $$b$$ and $$c$$ are known constants, randomly instantiated, and
$$a$$ is an unknown constant to be determined by the student.

The student sees two superimposed surface plots.
One with the unknown value for $$a$$, and another where the
value of $$a$$ can be controlled by a slider.

The student will have to use the slider to find the value for $$a$$
that makes the two functions coincide.

- [XML Code](XML/tune-3d.xml)

# How it works

## Question Variables (Maxima)

```maxima
/*Generate list with 3 random values for the coefficients*/
coef: makelist(rand_with_prohib(-4,4,[0]),3);

/*Polynomial with parameters a,b,c*/
func : b*y^2+a*x*y+c*x^2;

/*List of all parameter values*/
all_param: [a=coef[1], b=coef[2], c=coef[3]];

/*List of non-tunable parameter values (b,c)*/
non_tunable: [b=coef[2], c=coef[3]];

/*Tunable parameter*/
tunable:a;

/*x,y domains*/
xrang:[-7,7];
yrang:[-7,7];

/*Tolerance*/
maxError:0.2;

/*Function evaluated with all parameters*/
fxy2:ev(func, all_param);

/*Function evaluated without unknown parameter value*/
fxy:ev(func,non_tunable);
```

## Question Text

First we create a *hidden* input box which will
be manipulated from javascript only.

```html
<p style="display:none">[[input:ansa]] [[validation:ansa]]</p>
```

The question text proper is straight forward.

```html
We are given the function \(F(x,y) = {@fxy@}\) where the parameter \({@tunable@}\) is unknown.
are unknown. The figure below shows the correct function,
along with a function where you can tune the unknown parameter with the slider.
Find the correct function by manipulating the slider.
```

The critical part is the javascript code, in `[[jsxgraph]]` tags.

```javascript
[[jsxgraph input-ref-ansa="ansaRef"]]

   //Create the board
   var board = JXG.JSXGraph.initBoard(divid, {
      boundingbox: [-8, 8, 8, -8],
      keepaspectratio: false,
      axis: false
   });

   //Create the 3D-view
   var boxz = [-7, 7];
   var boxx = [-7,7];
   var boxy = [-7,7];
   var view = board.create('view3d', [[-6, -3], [8, 8], [boxx, boxy, boxz]], {
      xPlaneRear: {visible: false},
      yPlaneRear: {visible: false},
   });

   // Get the name of the tunable parameter from maxima
   var tunable_param = "{#tunable#}";

   //Create tunable slider - make sure to give it the correct name
   //from var tunable_param, this makes it "influence" the graph-function
   var tuning_slider = board.create('slider',
      [[-7, -6], [5, -6], [-4,0,4]],
      { name: tunable_param }
   );

   /*
    * Parse the function expressions with and without
    * known parameter from maxima. The Jessie code parsing
    * returns a callable function, and variables with the same name as
    * any sliders on the board gets the value of the sliders
    */
   var func_tunable = board.jc.snippet('{#fxy#}', true, 'x,y', false);
   var func = board.jc.snippet('{#fxy2#}', true, 'x,y', false);

   //Create the functiongraphs of both functions
   var graph_tunable = view.create('functiongraph3d',
      [func_tunable, boxx, boxy], {
      strokeColor: JXG.palette.blue,
      stepsU: 50, stepsV: 50, strokeWidth: 0.5
   });
   var graph = view.create('functiongraph3d', [func, boxx, boxy], {
      strokeColor: JXG.palette.red,
      stepsU: 50, stepsV: 50, strokeWidth: 0.5
   });

   /*
    * Bind the parameter-slider to the stack answer variable
    * The answerfield will contain the value of the slider,
    * and when the page reloads the slider will load any previous value
    * from the answerfield (remember to call board.update() to show this effect)
    */
   stack_jxg.bind_slider(ansaRef, tuning_slider);
   board.update();


[[/jsxgraph]]
```
