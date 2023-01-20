---
title: STACK Question - Select matching function
usemathjax: true
theme: minima
---

## Question Interpretation

> Given a function drawn on the 3D room, select the matching function expression

| ![image](https://user-images.githubusercontent.com/43517080/211090094-c650cfde-c43b-4fce-b678-20c316ee8931.png) |
|:--:|
| *First impression of the question* |

### Question description
User has to select function expression that matches the one drawn on the graph. They have a point connected to the graph which they can manipulate to find the answer


xml code 
- [XML Code](XML/question-select-matching-function.xml) 


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

```rust
<p style="font-size:1.3em">Select the function expression matching the function on the graph</p>
<div class="checkbox-container" style="float:left">
    <label><input type="radio" name="answer" class="checkbox-class" value="{#inputs[1]#}" id="A">\[ {#inputs[1]#}\]</label><br>
    <label><input type="radio" name="answer" class="checkbox-class" value="{#inputs[2]#}" id="B">\[ {#inputs[2]#}\]</label><br>
    <label><input type="radio" name="answer" class="checkbox-class" value="{#inputs[3]#}" id="C">\[ {#inputs[3]#}\]</label><br>
    <label><input type="radio" name="answer" class="checkbox-class" value="{#inputs[4]#}" id="C">\[ {#inputs[4]#}\]</label><br>
    <label><input type="radio" name="answer" class="checkbox-class" value="{#inputs[5]#}" id="C" checked="checked">\[ None\: of\: the\: above\]</label><br>
</div>

<p style="display:none">[[input:ans1]] [[validation:ans1]]</p>
<p>[[input:stateStore]] [[validation:stateStore]]</p>

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
border-color: blue;
}

input[type="radio"]:checked:after {
background-color: blue;
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

var F = board.jc.snippet('{#random_function#}', true, 'x,y', true); // JessieCode parsing

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

// check the previousley selected radio button
for (let i=0; !(i==inputs.length);i++) {
if(inputs[i].value == state.selected) {
inputs[i].checked = true;
}
}

//the point that controlls the point on the graph;
var Axy = view.create('point3d', [state['x'], state['y'], state['z']], { withLabel: false, color:'gray',strokeWidth:5 });

// Update the stored state when the position of the point Axy changes.
Axy.on('drag', function() {
state.x = Axy.X();
state.y = Axy.Y();
stateInput.value = JSON.stringify(state);
});

//the point reflected on the graph

var A = view.create('point3d',[ function() {return Axy.D3.X()}, function(){return Axy.D3.Y()},
function(){return F(Axy.D3.X(), Axy.D3.Y())}], { withLabel: false, color:'red' });

//optimize the el and az scale
view.D3.az_slide.setValue(state['az_slide']);
view.D3.el_slide.setValue(state['el_slide']);
board.update();

//store the value fo the az and el scale when user adjusts it
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



//set the input value equal to the default selected checkbox

const checkboxArray = document.querySelectorAll('.checkbox-class');
const checkboxArrayLength = checkboxArray.length;
var ans1n = document.getElementsByClassName('algebraic')[0].value=checkboxArray[checkboxArrayLength-1].value;


//change the input value when the user selects a different checkbox

const checkboxContainer = document.querySelector('.checkbox-container');
checkboxContainer.addEventListener('change', setValue);
function setValue(event) {
const checkbox = event.target;
if (checkbox.classList.contains('checkbox-class')) {
var ans1n = document.getElementsByClassName('algebraic')[0].value=checkbox.value;
state.selected = checkbox.value;
stateInput.value = JSON.stringify(state);

}
}
[[/jsxgraph]]
```
### Feedback variables

```rust
ta: random_function;
```
### Response tree
![image](https://user-images.githubusercontent.com/43517080/211091835-3b48dcea-7a60-4270-bb54-cdeb13df3fd9.png)
#### Node 1


