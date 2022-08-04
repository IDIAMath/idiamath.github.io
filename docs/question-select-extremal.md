---
title: STACK Question 3.  Select maxima/minma on a plot
usemathjax: true
theme: minima
---

# STACK Question 3.  Select maxima/minma on a plot

> Given a a surface defined by $$z=f(x,y)$$, where exact expression for $$f$$
> is unknown to a user, ask the user to select all local maxima/minima on the
> surface plot.

- [XML Code](XML/Question 1 part 2 local minmax (Jonas edit) -2022027.xml)

## What the student sees

| ![capture](https://user-images.githubusercontent.com/43517080/181220416-58f6716e-236e-41e1-8c63-632be8a72b93.PNG) |
|:-:|
| *This image shows the students first view of the question* |


### What the student has to do


The function may have any amount of local extremal points,
The 2D figure shows one red point for each minima, and one blue point for each maxima.
The student has to move them to the correct co-ordinate position to identify extramal
points on the surface in the 3D plot.

Depending on the function, the student will need to manipulate the 3D-figure extensively to identify the extremal values.
The sliders control the cameras azimuthal angle (az), elevation (el) and scales the z-axis.
In addition the controls in the lower right hand corner lets the student move the camera position and zoom in/out.

| ![controll](https://user-images.githubusercontent.com/43517080/181227525-8b141065-1284-443d-9b36-85d12b8c490e.PNG) |
|:-:|
| *The above image shows the 2D plane where the student can move the points to match approximately with the local min/max* |

Grading allows for a limited error, and the teacher should set a tolerance appropriate to the resolution on the x/y-axes.

## Question code

### Question variables

The following maxima code should be entered into the Question Variables field.

**Caveats**

1.  It does not use random variables.  The function `f` has to be hand-coded.
2.  The x, y, and z ranges (`xrang`, `yrang`, `zrang`) have to be hand-coded.
3.  The tolerance (`maxError`) is absolute (as opposed to relative) and have to be set to a reasonable value

```
xrang:[-2,5];  
yrang:[-2,4];
zrang:[-1,10];

maxError:0.3; /*Tolerance (absolute) */

/*Easy example*/
f:(2*x^4+2*y^4-8*x*y+12);

/*Hard example*/
/*f:expand((x-3)*(x+1)*(y-2)*(y+2)*(x*y+3));*/

/*Partial derivatives*/
fx:diff(f,x);
fy:diff(f,y);
fxx:diff(fx,x);
fyy:diff(fy,y);
fxy:diff(fy,x);
D: fxx*fyy-(fxy)^2;

/*Solve for critical points*/
critical:solve([fx,fy],[x,y]);

/*Function noDegen takes a list 'l' of solutions [ [x=a, y=b], ...] and returns a list of solutions not containing 'r'*/
noDegen(l,r):=sublist(l, lambda([ll], freeof(r,ll)));
/*Remove degenerate solutions (solutions with free variables stored in %rnum_list)*/
critical_nd: lreduce(noDegen,  %rnum_list,     critical);

/*Function isNotImag returns true if a solution sol ([x=a,y=b]) has no imaginary part*/
isNotImag(sol):=is(equal(imagpart(sol),[0=0,0=0]));
/*Remove complex solutions*/
critical_nd_real:sublist(critical_nd, isNotImag);

/*Classify critical points and collect local max/min */
localmin_expr:sublist(critical_nd_real, lambda([p], ev(fxx,p) >0 and ev(D,p)>0));
localmax_expr:sublist(critical_nd_real, lambda([p], ev(fxx,p) < 0 and ev(D,p)>0));

/*Store min/max in [a,b] form, instead of [x=a, y=b] form*/
localmin:float(map(lambda([x], map(rhs, x)),localmin_expr));
localmax:float(map(lambda([x], map(rhs, x)), localmax_expr));
```

Some lines should be changed by the teacher:

1.  The function `f` to be plotted.
2.  The error tolerance `maxError` which defines how accurately the student
    has to answer.
3.  The arrays `xrang` (`[x_min,x_max]`), `yrang` and `zrang` which sets axis ranges for the plots

### Question text

```
<p><span style="font-size: 0.9375rem;">Given a surface defined by z=f(x,y), where exact expression for f is unknown. Determine local min and max</span><br></p>
<p style="display:none">[[input:ans1]] [[validation:ans1]]</p>
<p style="display:none">[[input:ans2]][[validation:ans2]]</p>
[[jsxgraph height='500px' width='700px' input-ref-ans1="points_min_out" input-ref-ans2="points_max_out"]]```

   var divid2="stack-jsxgraph-2"; //div id for the bottom figure

   // Ranges for x,y,z from maxima
   var xrang = {#xrang#};
   var yrang = {#yrang#};
   var zrang = {#zrang#};

   //Create 3D-board
   // Boundingbox sets the vertical and horizontal units and the position of the origin of the figure
   // For instance a boundingbox [-5,10,5,-10] has upper left corner at (-5,10) and lower right corner at (5,-10)
   var board = JXG.JSXGraph.initBoard(divid, {
   boundingbox: [xrang[0]-4,yrang[1]+4,xrang[1]+4,yrang[0]-4],
   keepaspectratio: false,
   axis: false
   });

   //Create the 2D board
   var board2 = JXG.JSXGraph.initBoard(divid2, {boundingbox: [xrang[0],yrang[1],xrang[1],yrang[0]], axis: true});

   //Add the 3D board as a child of the 2D board, such that the 3D figure updates when we manipulate the 2D figure
   board2.addChild(board);

   //Arrays holding the min/max points in the 2D figure
   var p1min=[];
   var p1max=[];

   //Arrays holding the min/max points in the base of the 3D figure (xy-plane)
   var p2min=[];
   var p2max=[];

   //Arrays holding the correct max/min points
   var localmin = {#localmin#};
   var localmax = {#localmax#};
  
   // Create the 3D view: -------
   /*The large array is of form [ [x,y], [w,h], [[x1,x2], [y1,y2], [z1,z2]] ]. 
   The arrays [x, y] and [w, h] define the 2D frame into which the 3D cube is (roughly) projected. 
   [[x1, x2], [y1, y2], [z1, z2]] determines the coordinate ranges of the 3D cube. */

   var view = board.create('view3d', [[xrang[0], yrang[0]], [xrang[1]-xrang[0],yrang[1]-yrang[0]],[xrang, yrang, zrang]],{ xPlaneRear: {visible: false}, yPlaneRear: {visible:false}});

   //-----------

   //Create a slider that scales the z-values
   var z_scale = board.create('slider', [[-3,-5], [3,-5],[0,0.2,2]], {
      name: "Skaler z-akse",
      point1: {frozen: true},
      point2: {frozen: true}
   });

   var FF = board.jc.snippet('{#f#}', true, 'x,y', true); //Jessiecode parsing of function from maxima

   var F = (x,y)=>z_scale.Value()*FF(x,y); //Scale function value by multiplying with z_scale slider value

   //Create the surface plot
   var c = view.create('functiongraph3d',[F,xrang,yrang], { strokeWidth: 0.5, stepsU: 70, stepsV: 70 });


   // Create points for every minimum the students musts find
   for (let i=0; i<localmin.length; i++){
      p1min[i] = board2.create('point', [i,i], {name: "Min"+(i+1), color: "red"}); //Point in 2D plane
  
      //Create the corresponding point in the bottom of the 3D plot - values bound to the 2D plot
      p2min[i] = view.create('point3d', [()=>p1min[i].X(), ()=>p1min[i].Y(), zrang[0]], {name: "Min"+(i+1), color: "red"});

      //Create corresponding point on the surface plot - bound by the function value, and (x,y) of the 2D point
      var ptemp = view.create('point3d', [()=>p1min[i].X(), ()=>p1min[i].Y(), ()=>F(p1min[i].X(), p1min[i].Y())], {withLabel: false});

      //Lastly, create a dashed line from the base point to the point on the surface plot
      view.create('line3d', [p2min[i], ptemp], {dash: 1});
   }

   //Same procedure for the maximal values
   for (let i=0; i<localmax.length; i++){
      p1max[i] = board2.create('point', [i,i], {name: "Max"+(i+1), color: "blue"});
      p2max[i] = view.create('point3d', [()=>p1max[i].X(), ()=>p1max[i].Y(), zrang[0]], {name: "Max"+(i+1), color: "blue"});
      var ptemp = view.create('point3d', [()=>p1max[i].X(), ()=>p1max[i].Y(), ()=>F(p1max[i].X(), p1max[i].Y())], {withLabel: false});
      view.create('line3d', [p2max[i], ptemp], {dash: 1});
   }

   board.update();

   //Obtain references to the answer variables in stack
   var answer_min = document.getElementById(points_min_out);
   var answer_max=document.getElementById(points_max_out);

   //Set an update function that sets the answer variables in stack to the points the student has selected
   board.on('update', function() {

      //Arrays of min/max points
      var pout =  [];
      var pout2 = [];

      //Collect minimum points
      for (let i=0; i<p1min.length; i++){
       pout[i] = [p1min[i].X(), p1min[i].Y()];
      }
      //Set answer variable in stack to the current points selected by students in jsxgraph
      answer_min.value=JSON.stringify(pout);

     //Same procedure for max points
     for(let i=0; i<p1max.length; i++){
        pout2[i] = [p1max[i].X(), p1max[i].Y()];
     }
     answer_max.value=JSON.stringify(pout2); 
   });
[[/jsxgraph]]

[[jsxgraph width="500px" height="300px"]]
[[/jsxgraph]]
```
### Specific feedback

We define two partial response trees for this question.

```html
[[feedback:prt1]]
[[feedback:prt2]]
```

### Feedback Variables 

#### Feedback variabels PRT1

```
lenlocalmin:length(localmin);
lenans1:length(ans1);

dist(u,v):=float(sqrt((u[1]-v[1])^2+(u[2]-v[2])^2));
dists(a,b):=map(lambda([x],dist(a,x)), b);
errors:map(lambda([x],dists(x,localmin)),ans1);
errors_best:map(lmin,errors);
find_ind(e, errs):=first(sublist_indices(errs, lambda([x], x=e)));
indices:map(lambda([x], find_ind(errors_best[x], errors[x])), makelist(i,i,lenlocalmin));
zip(a,b):=map(lambda([x], [a[x],b[x]]), makelist(i,i,length(a)));
error_indices:zip(errors_best,indices);
errors_OK:sublist(error_indices, lambda([x], x[1]<maxError));
indices_OK:map(lambda([x], x[2]), errors_OK);
correct_mins:length(unique(indices_OK));
```

#### Feedback variabels PRT2

```
lenlocalmax:length(localmax);
lenans2:length(ans2);

dist(u,v):=float(sqrt((u[1]-v[1])^2+(u[2]-v[2])^2));
dists(a,b):=map(lambda([x],dist(a,x)), b);
errors2:map(lambda([x],dists(x,localmax)),ans2);
errors2_best:map(lmin,errors2);
find_ind(e, errs):=first(sublist_indices(errs, lambda([x], x=e)));
indices2:map(lambda([x], find_ind(errors2_best[x], errors2[x])), makelist(i,i,lenlocalmax));
zip(a,b):=map(lambda([x], [a[x],b[x]]), makelist(i,i,length(a)));
error2_indices:zip(errors2_best,indices2);
errors2_OK:sublist(error2_indices, lambda([x],x[1]<maxError));
indices2_OK:map(lambda([x],x[2]), errors2_OK);
correct_max:length(unique(indices2_OK));
```

### Partial Response Trees
