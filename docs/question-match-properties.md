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
- When user submits/checks answer, they are provided with graphs of Fx, Fy, and Fxy (partial derivatives). The graphs will be highlighted in green or red depending on the result of the answer.


## Question CODE

### Question Variables
a and b variables are the x and y coordinates.
```html
a:rand_with_prohib(-5,5,[0]);
b:rand_with_prohib(-5,5,[0]);
rand_question:rand_with_prohib(0,3,[0]);
question_text:if (rand_question = 1) then positive else if (rand_question = 2) then negative else "different in regards to the sign infront of them, example: fx = -5 fy =-1 fxy = 0.  is not valid because -1 and -5 are both negative";
```

### Question Text
```html
<p></p>
<p onclick="myFunction()">Give an example of a function where all partial derivatives at the coordinates ({#a#},{#b#}) are {#question_text#} <br></p>

[[jsxgraph height='850px' width='850px']]

//elements created
let btn = document.createElement("button");
btn.innerHTML = "Draw Function";
btn.setAttribute("type","button");
let br = document.createElement("br");

//set attributes


//append to document
document.getElementsByClassName("clearfix")[0].appendChild(br);
document.getElementsByClassName("clearfix")[0].appendChild(br);
document.getElementsByClassName("clearfix")[0].appendChild(btn);


btn.addEventListener("click", myFunction);

var board = JXG.JSXGraph.initBoard(divid, {
boundingbox: [-8, 8, 8, -8],
keepaspectratio: false,
axis: false
});


var box = [-5, 5];
var view = board.create('view3d',
[
[-6, -3], [8, 8],
[box, box, box]
],
{
xPlaneRear: {visible: false},
yPlaneRear: {visible: false},
});
view.D3.az_slide._smax = 12;

var funcExpr = '';

var F = board.jc.snippet(funcExpr, true, 'x,y', true); // JessieCode parsing
// 3D surface

var fGraph = view.create('functiongraph3d', [
F,
box, // () =&gt; [-s.Value()*5, s.Value() * 5],
box, // () =&gt; [-s.Value()*5, s.Value() * 5],
], { strokeWidth: 0.5, stepsU: 70, stepsV: 70 });

function myFunction() {
var ans1n = document.getElementsByClassName('algebraic')[0];

funcExpr = ans1n.value;

board.removeObject(fGraph,false);

F = board.jc.snippet(funcExpr, true, 'x,y', true); // JessieCode parsing
fGraph =view.create('functiongraph3d', [
F,
box, // () =&gt; [-s.Value()*5, s.Value() * 5],
box, // () =&gt; [-s.Value()*5, s.Value() * 5],
], { strokeWidth: 0.5, stepsU: 70, stepsV: 70 });
board.update();
}

[[/jsxgraph]]
<p></p>
<p><span>Your function = [[input:ans1]][[validation:ans1]] </span></p>
```

### Feedback variables
```html
f:ans1;
fx:diff(f,x);
fy:diff(f,y);
fxy:diff(fy,x);
score:0;
evfx:ev(fx,x=a,y=b);
evfy:ev(fy,x=a,y=b);
evfxy:ev(fxy,x=a,y=b);

question_procedure: if (rand_question =1) then (sa1:if evfx >0 then score:score+1, sa2:if evfy > 0 then score:score+1, sa3:if evfxy > 0 then score:score+1) else if (rand_question = 2) then (sa1:if evfx <0 then score:score+1, sa2:if evfy < 0 then score:score+1, sa3:if evfxy < 0 then score:score+1) 
/* the different combination check */
else (sa1:if evfx <0 then score:score+1 else if evfx>0 then score:score + 3 else score:score+6 , sa2:if evfy < 0 then score:score+1 else if evfy>0 then score:score + 3 else score:score+6 , sa3:if evfxy < 0 then score:score+1  else if evfxy >0 then score:score + 3 else score:score+6 );

ta: if (score =3 and (rand_question = 1 or rand_question = 2))then ans1 else if (score = 10 and rand_question = 3 ) then ans1;

```




