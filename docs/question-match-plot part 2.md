---
title: STACK Question - Function to match given properties part 2
usemathjax: true
theme: minima
---

# Read Some Properties From A Graph

I split the following question into two problems for now
due to a problem with creating 3 PRT's,
although i could create 2 PRT's as you can tell in part 2.

## Question Interpretation

> Given a a surface defined by $$z=f(x,y)$$, where exact expression for $$f$$ is unknown to a user, ask the user to select a point on it where partial derivatives are positive/negative/zero. Given the same, ask the user to select local maxima/minima.
I interpret this as: find a point on the functions where one of the three derivatives gives a positive number, another gives a negative and the third gives 0. For example, you could have a point that gives Fxy=1, Fx = 0, Fy=-1, that would be a solution.

It can also be interpreted as: select a point on the function where ALL partial derivatives would give a negative positive or 0. If so, this would be very similar to the provided solution and it would be an easy task change the solution to accommodate the following interpretation.


## Part 2
| ![capture](https://user-images.githubusercontent.com/43517080/181220416-58f6716e-236e-41e1-8c63-632be8a72b93.PNG) |
|:-:|
| *This image shows the students first view of the question* |
- [XML Code](XML/Question 1 part 2 local minmax (Jonas edit) -2022027.xml)

## Quesion code


### Quesion variables

```html
xrang:[-2,2];
yrang:[-2,2];
zrang:[-1,3];
maxError:0.3;
f:2*x^4+2*y^4-8*x*y+12;
fx:diff(f,x);
fy:diff(f,y);
fxx:diff(fx,x);
fyy:diff(fy,y);
fxy:diff(fy,x);
D: fxx*fyy-(fxy)^2;
/*pushing all valid zero points into a seperate array*/
zp:solve([fx,fy],[x,y]);
zpl:length(zp);
n:0;
zpclean:[];
zpdvalues:[];
zpfxxvalues:[];

/* a while loop that checks if a zp element contains imaginary number-> omits it, checks if it contains %r -> omits it. Further evaluates fxx values at valid zp points and pushes them to a list. Further removes the x and y variables along their = sign and pushes to zpclean. */
while (n<zpl) do (n:n+1, if (freeof(%i,zp[n]) = true) and (imagpart(zp[n][1]) = (0 = 0)) and (imagpart(zp[n][2]) = (0 = 0)) then (push(ev(D,zp[n][1],zp[n][2]),zpdvalues),push(ev(fxx,zp[n][1],zp[n][2]),zpfxxvalues),push([rhs(zp[n][1]),rhs(zp[n][2])],zpclean)) );

/* classify local min and max */
localmin:[];
localmax:[];
z:0;
/*push the clean localmin and localmax values in the form [x,y]*/
while (z< length(zpdvalues)) do (z:z+1, if (zpdvalues[z])> 0 and zpfxxvalues[z]>0 then (push(zpclean[z],localmin)) else if (zpdvalues[z] > 0 and zpfxxvalues[z]<0) then push(zpclean[z],localmax));
```


### Quesion text

```html
<p><span style="font-size: 0.9375rem;">Given a surface defined by z=f(x,y), where exact expression for f is unknown. Determine local min and max</span><br></p>
<p style="display:none">[[input:ans1]] [[validation:ans1]]</p>
<p style="display:none">[[input:ans2]][[validation:ans2]]</p>
[[jsxgraph height='500px' width='700px' input-ref-ans1="points_min_out" input-ref-ans2="points_max_out"]]

var divid2="stack-jsxgraph-2";
var xrang = {#xrang#};
var yrang = {#yrang#};
var zrang = {#zrang#};

var board = JXG.JSXGraph.initBoard(divid, {
boundingbox: [xrang[0]-4,yrang[1]+4,xrang[1]+4,yrang[0]-4],
keepaspectratio: false,
axis: false
});
var board2 = JXG.JSXGraph.initBoard(divid2, {boundingbox: [xrang[0],yrang[1],xrang[1],yrang[0]], axis: true});
board2.addChild(board);
var p1min=[];
var p1max=[];
var p2min=[];
var p2max=[];
var p3=[];
var localmin = {#localmin#};
var localmax = {#localmax#};

/* how much of the graph is visible, you will be able to change the x and y position of the point in accordance to the range provided in the box variable. the answer does not have to be confined to this, yuo can give[15,-9] as the answer for the question. you can also change it to whichever part of the graph you wish to be visible*/

var box = [-10,10];
var view = board.create('view3d', [[xrang[0], yrang[0]], [xrang[1]-xrang[0],yrang[1]-yrang[0]],[xrang, yrang, zrang]],{ xPlaneRear: {visible: false}, yPlaneRear: {visible:false}});

view.D3.az_slide._smax = 12;
var z_scale = board.create('slider', [[-3,-5], [3,-5],[0,0.2,2]], {name: "Skaler z-akse"});


var FF = board.jc.snippet('{#f#}', true, 'x,y', true);
var F = (x,y)=&gt;z_scale.Value()*FF(x,y);
var c = view.create('functiongraph3d',[F,xrang,yrang], { strokeWidth: 0.5, stepsU: 70, stepsV: 70 });

for (let i=0; i<localmin.length; i++){
   p1min[i] = board2.create('point', [i,i], {name: "Min"+(i+1), color: "red"});
   p2min[i] = view.create('point3d', [()=>p1min[i].X(), ()=>p1min[i].Y(), zrang[0]], {name: "Min"+(i+1), color: "red"});
   var ptemp = view.create('point3d', [()=>p1min[i].X(), ()=>p1min[i].Y(), ()=>F(p1min[i].X(), p1min[i].Y())], {withLabel: false});
   view.create('line3d', [p2min[i], ptemp], {dash: 1});
}
for (let i=0; i<localmax.length; i++){
   p1max[i] = board2.create('point', [i,i], {name: "Max"+(i+1), color: "blue"});
   p2max[i] = view.create('point3d', [()=>p1max[i].X(), ()=>p1max[i].Y(), zrang[0]], {name: "Max"+(i+1), color: "blue"});
   var ptemp = view.create('point3d', [()=>p1max[i].X(), ()=>p1max[i].Y(), ()=>F(p1max[i].X(), p1max[i].Y())], {withLabel: false});
   view.create('line3d', [p2max[i], ptemp], {dash: 1});
}

        board.update();

        var answer_min = document.getElementById(points_min_out);
        var answer_max=document.getElementById(points_max_out);
        board.on('update', function() {
        var pout = [];
        var pout2 = [];
        for(let i=0; i<p1max.length; i++){
     pout2[i] = [p1max[i].X(), p1max[i].Y()];
  }
        for (let i=0; i<p1min.length; i++){
       pout[i] = [p1min[i].X(), p1min[i].Y()];
   }
  });

```
### Specific feedback
```html
[[feedback:prt1]]
[[feedback:prt2]]
```
### Feedback variabels PRT1
```html
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

### Feedback variabels PRT2
```html
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
