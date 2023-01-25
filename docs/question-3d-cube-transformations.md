---
title: STACK Question -  3d-cube-transformations
usemathjax: true
theme: minima
---
# Question under construction

### Question Text


```javascript
<p></p>
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

		    

		    
   
				function create3DCube(board, height, width, x, y, z) {
					var point_attr = { withLabel: false, fixed:true, label: { offset: [5, 5] } },
		        // Cube

		        // Icosahedron
		        phi = (1 + Math.sqrt(5)) * 0.5,
		        pol_attr = { borders: { strokeWidth: 0.5 }, fillColor: 'red' },
		        
  points = [],
      faces = [];
  // Create the points for the cube
var phi = (1 + Math.sqrt(5)) * 0.5;
  var pos_x = x, pos_y = y, pos_z = z;
  var size_x = width, size_y = height, size_z = 1;
   var pointCoords = [    
    [pos_x-phi*size_x, pos_y-phi*size_y, pos_z-phi*size_z],
    [pos_x-phi*size_x, pos_y+phi*size_y, pos_z-phi*size_z],
    [pos_x+phi*size_x, pos_y+phi*size_y, pos_z-phi*size_z],
    [pos_x+phi*size_x, pos_y-phi*size_y, pos_z-phi*size_z],
    [pos_x-phi*size_x, pos_y-phi*size_y, pos_z+phi*size_z],
    [pos_x-phi*size_x, pos_y+phi*size_y, pos_z+phi*size_z],
    [pos_x+phi*size_x, pos_y+phi*size_y, pos_z+phi*size_z],
    [pos_x+phi*size_x, pos_y-phi*size_y, pos_z+phi*size_z],
  ];
  for (var i = 0; i < 8; i++) {
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
create3DCube(board,1,1,1,5,3)
		    
[[/jsxgraph]]

<p>[[input:ans1]] [[validation:ans1]]</p>
```
