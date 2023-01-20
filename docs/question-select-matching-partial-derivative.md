---
title: STACK Question - Select matching partial derivative
usemathjax: true
theme: minima
---

## Question Interpretation

> Given a function drawn on the 3D room, select the matching partial derivative

| ![image](https://user-images.githubusercontent.com/43517080/210074812-a0b0480e-6703-4143-a6cd-2d64b93860c0.png) |
|:--:|
| *First impression of the question* |

### Question description
User has to select partial derivative for the given and plotted function expression.


xml code 
- [XML Code](XML/question-select-matching-partial-derivative.xml) 


## Question code

### Question variables

```rust
/*random variables to randomize functions*/
a:rand_with_prohib(-3,3,[0]);
b:rand_with_prohib(-3,3,[0]);

/* Function expressions one of which corresponds to the answer*/
f1:a*3*x+b*4*y;
f2:a*2*x-b*5*y;
f3:a*1*x^2+b*2*y^2;
f4:a*4*x^2+b*1*y^2;
f5:a*2*x^2+b*1*y;

/*insert all the function expressions in an array*/
inputs :[f1,f2,f3,f4,f5];

/*Select a random function expression  from the array of functions*/
k:rand_with_prohib(1,length(inputs),[0]);
random_function:inputs[k];


```
### Question text

```JavaScript
<p style="font-size:1.3em">Select the partial derivative Fx for the function \[ {#random_function_f#}\]
    <!--</p-->
</p>
<div class="checkbox-container" style="float:left">
    <label><input type="radio" name="answer" class="checkbox-class" value="{#inputs_fx[1]#}">\[ {#inputs_fx[1]#}\]</label><br>
    <label><input type="radio" name="answer" class="checkbox-class" value="{#inputs_fx[2]#}">\[ {#inputs_fx[2]#}\]</label><br>
    <label><input type="radio" name="answer" class="checkbox-class" value="{#inputs_fx[3]#}">\[ {#inputs_fx[3]#}\]</label><br>
    <label><input type="radio" name="answer" class="checkbox-class" value="{#inputs_fx[4]#}">\[ {#inputs_fx[4]#}\]</label><br>
    <label><input type="radio" name="answer" class="checkbox-class" value="{#inputs_fx[5]#}" checked="">\[ None\: of\: the\: above\]</label><br>
</div>

[[jsxgraph height='850px' width='850px' input-ref-stateStore="stateRef"]]

const head = document.querySelector('head');
const style = document.createElement('style');
style.setAttribute('type', 'text/css');
style.innerHTML = `input[type="radio"] {
position: relative;
}

input[type="radio"]:before {
content: "";
position: absolute;
top: 0;
left: 0;
width: 20px;
height: 20px;
border-radius: 50%;
border: 2px solid grey;
background-color: white;
}

input[type="radio"]:after {
content: "";
position: absolute;
top: 5px;
left: 5px;
width: 10px;
height: 10px;
border-radius: 50%;
background-color: grey;
}

input[type="radio"]:checked:before {
border-color: red;
}

input[type="radio"]:checked:after {
background-color: red;
}
`;

head.appendChild(style);


var board = JXG.JSXGraph.initBoard(divid, {
boundingbox: [-8, 8, 8, -8],
keepaspectratio: false,
axis: false
});

var box = [-10,10];
var view = board.create('view3d', [[-6, -3], [8, 8],[box, box, box]],{ xPlaneRear: {visible: false}, yPlaneRear: {visible:false}});

view.D3.az_slide._smax = 12;

// Draw function

var F = board.jc.snippet('{#random_function_f#}', true, 'x,y', true); // JessieCode parsing

var fGraph =view.create('functiongraph3d', [
F,
box, // () =&gt; [-s.Value()*5, s.Value() * 5],
box, // () =&gt; [-s.Value()*5, s.Value() * 5],
], { strokeWidth: 0.5, stepsU: 70, stepsV: 70 });
board.update();

let inputs = document.getElementsByClassName("checkbox-class");
var state = {'x':2, 'y':2, 'z':-5, 'az_slide':0.87 , 'el_slide':1.5, 'selected':inputs[inputs.length-1].value};
var stateInput = document.getElementById(stateRef);
if (stateInput.value){
if(stateInput.value != '') {
state = JSON.parse(stateInput.value);
}
}

//the point that controlls the point on the graph;
var Axy = view.create('point3d', [2, 2, -5], { withLabel: false, color:'gray',strokeWidth:5 });

//the point reflected on the graph

var A = view.create('point3d',[ function() {return Axy.D3.X()}, function(){return Axy.D3.Y()},
function(){return F(Axy.D3.X(), Axy.D3.Y())}], { withLabel: false, color:'red' });


//set the input value equal to the default selected checkbox

const checkboxArray = document.querySelectorAll('.checkbox-class');
const checkboxArrayLength = checkboxArray.length;
var ans1n = document.getElementsByClassName('algebraic')[0].value=checkboxArray[checkboxArrayLength-1].value;
var cGraph="";
var funcExpr="";
function drawChecked(checkBoxValue){
funcExpr = checkBoxValue;

F = board.jc.snippet(funcExpr, true, 'x,y', true); // JessieCode parsing
cGraph =view.create('functiongraph3d', [
F,
box, // () =&gt; [-s.Value()*5, s.Value() * 5],
box, // () =&gt; [-s.Value()*5, s.Value() * 5],
], {strokeWidth: 0.5, stepsU: 70, stepsV: 70, color:'red' });
board.update();
}

//change the input value when the user selects a different checkbox

const checkboxContainers = document.querySelectorAll('.checkbox-container');
for (let container of checkboxContainers) {
container.addEventListener('change', setValue);
}
var firstEntry=true;
function setValue(event) {
const checkbox = event.target;
if (checkbox.classList.contains('checkbox-class')) {
var ans1n = document.getElementsByClassName('algebraic')[0].value=checkbox.value;
state.selected= checkbox.value;
stateInput.value = JSON.stringify(state);


//this is to connect the point to main function when the 'None of the above' checkbox is checked
//if its the last element (if user selects the 'None' option)
if({#inputs_fx[length(inputs_fx)]#}==checkbox.value) {

//remove everything
board.removeObject(cGraph,false);
board.removeObject(fGraph,false);
board.update();

// redraw main graph
F = board.jc.snippet('{#random_function_f#}', true, 'x,y', true); // JessieCode parsing

fGraph =view.create('functiongraph3d', [
F,
box, // () =&gt; [-s.Value()*5, s.Value() * 5],
box, // () =&gt; [-s.Value()*5, s.Value() * 5],
], { strokeWidth: 0.5, stepsU: 70, stepsV: 70 });
board.update();
} else{
board.removeObject(cGraph,false);
drawChecked(checkbox.value);
}
}

}


// check the previousley selected radio button
for (let i=0; !(i==inputs.length);i++) {
if(inputs[i].value == state.selected) {
inputs[i].checked = true;
//check if none is selected, then dont display it
if (!(inputs[i].value ==inputs[inputs.length-1].value) ) {
drawChecked(inputs[i].value);
}
}
}


view.D3.az_slide.setValue(state['az_slide']);
view.D3.el_slide.setValue(state['el_slide']);
board.update();

view.D3.az_slide.on('drag', function() {
var az_slide_value = view.D3.az_slide.Value();
state.az_slide = az_slide_value;
stateInput.value = JSON.stringify(state);
});

view.D3.el_slide.on('drag', function() {
var el_slide_value = view.D3.el_slide.Value();
state.el_slide = el_slide_value;
stateInput.value = JSON.stringify(state);
});


var t1 = board.create('text',[0,1,Axy.D3.X()]);


[[/jsxgraph]]

<p style="display:none">[[input:ans1]] [[validation:ans1]]</p>
<p>[[input:stateStore]] [[validation:stateStore]]</p>
}
[[/jsxgraph]]





<p style="display:none">[[input:ans1]] [[validation:ans1]]</p>
```
### Feedback variables

```rust
ta: random_function;
```
### Response tree
![image](https://user-images.githubusercontent.com/43517080/211091835-3b48dcea-7a60-4270-bb54-cdeb13df3fd9.png)
#### Node 1


