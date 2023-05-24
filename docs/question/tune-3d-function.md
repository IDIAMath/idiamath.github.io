---
title: Question - Tune 3D Function
usemathjax: true
---

# Question

![Screenshot - Tune 3D](tune-3D.png)

The student is given a plot of a function $$f(x,y)$$ containing one or
more unknown parameters, say $$a,b,c$$. 

The student sees two superimposed surface plots.
One with the correct values for the unkown parameters, and another where the
value of the parameter, or parameters, 
can be controlled by one or more sliders.

The student will have to use the slider to find the value for the parameters
that makes the two functions coincide.

- [XML Code](XML/tune-3d-function-xml)

# How it works

## Question Variables (Maxima)

```maxima
/*Non-randomized small example*/

func: a*x^2+b*x*y+c*y^2;
all_params: [a,b,c];
coef: [1,2,3];
params: [b];
param_sliders: [-4,0,4];

/*Randomized big example*/
/*
func: a*x^2+b*y^2+c*x*y+d*x+f*y+g;
all_params: [a,b,c,d,f,g];
coef: random_permutation(append([0,0],makelist(rand_with_prohib(-4,4,[0]),4)));
params: rand_selection(all_params,2);
param_sliders: [-4,0,4];
*/

/*x- and y- ranges */
xrang:[-7,7];
yrang:[-7,7];

/* Max error for tunable parameters */
maxError:0.2;

/*List of non-tunable parameters*/
non_params: sublist(all_params, lambda([x], not member(x, params)));

/*Stack-map, example: ["stack-map",
   ["a", 1], ["b", 2], ["pnames", ["a","b"]], ["sliders, [-4,0,4]]
]*/
params_json: append(["stack_map"],
   makelist([string(all_params[i]),coef[i]], i, length(all_params)),
   [["pnames", map(string,params)]], [["sliders", param_sliders]]
);

/*Parameter equation lists, example [a=1, b=2, c=3]*/
all_params_eq: makelist(all_params[i] = coef[i], i, length(all_params));
params_eq: sublist(all_params_eq, lambda ([x], member(lhs(x), params)));
non_params_eq: sublist(all_params_eq, lambda([x], member(lhs(x), non_params)));

/*Function evaluated with un-tunable parameters*/
fxy:ev(func,non_params_eq);

/*Function evaluated with all parameters given*/
fxy2:ev(func,all_params_eq);
```

## Question Text

First we create three *hidden* input boxes which will
be manipulated from javascript only.

```html
<p style="display:none">[[input:ansa]] [[validation:ansa]]</p>
<p style="display:none">[[input:ansb]] [[validation:ansb]]</p>
<p style="display:none">[[input:ansc]] [[validation:ansc]]</p>
```

The question text proper is straight forward.

```html
Gitt uttrykket \(f(x,y) = {@fxy@}\), hvor parameteren \({@simplode(params,",")@}\) er ukjent, bruk figuren til å finne den ukjente parameteren ved å stille på slideren slik at grafene blir like.
```

The critical part is the javascript code, in `[[jsxgraph]]` tags.

```javascript
[[jsxgraph input-ref-ans="ansRef"]]
(function () {

   //Get divid for bottom jsxgraph figure by 
   //incrementing number in divid of top figure
   //
   //Regex that matches number between 1-99
   var regexMatchNumber = /([1-9][0-9]|[1-9])/g; 
   var nextDivNum = Number(divid.match(regexMatchNumber))+1;//Match+increment
   var divid2 = divid.replace(regexMatchNumber, nextDivNum);//Replace new number
   
   //Reference to the bottm jsxgraph figure
   var jsxgraph2 = document.getElementById(divid2);
   jsxgraph2.style.display="none";//Hide second fiture by default


   //{#stackjson_stringify(params_json)#} inserts JSON string from maxima
   //JSON.parse() returns a JSON object of the data from maxima
   var stack_input = JSON.parse({#stackjson_stringify(params_json)#});
   var ans = document.getElementById(ansRef);//Reference to answer field
   ans.value= JSON.stringify(stack_input); //Write JSON object to ans as string

   // Create the board
   var board = JXG.JSXGraph.initBoard(divid, {
      boundingbox: [-11 ,11, 11, -11],
      keepaspectratio: false,
      axis: false
   });


   //Create the 3D view 
   var box = [-2, 2];
   var boxx = {#xrang#};
   var boxy = {#yrang#};
   var view = board.create('view3d', [[-6, -3], [8, 8], [boxx, boxy, box]], {
      xPlaneRear: {visible: false},
      yPlaneRear: {visible: false},
   });

   //Array of strings containing the names of the tunable parameters 
   //Example: ["a", "c",...]
   var pnames = stack_input["pnames"];

   //Start, initial and stop value of sliders
   var slider_size = stack_input["sliders"];

   var sliders = []; //Array holding a reference to sliders
   
   //For several sliders, we need to shift the y-position so they don't overlap
   var slider_shift = 0 

   // Iterate over the the names of the tunable parameters
   for (var i=0; i<pnames.length; i++){

      //Set the initial tunable parameter value to startvalue of sliders
      stack_input[pnames[i]] = slider_size[1];
      //Write new parameter values to answer field
      ans.value = JSON.stringify(stack_input);

      
      //Create and push new slider to list of sliders
      sliders.push(board.create('slider',  
         [[-7, -6-slider_shift], [5, -6-slider_shift], slider_size], 
         { name: pnames[i] }//New slider has same name as the parameter
      ));
      slider_shift++; //Shift y-position for next slider

      board.update();
   }

   //Create the scaleing slider for the function plot
   var scale_slider = board.create('slider', 
      [[-7,-6-slider_shift],[5,-6-slider_shift], [0,1,2]], 
      {name: "Scale z-axis"}
   );

   // JessieCode parsing of maxima function "fxy" - tunable
   var ff = board.jc.snippet('{#fxy#}', true, 'x,y', true);  
   
   // The function f calls the tunable function ff, andj scales it 
   // by mulitplying with z-axis slider 
   var f = (x,y) => scale_slider.Value()*ff(x,y);

   //Create the 3D tunable surface
   view.create('functiongraph3d', [f, boxx, boxy], {
      strokeColor: JXG.palette.blue,
      stepsU: 50, stepsV: 50, strokeWidth: 0.5
   });


   //JessieCode parsing of maxima function "fxy2"- non-tunable
   var gg = board.jc.snippet('{#fxy2#}', true, 'x,y', true); 
   // g(x,y) calls gg(x,y) but scales it with the slider
   var g = (x,y) => scale_slider.Value()*gg(x,y);

   //Create the 3D non-tunable function
   var func_g = view.create('functiongraph3d', [g, boxx, boxy], {
      strokeColor: JXG.palette.red,
      stepsU: 50, stepsV: 50, strokeWidth: 0.5
   });

   //Create a second board for the jsxgraph figure below (divid2)
   var board2 = JXG.JSXGraph.initBoard(divid2, {
      boundingbox: [-8, 8, 8, -8],
      keepaspectratio: false,
      axis: false
   });

   //Add board2 as a child to board - the bottom figure updates automatically
   //when the student manipulates the top figure
   board.addChild(board2);

   //Create 3D view for botoom figure
   var view2 = board2.create('view3d', 
      [[-6, -3], [8, 8], [boxx, boxy, box]], {
      xPlaneRear: {visible: false},
      yPlaneRear: {visible: false},
   });
   
   //Create 3D non-tunable surface in bottom figure
   view2.create('functiongraph3d', [g, boxx, boxy], {
      strokeColor: JXG.palette.red,
      stepsU: 50, stepsV: 50, strokeWidth: 0.5
   });
   board2.update();

   //We want to control rotation and elevation of bottom figure
   //with the controls in the top figure
   //Hide the controls in bottom figure
   view2.D3.az_slide.hide();
   view2.D3.el_slide.hide();
   //Set the rotation and elevation of bottom figure to 
   //that of the first figure
   view2.D3.az_slide = view.D3.az_slide;
   view2.D3.el_slide = view.D3.el_slide;

   //Rename elevation and rotation control sliders
   view.D3.el_slide.name = "elevation";
   view.D3.az_slide.name="rotate";
   board.update();

   // Set event listener for button that allows student 
   // to split the tunable and non-tunable surfaces into different figures
   //
   // Get reference to clickable button
   var button = document.getElementById('split-button');
   //Add the function to be called when the button is clicked
   button.addEventListener('click', function() { 
      
      // If value=0 we hide the goal-plot and show the bottom figure
      if (button.value == "0"){
         func_g.hide();                     //Hide target function 
         button.value = "1";                //Toggle button value
         button.textContent = "Merge";      //Toggle button text
         jsxgraph2.style.display = "block"; //Show bottom figure
      }
      else { //If value != 0 we show the goal-plot and hide the bottom figure
         func_g.show();                    //Show target function
         button.value="0";                 //Toggle button value
         button.textContent = "Split";     //Toggle button text
         jsxgraph2.style.display = "none"; //Hide bottom figure
      }
   });

   //Add a function that updates the answer variable going out to STACK
   //everytime the board changes
   board.on('update', function(){
      for (let i=0; i<sliders.length; i++) //Iterate over tunable sliders
      {
         var ans_name = sliders[i].getAttribute("name"); //Get name of slider
         stack_input[ans_name] = sliders[i].Value();  //Update value of parameter
      }
      //Write JSON object with updated parameter values to stack answer field
      ans.value=JSON.stringify(stack_input);
   });

})();	
[[/jsxgraph]]
```

An additional feature allows the student to split the view, seeing
each function in different surface plots.

```html
<button type="button" value="0" id="split-button"> Split </button>
```
