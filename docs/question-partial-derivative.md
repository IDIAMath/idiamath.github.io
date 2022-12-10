---
title: STACK Question - Interpret partial derivatives
usemathjax: true
theme: minima
---

## Question Interpretation

> Given a a surface defined by $$z=f(x,y)$$, where exact expression for $$f$$ is unknown to a user, ask the user to select a point on it where partial derivatives are positive/negative/zero.

| ![image](https://user-images.githubusercontent.com/43517080/193473705-113236f8-2727-444c-867e-23f526f04407.png) |
|:--:|
| * First impression of the question* |

### Question description
Find one (x,y) **coordinates** on the functions where all first order partial derivative `(Fx,Fy,Fxy)` values on the corresponding coordinates provide values with unique signs from each other `(+, -, 0/neutral)`.

In different words: the signs infront of the real numbers derived from inputing the coordinates into the partial derivatives of a given function should be different from each other. We have three unique signs in math: 0, +, and - (plus, minus and 0/neutral).

- [XML Code](XML/Question 1 part 1 unknown F - 202207.xml)

#### Example

Example function:
$$F(x,y) = x\cdot y^2-x$$

The answer would be for example `[ any_negative_number, 1]`


### Student perspective
The student has to type in the correct answer in the `[x,y]` input field.

#### Show/hide
Check this box if you want to hide a given graph

| ![hiddenGraph](https://user-images.githubusercontent.com/43517080/193473928-6575d055-597d-4abc-8163-dc32fed5b3a2.png) |
|:--:|
| *The above image displays how the student can check a checkbox corresponding to a given graph, to set change the mentioned graphs visibility* |

### Teacher perspective
The teacher does not have to change anything, but they may choose to add or delete constants or chanage the function itself.

| ![image](https://user-images.githubusercontent.com/43517080/178975348-eeeacdce-7cac-47bc-ac33-68953c929989.png) |
|:--:| 
| *the above image shows which values the teacher may choose to change* |


## Question code


### Question variables
We create 2 variables a and b that have a random number ranging between 10 and -10. These numbers can be multiplied by the function to achieve a level of 'randomness'.
The **f** variable is where we store the function.
variables: `fy`,`fx` and `fxy` give us the needed partial derivatives for the function **f**.
```rust
a:rand_with_prohib(-10,10,[0]);
b:rand_with_prohib(-10,10,[0]);
f:x*y^2-x;

fx:diff(f,x);
fy:diff(f,y);
fxy:diff(fy,x);
```


### Question text
The objectives here are the following:
- Display the main function and its desired partial derivatives. 
- Be able to hide any given function by crossing a checkbox

The proposed objectives can be achieved following these procedures:
- **1 Segment** We create and plot the functions, then we store them in an array 
- **2 Segment** Here a for loop is used to create a checkbox for each function.
- **3 Segment** The **'graphSelectVisibility'** function detects weather a checkbox element is checked or unchecked. It's activated when a user clicks on input element of type 'checkbox'.



```javascript
<p>drag the black point to move the red point on the graph</p>

<p></p>
<p><span style="font-size: 0.9375rem;" id="question">Given a surface defined by z=f(x,y), and Fxy where exact expression for f is unknown. Select a point where
partialt derivatives are positive/negative/zero</span><br></p>

[[jsxgraph height='750px' width='1550px']]

// set the value of input field to []
document.getElementsByClassName("algebraic")[0].value = "[x,y]";

//SEGMENT 1 _________ Creating and storing functions_______________
var functionArr = [];
var labelArr = [];
var selected = false;

var board = JXG.JSXGraph.initBoard(divid, {
boundingbox: [-8, 8, 8, -8],
keepaspectratio: false,
axis: false
});

/* x, y and z axis scale and visibility*/

var box = [-10,10];
var view = board.create('view3d', [[-6, -3], [8, 8],[box, box, box]],{ xPlaneRear: {visible: false}, yPlaneRear: {visible:false}});

view.D3.az_slide._smax = 12;


//graph f
var FExpr = '{#f#}';

var F = board.jc.snippet(FExpr, true, 'x,y', true);
var FGraph = view.create('functiongraph3d',[F,box,box,], { strokeWidth: 0.5, stepsU: 70, stepsV: 70 });
functionArr.push(FGraph);
FGraph.setLabel(FExpr);
labelArr.push(FExpr);

//the point that controlls the point on the graph;
var Axy = view.create('point3d', [2, 2, -5], { withLabel: false, color:'gray',strokeWidth:5 });

//the point reflected on the graph
var A = view.create('point3d',[ function() {return Axy.D3.X()}, function(){return Axy.D3.Y()},
function(){return F(Axy.D3.X(), Axy.D3.Y())}], { withLabel: false, color:'red' });

//graph fxy
var FxyExpr = '{#fxy#}';

var Fxy = board.jc.snippet(FxyExpr, true, 'x,y', true); // JessieCode parsing
// 3D surface
var FxyGraph = view.create('functiongraph3d', [
Fxy,
box,
box,
], { strokeWidth: 0.5, stepsU: 70, stepsV: 70,color:'orange'});

functionArr.push(FxyGraph);
FxyGraph.setLabel(FxyExpr);
labelArr.push(FxyExpr);

//SEGMENT 2 _________ Checkbox elements created_______________
// create checkbox for every stored function in the array 
for(var i = 0; !(i == functionArr.length);i++) {
var input = document.createElement("input");
var span = document.createElement("span");
var br = document.createElement("br");


input.setAttribute("type","checkbox");
input.setAttribute("class","graphToggle");
span.innerHTML = "hide/show "+ labelArr[i];
document.getElementsByClassName("clearfix")[0].appendChild(span);
document.getElementsByClassName("clearfix")[0].appendChild(input);
document.getElementsByClassName("clearfix")[0].appendChild(br);
input.addEventListener("change",graphSelectVisibility);

}
//SEGMENT 3 _________ Hide/show function_______________
/* on/off graph toggle*/
function graphSelectVisibility() {

var input = document.getElementsByClassName('graphToggle');
for(var i =0; !(i == functionArr.length);i++){
if(input[i].checked == true) {
functionArr[i].setAttribute({visible:false});
} else {
functionArr[i].setAttribute({visible:true});
  }
 }
}

[[/jsxgraph]]
<p></p>
<p><span>The point = [[input:ans1]][[validation:ans1]] </span></p>
```

### Feedback variabels
The objective here is:
- Determine if the signs infront of the real numbers derived from inputing the coordinates into the partial derivatives of a given function are indeed different from each other as the question descriptions states

#### Procedure:
We need to check for each partial derivative, which sign it provides on the provided user coordinates. give a score of 6 if a partial derivative is equal to zero, a score of 3 if it is a positive number, and a score of 1 if it is a negative number. If the values from partial derivatives are unique (unique signs infront of all 3 values), then they should add up to 10. The numbers 1, 3 and 6 are not chosen randomley, rather strategically (think combinatorics).

Lastley if the score adds up to 10 we set the student answer to be the correct answer. Otherwise we set a random long integer to be the correct answer, this is because we are required to insert an answer for the question. Ideally we would compute all the possible coordinates that would satisfy the requirements for a correct answer (unique signs), but it is almost impossible to do; so, we choose this approach instead.
```rust
score:0;
sa1:if ev(fx,x=ans1[1],y=ans1[2]) = 0 then score: score+6 else if ev(fx,x=ans1[1],y=ans1[2]) >0 then score: score+3 else if ev(fx,x=ans1[1],y=ans1[2]) <0 then score:score +1; 
sa2:if ev(fy,x=ans1[1],y=ans1[2]) = 0 then score: score+6 else if ev(fy,x=ans1[1],y=ans1[2]) >0 then score: score+3 else if ev(fy,x=ans1[1],y=ans1[2]) <0 then score:score +1; 
sa3:if ev(fxy,x=ans1[1],y=ans1[2]) = 0 then score: score+6 else if ev(fxy,x=ans1[1],y=ans1[2]) >0 then score: score+3 else if ev(fxy,x=ans1[1],y=ans1[2]) <0 then score:score +1; 
/*if the student answer is right, then the teachers answer is equivelant to stud answer, otherwise, the teachers answer needs to have a value, so the best solution is 
 set the value to a random code, that no student will ever get right or know exists*/
ta: if score =10 then ans1 else [rand(50)+10000,rand(50)+10000];
```

### Node 1 
AnswerTest: AlgEquiv, SAns:Ans1, TAns:ta
