---
title: Parametric Curve Matching circular
usemathjax: true
theme: minima
---
## Aim of task
+	Student knows how a multidimensional curve is produced from varying a single parameter (Handling mathematical symbols and formalism).
+	Student can find other parameters of the curve using a 3D visualization (represent mathematical entities, posing and solving mathematical problems, making use of aids and tools  ).
+	Student understands geometrical implications of individual parameters in a multi-dimensional curve (representing mathematical entities).
+	Student understands that exact geometric features can be approximated numerically (modelling mathematically).

| ![First impression](./images/parametric_curve_matching_circular_2023-05-25.png) |
|:--:|
| *First impression of the question* |

+ [XML Code](./XML/quiz-IDIAM-Parametric%20Curve%20Matching%20circular.xml)
## Question description

A 3D curve is plotted. It is a closed curve whose constant parameters are randomly selected upon starting the task. Its equation is
```math
t \mapsto \begin{pmatrix} r \cdot \cos(t) \\ r \cdot \sin(t) \\ h \cdot \cos(n \cdot t - \phi) \end{pmatrix}. 
```
A second curve given by the same equation is plotted that can be varied using sliders.
That way, by matching the two curves, the parameters can be interactively obtained.
The task is to find the correct values for $r$, $h$, $\phi$ and $n$ in real numbers or integer numbers.


### Student perspective

The student sees a cartesian coordinate system and a curve plotted in 3D.

It is the task to reconstruct the parameters of the 3D curve. In order to do this they have to find out the radius, amplitude, phase shift and number of oscillations by matching a second curve to the given one using sliders. If they overlap exactly, the parameter values can be read from the sliders and are automatically given as numerical values.

| ![Click draw button](./images/parametric_curve_matching_circular_student_2023-05-25.png) |
|:--:|
| *When the student solves the problem* |


### Teacher perspective
The teacher is able to give a list of possible values for parameters. In order to do this, they simply need to modify the entries in the lists specified e.g. change `phaser : rand([1/6, 1/4, 1/3, 1/2, 2/3, 3/4, 5/6 ,1]);` to `phaser : rand([1/6, 1/4, 1/3, 1/2]);`. 

Another example - in the case of the radius - is the following: change `radiusr:rand(6)/2` to `radiusr: rand(8)/2` in order to select numbers from 0 to 4 in steps of 1/2.

For an explanation of the processing of the values read **Question variables** and **Question text**.


| ![values the teacher can change](./images/Parametric_curve_matching_circular_teacher_2023-03-25.jpg) |
|:--:|
| *The above image shows which values the teacher may wish to change* |


## Question code

### Question Variables
+	`radiusr` and `ampr` are randomly selected integer numbers created using `rand()`, they are divided by 2 and 4 respectively to allow for non-integer steps
+ `phaser`is randomly selected from a list of possible values using `rand()`
+ `numr`is a randomly selected integer number created using `rand()`
+ The value of `phaser` is multiplied by pi and saved to `phase` as a numerical value. This is done in between `numer:true` and `numer:false`.
+ The value of `phaser` is multiplied by pi to use it for plotting in **Question text**.



#### Question variable code
```jacascript
/*randomize radius, amplitude, phase shift, number of oscillations*/

radiusr: 1+rand(6)/2;
ampr: 1/4+rand(10)/4; 
phaser : rand([1/6, 1/4, 1/3, 1/2, 2/3, 3/4, 5/6 ,1]);
numr: 1+rand(5);

/* prepare numerical values for JSXGraph */
numer:true
phase:phaser*%pi;
numer:false

/*symbolic data for checking*/
phaser: phaser*%pi
```

### Question Text
Reconstruct the blue 3D curve dependent on a parameter $t$.

In order to do so, you need to find the correct radius $r$, amplitude $h$, phase shift $\phi$ and number of oscillations $n$.

The curve is the trace of the function 
```math
t \mapsto \begin{pmatrix} r \cdot \cos(t) \\ r \cdot \sin(t) \\ h \cdot \cos(n \cdot t - \phi) \end{pmatrix}.
```

Use the sliders in the JSXGraph applet or type in your reply right below it.
	

+ Task explanation using LaTex
+	JSXGraph applet using and variables defined in **Question variables** plotting the 3D curve
	+	`[[input:ans1]]`, `[[input:ans2]]`, `[[input:ans3]]` and `[[input:ans4]]` at the end of JSXGraph code to allow input of  answers of the student for r, h and phase shift and n respectively
+	`[[validation:ans1]]`,  `[[validation:ans2]]` , `[[validation:ans3]]` and `[[validation:ans4]]`  are used for checking of answer

#### Question text code


```javascript
<p>Reconstruct the blue 3D curve dependent on a parameter \(t\).</p>
<p>In order to do so, you need to find the correct radius \(r\), amplitude \(h\), phase shift \(\phi\) and number of oscillations \(n\).</p>
<p> The curve is the trace of the function \( t \mapsto \begin{pmatrix} r \cdot \cos(t) \\ r \cdot \sin(t) \\ h \cdot \cos(n \cdot t - \phi) \end{pmatrix} \). </p>
<p>Use the sliders in the JSXGraph applet or type in your reply right below it.</p>

[[jsxgraph width="500px" height="500px" input-ref-ans1='ans1Ref' input-ref-ans2='ans2Ref' input-ref-ans3='ans3Ref' input-ref-ans4='ans4Ref' ]]
var board = JXG.JSXGraph.initBoard(divid,{boundingbox : [-10, 10, 10,-10], axis:false, shownavigation : false});
                        var box = [-5,5];
                        var view = board.create('view3d',
		                            [[-6, -3], [8, 8],
		                            [box, box, box]],
		                            {});
			
			//curve from STACK values
			var r = {#radiusr#};
			var h = {#ampr#};
			var p = {#phase#};
			var n = {#numr#};
			var c_base = view.create('curve3d', [
				(t) => r* Math.cos(t),
				(t) => r* Math.sin(t),
				(t) => h* Math.cos(n *t-p),
				[-Math.PI, Math.PI]], {strokeWidth: 2, strokeColor: "#1f84bc"});

			//slider-based curve 

			var rslide = board.create('slider', [[-7,-6], [3,-6], [0,5,10]], {name: "r", snapwidth:0.1, highline: {strokeColor: '#EE442F'}, baseline: {strokeColor: '#EE442F'}});
			var hslide = board.create('slider', [[-7,-7],[3,-7],[0,2,4]], {name: 'h', snapwidth:0.05, highline: {strokeColor: '#EE442F'}, baseline: {strokeColor: '#EE442F'}});
			var nslide = board.create('slider', [[-7,-9],[3,-9],[0,4,7]], {name: 'n', snapWidth:1, highline: {strokeColor: '#EE442F'}, baseline: {strokeColor: '#EE442F'}});
			var sslide = board.create('slider', [[-7,-8],[3,-8],[0,2,Math.PI]], {name: 'phase shift', snapwidth:0.05, highline: {strokeColor: '#EE442F'}, baseline: {strokeColor: '#EE442F'}});
			stack_jxg.bind_slider(ans1Ref,rslide);
			stack_jxg.bind_slider(ans2Ref,hslide);
			stack_jxg.bind_slider(ans3Ref,sslide);
			stack_jxg.bind_slider(ans4Ref,nslide);
			
			var c = view.create('curve3d', [
				(t) => rslide.Value()* Math.cos(t),
				(t) => rslide.Value()* Math.sin(t),
				(t) => hslide.Value()* Math.cos(nslide.Value()*t- sslide.Value()),
				[-Math.PI, Math.PI]], {strokeWidth: 2, strokeColor: '#EE442F'});

                       /* axis labels*/
                       var xlabel=view.create('point3d',[0.9*box[1],0,(0.6*box[0]+0.4*box[1])], {size:0,name:"x"});
                       var ylabel=view.create('point3d',[0,0.9*box[1],(0.6*box[0]+0.4*box[1])], {size:0,name:"y"});
                       var zlabel=view.create('point3d',[
                           0.7*(0.6*box[0]+0.4*box[1]),
                           0.7*(0.6*box[0]+0.4*box[1]),
                           0.9*box[1]], 
                           {size:0,name:"z"});

[[/ jsxgraph]]
<p>\(r=\) [[input:ans1]] [[validation:ans1]]</p>
<p>\(h=\) [[input:ans2]] [[validation:ans2]]</p>
<p>\(\phi=\) [[input:ans3]] [[validation:ans3]]</p>
<p>\(n=\) [[input:ans4]] [[validation:ans4]]</p>
```
## Answers
### Answer ans 1
|property | setting| 
|:---|:---|
|Input type | Numerical|
|Model answer | `radiusr` defined in **Question variables** |
| Forbidden words | none |
| Forbid float | No |
| Student must verify | Yes |
| Show the validation | Yes, with variable list|
---
### Answer ans 2
|property | setting| 
|:---|:---|
|Input type | Numerical|
|Model answer | `ampr` defined in **Question variables** |
| Forbidden words | none |
| Forbid float | No |
| Student must verify | Yes |
| Show the validation | Yes, with variable list|
---
### Answer ans 3
|property | setting| 
|:---|:---|
|Input type | Numerical|
|Model answer | `phaser` defined in **Question variables** |
| Forbidden words | none |
| Forbid float | No |
| Student must verify | Yes |
| Show the validation | Yes, with variable list|
---
### Answer ans 4
|property | setting| 
|:---|:---|
|Input type | Numerical|
|Model answer | `numr` defined in **Question variables** |
| Forbidden words | none |
| Forbid float | No |
| Student must verify | Yes |
| Show the validation | Yes, with variable list|
## Potential response tree
### prt1

Feedback variables:
```
radiusans: ans1
ampans: ans2

```

| ![prt1](./images/parametric_curve_matching_circular_2_prt_1_2023-05-06.png) |
|:--:|
| *Visualization of **prt1*** |



### Node 1
|property | setting| 
|:---|:---|6
|Answer Test | NumAbsolute|
|SAns | `radiusans`|
|TAns | `radiusr`| 
|Test options | 0.1 |
|Node 1 true feedback | `<p> Nice! You found the correct radius \(r\). Good job!</p>`|
|Node 1 false feedback |`<p>The radius \(r\) is not yet correct. Check, whether the curves align, when you slide "r" to this value.</p>`|


| ![Node 1](./images/parametric_curve_matching_circular_2_prt_1_node_1_2023-05-06.png) |
|:--:|
| *Values of **node 1*** |

### Node 2
 |property | setting| 
|:---|:---|
|Answer Test | NumAbsolute|
|SAns | `ampans`|
|TAns | `ampr`| 
|Test options | 0.1 |
|Node 2 true feedback | `<p>Nice! You also found the correct amplitude a. Good job!</p>`|
|Node 2 false feedback |`<p>The amplitude a is not yet correct. Check, whether the curves align, when you slide "h" to this value.</p>`|

| ![Node 2](./images/parametric_curve_matching_circular_2_prt_1_node_2_2023-05-06.png) |
|:--:|
| *Values of **node 2*** |

### Node 3
 |property | setting| 
|:---|:---|
|Answer Test | NumAbsolute|
|SAns | `ampans`|
|TAns | `ampr`| 
|Test options | 0.1 |
|Node 3 true feedback | `<p>Nice! You found the correct amplitude a. Good job!</p> <p> Check whether the curves align perfectly to get the radius right as well! </p>`|
|Node 3 false feedback |`<p>The amplitude a is not yet correct as well. Check, whether the curves align, when you slide "h" to this value.</p> <p> If the orange and blue curve overlap, the answer will be correct! </p>`|

| ![Node 3](./images/parametric_curve_matching_circular_2_prt_1_node_3_2023-05-06.png) |
|:--:|
| *Values of **node 3*** |




### prt2

Feedback variables:
```
phaseans: ans3
numans: ans4
```
| ![prt2](https://user-images.githubusercontent.com/120648145/236681208-294dd091-772c-4afa-9c25-d961f8cd8e2f.png) |
|:--:|
| *Visualization of **prt2*** |



### Node 1
|property | setting| 
|:---|:---|
|Answer Test | NumAbsolute|
|SAns | `phaseans`|
|TAns | `phaser`| 
|Test options | 0.1 |
|Node 1 true feedback | `<p>Nice! You found the correct phase shift \(\phi\). Good job!</p>`|
|Node 1 false feedback |`<p>The phase shift \(\phi\) is not yet correct. Check, whether the curves align, when you slide "phase shift" to this value.</p>`|


| ![Node 1](./images/parametric_curve_matching_circular_2_prt_2_node_1_2023-05-06.png) |
|:--:|
| *Values of **node 1*** |

### Node 2
 |property | setting| 
|:---|:---|
|Answer Test | NumAbsolute|
|SAns | `numans`|
|TAns | `numr`| 
|Test options | 0.1 |
|Node 2 true feedback | `<p>Nice! You also found the correct number of oscillations \(n\). Good job!</p>`|
|Node 2 false feedback |`<p>The number of oscillations \(n\) is not yet correct. Check, whether the curves align, when you slide "n" to this value. Note, that you can count the maxima of the curve and try again.</p>`|

| ![Node 2](./images/parametric_curve_matching_circular_2_prt_2_node_2_2023-05-06.png) |
|:--:|
| *Values of **node 2*** |

### Node 3
 |property | setting| 
|:---|:---|
|Answer Test | NumAbsolute|
|SAns | `numans`|
|TAns | `numr`| 
|Test options | 0.1 |
|Node 3 true feedback | `<p>Nice! You also found the correct number of oscillations \(n\). Good job!</p><p> Make sure to get the phase shift right as well by matching the curves! </p>`|
|Node 3 false feedback |`<p>The number of oscillations \(n\) is also not yet correct. Check, whether the curves align, when you slide "n" to this value. Note, that you can count the maxima of the curve and try again.</p>`|

| ![Node 3](./images/parametric_curve_matching_circular_2_prt_2_node_3_2023-05-06.png) |
|:--:|
| *Values of **node 3*** |

## Todo:
* [x] check grading
* [x] check why randomization does not work
* [x] update figures
