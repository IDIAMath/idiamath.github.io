---
title: STACK Question -  3d-cube-transformations
usemathjax: true
theme: minima
---
# Question under construction

### Question Text


```javascript
<p id="p"></p>
[[jsxgraph height='850px' width='850px']]

 var board = JXG.JSXGraph.initBoard(divid, {
		        boundingbox: [-8, 8, 8, -8],
		        keepaspectratio: false,
		        axis: false
		    });

		    var view = board.create('view3d',
		        [[-6, -3], [8, 8],
		        [[-5, 5], [-5, 5], [-5, 5]]],
		        {
		            xPlaneRear: {visible: false},
		            yPlaneRear: {visible: false},
		            zPlaneRear: {visible: false}
		        });

		    

		    
   		    var names =["A","B","C","D","E","F","G","H","i"];

				function create3DCube(board, height, width,depth, x, y, z) {
					var point_attr = { fixed:true, label: { offset: [5, 5] } },
		        // Cube

		        phi = (1 + Math.sqrt(5)) * 0.5,
		        pol_attr = { borders: { strokeWidth: 0.5 }, fillColor: 'red' },
		        
  points = [],
      faces = [];
  // Create the points for the cube
var phi = (1 + Math.sqrt(5)) * 0.5;
   var pointCoords = [    
    [x-phi*width, y-phi*height, z-phi*depth],
    [x-phi*width, y+phi*height, z-phi*depth],
    [x+phi*width, y+phi*height, z-phi*depth],
    [x+phi*width, y-phi*height, z-phi*depth],
    [x-phi*width, y-phi*height, z+phi*depth],
    [x-phi*width, y+phi*height, z+phi*depth],
    [x+phi*width, y+phi*height, z+phi*depth],
    [x+phi*width, y-phi*height, z+phi*depth],
  ];

  for (var i = 0; i < 8; i++) {
    point_attr = { fixed:true, name:names[i],label: { offset: [5, 5] } }
    var point = view.create('point3d', pointCoords[i], point_attr);
    points.push(point);
  }

  // Create the faces for the cube
  var facesArray = [   
    [0, 1, 2, 3],
    [4, 5, 6, 7],
    [0, 1, 5, 4],
    [2, 3, 7, 6],
    [1, 2, 6, 5],
    [0, 3, 7, 4]
  ];
  for (var i = 0; i < facesArray.length; i++) {
    faces.push(board.create('polygon', [points[facesArray[i][0]], points[facesArray[i][1]], points[facesArray[i][2]], points[facesArray[i][3]]], {fillColor: 'red', borders: { strokeWidth: 0.5 }}));
  }
  
  return {
    points: points,
    faces: faces
  };
}	
var cube1 = create3DCube(board,0.4,0.4,0.4,1,5,3);
var p =document.getElementById("p");
var cubeX = cube1.points[0].X();
p.innerHTML = ` ${cubeX}`;

function moveTo(cube, x, y, z) {
  var points = cube.points;
var phi = 1.6018,
 width= 0.4,
height =0.4,
 depth= 0.4;
  for (var i = 0; i < points.length; i++) {
    points[i].moveTo([x + points[i].X() - phi*width, y + points[i].Y() - phi*height, z + points[i].Z() - phi*depth], 1);
  }
board.update();
view.update();
}

moveTo(cube1,15,15,15);

board.update();
[[/jsxgraph]]

<p>[[input:ans1]] [[validation:ans1]]</p>
```
