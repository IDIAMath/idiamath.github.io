---
title: STACK Question -  3d-cube-transformations
usemathjax: true
theme: minima
---
# Question under construction
![image](https://user-images.githubusercontent.com/43517080/217853631-9f85c393-c3c5-49a0-94a9-0b9b6ee83d1a.png)

### Question variables
```rust
M:matrix([1,2,3],[3,4,5],[6,7,8]);
```

### Question Text

```javascript
<p>Transform the given cube</p>
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
        
        function getRotationMatrices(rotation) {
  var radX = -rotation[0] * Math.PI / 180;
  var radY = -rotation[1] * Math.PI / 180;
  var radZ = -rotation[2] * Math.PI / 180;

  var xRotation = [    [1, 0, 0],
    [0, Math.cos(radX), -Math.sin(radX)],
    [0, Math.sin(radX), Math.cos(radX)],
  ];

  var yRotation = [    [Math.cos(radY), 0, Math.sin(radY)],
    [0, 1, 0],
    [-Math.sin(radY), 0, Math.cos(radY)],
  ];

  var zRotation = [    [Math.cos(radZ), -Math.sin(radZ), 0],
    [Math.sin(radZ), Math.cos(radZ), 0],
    [0, 0, 1],
  ];

  return { xRotation, yRotation, zRotation };
}


//rotate the cube
  function rotateMatrix(pointCoords, position, rotMat, rotation) {

    //set cube coordinates to (0,0,0) in order to rotate the cube
  for (var i = 0; i < pointCoords.length; i++) {
    pointCoords[i][0] = pointCoords[i][0] - position[0];
    pointCoords[i][1] = pointCoords[i][1] - position[1];
    pointCoords[i][2] = pointCoords[i][2] - position[2];
  } 
  
  //apply matrix rotations
  var rotXYZ = JXG.Math.matMatMult(rotMat.xRotation, rotMat.yRotation);
  rotXYZ = JXG.Math.matMatMult(rotXYZ, rotMat.zRotation);
  pointCoords = JXG.Math.matMatMult(pointCoords, rotXYZ);

  //set cube coordinates back to user provided coordinates
  for (var i = 0; i < pointCoords.length; i++) {
    pointCoords[i][0] = pointCoords[i][0] + position[0];
    pointCoords[i][1] = pointCoords[i][1] + position[1];
    pointCoords[i][2] = pointCoords[i][2] + position[2];
  }

  return pointCoords;
}




				function create3DCube(position , scale, rotation) {
          
           // require x,y, and z coordinates from user
           if (!Array.isArray(position) || position.length !== 3) {
    throw new Error('The "coordinates" parameter must be an array with three elements.');
  }
          rotation = rotation || [0, 0, 0];
          position = position || [0, 0, 0];
          scale = scale || [0.5, 0.5, 0.5];

					var point_attr = { withLabel: false, fixed:true, label: { offset: [5, 5] } },
		        // Cube
            
		        pol_attr = { borders: { strokeWidth: 0.5 }, fillColor: 'red' },
		        
      faces = [];
      points = [];

var rotMat =getRotationMatrices(rotation)

var phi = (1 + Math.sqrt(5)) * 0.5;
var pointCoords = [  
  [position[0]-phi*scale[0], position[1]-phi*scale[1], position[2]-phi*scale[2]],
  [position[0]-phi*scale[0], position[1]+phi*scale[1], position[2]-phi*scale[2]],
  [position[0]+phi*scale[0], position[1]+phi*scale[1], position[2]-phi*scale[2]],
  [position[0]+phi*scale[0], position[1]-phi*scale[1], position[2]-phi*scale[2]],
  [position[0]-phi*scale[0], position[1]-phi*scale[1], position[2]+phi*scale[2]],
  [position[0]-phi*scale[0], position[1]+phi*scale[1], position[2]+phi*scale[2]],
  [position[0]+phi*scale[0], position[1]+phi*scale[1], position[2]+phi*scale[2]],
  [position[0]+phi*scale[0], position[1]-phi*scale[1], position[2]+phi*scale[2]]
];

//we have to transalte the cube to origin, rotate
//then translate back to desired user position
//otherwise the cube will move around the rotation axis.

//translate points to origin
pointCoords = rotateMatrix(pointCoords,position,rotMat,rotation);

// Create the points for the cube 
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
    pointCoords:pointCoords,
    points: points,
    faces: faces,
    scale:scale,
    rotation:rotation,
    position:position
  };
}	
var cube1 = create3DCube([1,5,3],0,[45,5,5]);


function transform(cube,position,scale,rotation) {
  position = position || [cube.position[0], cube.position[1], cube.position[2]];
  scale = scale || [cube.scale[0], cube.scale[1], cube.scale[2]];
  rotation = rotation || [cube.rotation[0], cube.rotation[1], cube.rotation[2]];


  //destructuring: if provided wrong number of elements, allow it but set missing element value to original cube value
  let [x = cube.position[0], y = cube.position[1], z = cube.position[2]] = position;
  let [scaleX = cube.scale[0], scaleY = cube.position[1], scaleZ = cube.position[2]] = scale;
  let [rotX = cube.rotation[0], rotY = cube.rotation[1], rotZ = cube.rotation[2]] = rotation;




					var point_attr = { withLabel: true, fixed:true, label: { offset: [5, 5] } },
		        

		        pol_attr = { borders: { strokeWidth: 0.5 }, fillColor: 'red' };
		        
  // Create the points for the cube 

  var rotMat =getRotationMatrices(rotation);
var phi = (1 + Math.sqrt(5)) * 0.5;

   var pointCoords = [    
    [x-phi*scaleX, y-phi*scaleY, z-phi*scaleZ],
    [x-phi*scaleX, y+phi*scaleY, z-phi*scaleZ],
    [x+phi*scaleX, y+phi*scaleY, z-phi*scaleZ],
    [x+phi*scaleX, y-phi*scaleY, z-phi*scaleZ],
    [x-phi*scaleX, y-phi*scaleY, z+phi*scaleZ],
    [x-phi*scaleX, y+phi*scaleY, z+phi*scaleZ],
    [x+phi*scaleX, y+phi*scaleY, z+phi*scaleZ],
    [x+phi*scaleX, y-phi*scaleY, z+phi*scaleZ],
  ];

//we have to transalte the cube to origin, rotate
//then translate back to desired user position
//otherwise the cube will move around the rotation axis.

//translate points to origin
pointCoords = rotateMatrix(pointCoords,position,rotMat,rotation);

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
    position:position,
    scale:scale,
    rotation:rotation
    
  };
}		

//debounce function for input optimization
const debounce = (func, delay) => {
  let timerId;
  return function(...args) {
    if (timerId) {
      clearTimeout(timerId);
    }
    timerId = setTimeout(() => {
      func(...args);
      timerId = null;
    }, delay);
  };
};



//Give elements an Id and className
let position =[];
let scale=[];
let rotation =[];
for(let z=0; z< elements.length;z++) {
    elements[z].id=elementId[z];
    elements[z].value=0;
        //add input value restrictions here

        //give a className to inputs
        elements[z].className=classNames[0];



        //add for loop that sets input value equal to cube value here

        

        //function to transform cube when input changes
        elements[z].addEventListener('input', debounce(function() {
          console.log("s");
               position = [0,0,0];
               scale = [0,0,0];
               rotation = [0,0,0]; 
 
            for(let i=0; i<3;i++) {
                position[i] = parseFloat(document.getElementsByClassName(classNames[0])[i].value);
                scale[i] = parseFloat(document.getElementsByClassName(classNames[0])[i+3].value);
                rotation[i] = parseFloat(document.getElementsByClassName(classNames[0])[i+6].value);



            }
            transform(cube1,position,scale,rotation);
            },500));

    
    
    
}
[[/jsxgraph]]

<p>[[input:ans1]] [[validation:ans1]]</p>
```
