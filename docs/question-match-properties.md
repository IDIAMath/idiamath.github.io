---
title: Question - find a function with given properties
usemathjax: true
theme: minima
---
> Ask the user to enter a function defined by algebraic expression such that certain properties are in place. Use graphics to plot the surface with corresponding properties before it is evaluated formally.
>
> Property example: partial derivatives at all points with coordinates (t,t) are positive.
![image](https://user-images.githubusercontent.com/43517080/178961686-f936dea0-f8ac-48f4-b6e8-0f37f1a868ae.png)

## Question description

Random $$(x, y)$$ coordinates are generated, the user is asked 1 of 3 randomly generated questions: 
1. to provide a function that will result in positive output for all first derivatives of the given function at the generated coordinates.
2. to provide a function that will result in negative output for all first derivatives of the given function at the generated coordinates.
3. to provide a function that will result in unique number **SIGN** (0, -, 0 are unique signs)
The user may choose to draw the provided function by clicking on the 'Draw Function' button.

Although simple, this question demonstrates how STACK and JSXGraph can
be complementary. STACK works to provide the user with input feedback
in realtime. For example the user may type `xy` instead of `x*y`,
STACK will highlight this error. This is useful as it omits the need for string manipulation in JS, saves time.

+ [Moodle XML](questions-majd testing-Question 2 define function with certain properties-20220628-0956 (1).xml)

### Student perspective
The student will type in the function that fits the criteria of the random question (1 of the 3) generated for them by clicking the **Draw** button.
![image](https://user-images.githubusercontent.com/43517080/178962343-de23cd55-799c-4a47-a599-71e240d4f77b.png)


### Teacher perspective
The teacher's do not have to change anything unless they wish to change the randomly generated x and y coordinates for the question. Then they may change the a (x) and b (y) variables.
![image](https://user-images.githubusercontent.com/43517080/178966711-719fea4b-25c1-46e5-a0c6-6d1b9f908dba.png)
## Question CODE

### Question Variables

a and b variables are the x and y coordinates.

```maxima
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

```maxima
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




