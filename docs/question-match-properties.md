---
title: Question - find a function with given properties
usemathjax: true
theme: minima
---
> Ask the user to enter a function defined by algebraic expression such that certain properties are in place. Use graphics to plot the surface with corresponding properties before it is evaluated formally.
>
> Property example: partial derivatives at all points with coordinates (t,t) are positive.

| ![image](https://user-images.githubusercontent.com/43517080/178961686-f936dea0-f8ac-48f4-b6e8-0f37f1a868ae.png) |
|:--:|
| *First impression of the question* |

## Question description

Random $$(x, y)$$ coordinates are generated, the user is asked 1 of 3 randomly generated questions: 
1. To provide a function that will result in positive output for all first order partial/mixed derivatives (Fx,Fy and Fxy) of the given function at the generated coordinates.
2. To provide a function that will result in negative output for all first order partial/mixed derivatives (Fx,Fy and Fxy)  of the given function at the generated coordinates.
3. To provide a function where all first order partial derivative (Fx,Fy,Fxy) values at the generated coordinates provide unique signs from each other (+, -, 0/neutral).

The user may choose to draw the provided function by clicking on the **'Draw Function'** button.

Although simple, this question demonstrates how STACK and JSXGraph can
be complementary. STACK works to provide the user with input feedback
in realtime. For example the user may type `xy` instead of `x*y`,
STACK will highlight this error. This is useful as it omits the need for string manipulation in JS, saves time.

- [XML Code](XML/Question 2 define function with certain properties 22022.xml)

### Student perspective
The student will type in the function that fits the criteria of the random question (1 of the 3) generated for them by clicking the **Draw** button.

| ![image](https://user-images.githubusercontent.com/43517080/178962343-de23cd55-799c-4a47-a599-71e240d4f77b.png) |
|:--:|
| *When the student types in the function and clicks the **Draw** button* |


### Teacher perspective
The teacher's do not have to change anything unless they wish to change the range for the randomly generated x and y coordinates for the question. Then they may change the a (x) and b (y) variables inside the **Question variables** box.

| ![image](https://user-images.githubusercontent.com/43517080/191801405-a9083b67-b488-4c80-8fa2-1928e6b8aae5.png) |
|:--:|
| *The above image shows which values the teacher may wish to change (highlighted in yellow)* |

### Question's and answers examples
#### Question variant 1.
> "Give an exmaple of a function where all partial derivatives at the coordinates (2,1) are positive"
> 
> Answer: `y^2*x^2`
#### Question variant 2.
> "Give an exmaple of a function where all partial derivatives at the coordinates (2,1) are negative"
> 
> Answer: `-y^2*x^2`
#### Question variant 3.
>"Give an example of a function where all partial derivatives at the coordinates (2,-3) are "different in regards to the sign infront of them, example:` fx = -5 fy =-1 fxy = 0`. is not valid because -1 and -5 are both negative""
>
>Here if we can get Fx to be any positive number, then we have two options left for Fy and Fxy. The options are a negative number or 0; Fy can be a negative number, then Fxy has to be 0. Fy can be 0, then Fxy has to be a negative number. The point is the sign's for all partial derivative values have to be unique from each other.
>
>Answer: `x+y^2`
The answer is such because;
`Fx:1
Fy:-6
Fxy:0`

## Question CODE

### Question Variables

**a** and **b** variables are the x and y coordinates.

The **rand_question generates** a random value between 0-3, that value is further utilized in the **question_text** variable.

The question_text variable checks which random value was generated and displays the message that coreesponds to the question value.
```rust
a:rand_with_prohib(-5,5,[0]);
b:rand_with_prohib(-5,5,[0]);
rand_question:rand_with_prohib(0,3,[0]);
question_text:if (rand_question = 1) then positive else if (rand_question = 2) then negative else "different in regards to the sign infront of them, example: fx = -5 fy =-1 fxy = 0.  is not valid because -1 and -5 are both negative";
```

### Question Text
The code is divided into segments, each of which is explained
- 1 Segment we create a **button**, give it atrributes.

- 2 Segment we append the button to the page in the appropriate location which is in a div that has class name **'Clearfix'**.

- 3 Segment we create one eventlistener that is attached to the button that was created. Everytime we click the button then the function called **"myFunction"** will run.

- 4 Segment this is default code for creating the 3D room and the plane with x,y and z axis.

- 5 Segment the function myFunction is created. It recieves the input from an input element in the DOM which has the class name **"algebraic"**, then we remove any previously drawn graphs/3d functions and draw a new graph/3d function based on the recieved user input.

```javascript
<p></p>
<p onclick="myFunction()">Give an example of a function where all partial derivatives at the
  coordinates ({#a#},{#b#}) are {#question_text#} <br></p>

[[jsxgraph height='850px' width='850px']]

//SEGMENT 1 _________elements created_______________
let btn = document.createElement("button");
btn.innerHTML = "Draw Function";
btn.setAttribute("type","button");
let br = document.createElement("br");

//SEGMENT 2 _________apend to document_______________
document.getElementsByClassName("clearfix")[0].appendChild(br);
document.getElementsByClassName("clearfix")[0].appendChild(br);
document.getElementsByClassName("clearfix")[0].appendChild(btn);

//SEGMENT 3 _________Add eventlisteners_______________
btn.addEventListener("click", myFunction);

//SEGMENT 4 _________Create the 3D room and the plane with x,y and z axis._______________
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


//SEGMENT 5 _________ Create function that gets function expression and draws graph_______________
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
We retrieve the student answer (function expression) and store it in the variable **f**. 

We then find the partial derivatives of the provided expression and create a **score** variable set it to 0.

The provided function is evaluated at the randomly generate x and y values (which we named **'a'** and **'b'* in this case)

Variable **question_procedure** checks which of the 3 random questions that has been picked and further. we further increase the score if all three conditions of that question are met.

The **ta** variable check wether the score matches the required score to pass the selected question, if it is, then its set to the student retrived input because its the correct answer. It bascally checks if the answer is correct and stores it.


```rust
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
### Partial response tree

| ![image](https://user-images.githubusercontent.com/43517080/191792093-1f2181ef-cad3-412b-b566-48303d98a658.png) |
|:--:|
| *Values of **node 1*** |

#### Node 1
The answer test is set to AlgEquiv which checks if the user input algebriac expression is equivelant to the required expression to pass the question criteria.
We also display the question text when the student answer is incorrect `{#rand_question#}`


