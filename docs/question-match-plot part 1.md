---
title: STACK Question - Function to match given properties part 1
usemathjax: true
theme: minima
---

# Read Some Properties From A Graph

## Question Interpretation

> Given a a surface defined by $$z=f(x,y)$$, where exact expression for $$f$$ is unknown to a user, ask the user to select a point on it where partial derivatives are positive/negative/zero (Part 1). Given the same, ask the user to select local maxima/minima (part 2).

I interpret this as: find a point on the functions where one of the three derivatives gives a positive number, another gives a negative and the third gives 0/neutral. For example, you could have a point that gives Fxy=1, Fx = 0, Fy=-1, which would be a solution.



## Part 1

| ![Capture](https://user-images.githubusercontent.com/43517080/178970093-ca6c50b4-d9b4-45a5-9687-575a896a4fc0.PNG) |
|:--:|
| *This image shows the students first view of the question* |

- [XML Code](XML/Question 1 part 1 unknown F - 202207.xml)

Example functions:
$$F(x,y) = x\cdot y^2-x$$

The answer would be for example `[ any_negative_number, 1]`


### Student perspective
The student has to type in the correct answer in the `[x,y]` input field.

#### Visible on hover
Check this box if you want all other graphs to become invisible when you hover on one graph

| ![cursorHover](https://user-images.githubusercontent.com/43517080/178970454-c67b02e0-ce4a-4d4c-a747-53f26593c972.PNG) |
|:--:| 
| *The above image shows the visible on hover functionality, where the student is able to hover on one graph to make all other graphs invisible* |


#### Show/hide
Check this box if you want to hide a given graph

| ![hiddenGraph](https://user-images.githubusercontent.com/43517080/178970723-a23680ae-f859-4c6d-92b7-7bf311f77f45.PNG) |
|:--:|
| *The above image displays how the student can check a checkbox corresponding to a given graph, to set change the mentioned graphs visibility* |

### Teacher perspective
The teacher does not have to change anything, but they may choose to add or delete constants or chanage the function itself.

| ![image](https://user-images.githubusercontent.com/43517080/178975348-eeeacdce-7cac-47bc-ac33-68953c929989.png) |
|:--:| 
| *the above image shows which values the teacher may choose to change* |


## Quesion code


### Quesion variables

```html
a:rand_with_prohib(-10,10,[0]);
b:rand_with_prohib(-10,10,[0]);
f:x*y^2-x;

fx:diff(f,x);
fy:diff(f,y);
fxy:diff(fy,x);
```


### Quesion text

```html
<p>drag the black point to move the red point on the graph</p>




<p></p>
<p><span style="font-size: 0.9375rem;" id="question">Given a surface defined by z=f(x,y), and Fxy where exact expression for f is unknown. Select a point where partial derivatives are positive/negative/zero</span><br></p>

[[jsxgraph height='750px' width='1550px']]

// set the value of input field to []
document.getElementsByClassName("algebraic")[0].value = "[x,y]";

// checkbox visibility on hover creation

var checkbox = document.createElement("input");
var span = document.createElement("span");
var br = document.createElement("br");
var br2 = document.createElement("br");

checkbox.setAttribute("type","checkbox");
span.innerHTML = `Visible on hover`;
document.getElementsByClassName("clearfix")[0].appendChild(span);
document.getElementsByClassName("clearfix")[0].appendChild(checkbox);
document.getElementsByClassName("clearfix")[0].appendChild(br);
document.getElementsByClassName("clearfix")[0].appendChild(br2);


checkbox.addEventListener("change",graphHoverVisibility);

//constants
var functionArr = [];
var labelArr = [];
var selected = false;

var board = JXG.JSXGraph.initBoard(divid, {
boundingbox: [-8, 8, 8, -8],
keepaspectratio: false,
axis: false
});

/* how much of the graph is visible, you will be able to change the x and y position of the point in accordance to the range provided in the box variable. the answer does not have to be confined to this, yuo can give[15,-9] as the answer for the question. you can also change it to whichever part of the graph you wish to be visible*/

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
var A = view.create('point3d',[ function() {return Axy.D3.X()}, function(){return Axy.D3.Y()},function(){return F(Axy.D3.X(), Axy.D3.Y())}], { withLabel: false, color:'red' });

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










/* create checkbox for every function3d .
the 'advantage' of lack of element tracing is
i know in which order i created the graphs
*/
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

FxGraph.setAttribute({highlightStrokeColor:'#f5ad42'});
FGraph.setAttribute({highlightStrokeColor:'#00ccff'})


/*this function hides all other graphs
*/
function selectedGraph(graph) {
graph.on('mouseover', function(){
if(!selected) {
if (checkbox.checked ===false) {
selected = true;

if(graph.getAttribute(highlight) == true) {
for(var i =0; !(i == functionArr.length); i++) {
functionArr[i]
}
}

}else {
graph.setAttribute({highlight:true});
for(var i =0; !(i == functionArr.length); i++) {
functionArr[i].setAttribute({visible:false});
}

graph.setAttribute({visible:true});


graph.on('mouseout', function(){
for(var i =0; !(i == functionArr.length); i++) {
functionArr[i].setAttribute({visible:true});
}
selected = false;
})
}

}
})
}


function graphHoverVisibility() {

if (checkbox.checked ===true) {

for(var c =0; !(c == functionArr.length); c++) {
selectedGraph(functionArr[c]);
}
}//end if
}//end functionvisibility

[[/jsxgraph]]
<p></p>
<p><span>The point = [[input:ans1]][[validation:ans1]] </span></p>
```

### Feedback variabels
```html
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
