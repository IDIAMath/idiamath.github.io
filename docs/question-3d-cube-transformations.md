---
title: STACK Question -  3d-cube-transformations
usemathjax: true
theme: minima
---
# Question under construction

### Question Text


```javascript
<input type="text" id="fname" name="fname" placeholder="Input..." oninput="myFunction()">
[[jsxgraph height='850px' width='850px']]

//element id's list and select which inputs to change id
let elementId =["x","y","z","rotX","rotY","rotZ","scaleX","scaleY","scaleZ"];
let classNames =["position","scale","rotate"];
let mainElement =document.getElementsByClassName("matrixtable")[0];
let elements = mainElement.getElementsByTagName("input");

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

		    var points =[];
        var faces = [];
		    var namesr =["A","B","C","D","E","F","G","H","i"];
   
				function create3DCube(board, height, width,depth, x, y, z) {
					var point_attr = { withLabel: false, fixed:true, label: { offset: [5, 5] } },
		        // Cube
            

		        // Icosahedron
		        phi = (1 + Math.sqrt(5)) * 0.5,
		        pol_attr = { borders: { strokeWidth: 0.5 }, fillColor: 'red' },
		        
      faces = [];
      points = [];

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
    point_attr = { fixed:true, name:namesr[i],label: { offset: [5, 5] } }
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
    faces: faces,
    width:width,
    height:height,
    depth:depth
  };
}	
var cube1 = create3DCube(board,0.4,0.4,0.4,1,5,3);



function goTo(cube,x, y, z) {
					var point_attr = { withLabel: true, fixed:true, label: { offset: [5, 5] } },
		        // Cube

		        // Icosahedron
		        phi = (1 + Math.sqrt(5)) * 0.5,
		        pol_attr = { borders: { strokeWidth: 0.5 }, fillColor: 'red' },
		        
      
  // Create the points for the cube 
 phi = (1 + Math.sqrt(5)) * 0.5;
   var pointCoords = [    
    [x-phi*cube.width, y-phi*cube.height, z-phi*cube.depth],
    [x-phi*cube.width, y+phi*cube.height, z-phi*cube.depth],
    [x+phi*cube.width, y+phi*cube.height, z-phi*cube.depth],
    [x+phi*cube.width, y-phi*cube.height, z-phi*cube.depth],
    [x-phi*cube.width, y-phi*cube.height, z+phi*cube.depth],
    [x-phi*cube.width, y+phi*cube.height, z+phi*cube.depth],
    [x+phi*cube.width, y+phi*cube.height, z+phi*cube.depth],
    [x+phi*cube.width, y-phi*cube.height, z+phi*cube.depth],
  ];

  board.removeObject(cube.points)
  for (var i = 0; i < 8; i++) {
    point_attr = { fixed:true, name:namesr[i],label: { offset: [5, 5] } }
    var point = view.create('point3d', pointCoords[i], point_attr);
    points.shift();
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
    points:points,
    faces:faces,
    cube:cube,
    x:x,
    y:y,
    z:z
  };
}	
let position =[];
//Give elements an Id and className
for(let z=0; z< elements.length;z++) {
    elements[z].id=elementId[z];
    if(z < 3) {
        //give a className to inputs
        elements[z].className=classNames[0];

        //add input value restrictions here


        //add for loop that sets input value equal to cube value here

        

        //function to transform cube when input changes
        elements[z].addEventListener('input', function() {
             position = [0,0,0];
            for(let i=0; i<2;i++) {
                position[i] = document.getElementsByClassName(classNames[0])[i].value;
            }
            goTo(cube1,position[0],position[1],position[2]);
            
});
    }else if(z > 5) {
        elements[z].className="scale";
        elements[z].addEventListener('input', function() {
        console.log("test2");
    });
    }else {
        elements[z].className="rotate";
        elements[z].addEventListener('input', function() {
        console.log("test3");
    });
    }
    
}
[[/jsxgraph]]

<p>[[input:ans1]] [[validation:ans1]]</p>
```
