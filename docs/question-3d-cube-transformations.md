---
title: STACK Question -  3d-cube-transformations
usemathjax: true
theme: minima
---
# STACK Question 8 Interpretation

> Two figures showing a 3D object and a rotation and/or translation of the same object.
> Enter a matrix which transforms the first figure into the second.

- [XML Code](XML/question-select-extremal.xml)



| ![image](https://user-images.githubusercontent.com/43517080/219105972-62f44e65-ac71-4fd8-9d56-305405474e1c.png) |
|:--:|
| <sup>*-  figure 1:*</sup> *First impression of the question* |

## Question description
The user is given a skyblue cube, they need to transform it into the green cube. The cube transforms when the user updates the input fields.

## Student perspective

| ![image](https://user-images.githubusercontent.com/43517080/219118766-bedebf56-67bd-4df1-838b-b98a4393ba89.png) |
|:--:|
| <sup>*-  figure 2:*</sup>  *When the user updates the input fields* |

When the user updates the input field the cube is transformed

| ![image](https://user-images.githubusercontent.com/43517080/219126215-cf5d8695-bebe-4ece-ab42-366c2fdeb3a7.png) |
|:--:|
| <sup>*-  figure 3:*</sup> *When the user submits the question* |

When the user submits the question, STACK highlights the wrong matrix entries in red

## Question code
### Question variables
```rust
/* Default task cube parameters */
MPosition:[1,5,3];
MScale:[1,1,1];
MRotation:[45,15,100];
color:"green";

/*input this variable into the model answer if its not */
M:matrix(MPosition,MScale,MRotation);

/* User controlled cube parameters */
MPosition_user:[-2,-1,-2];
MScale_user:[0.5,0.4,0.7];
MRotation_user:[45,0,0];
color_user:"skyblue";
```

### Question Text

```javascript
<p id="p">Transform the {#color_user#} cube into the {#color#} cube</p>
[[jsxgraph height='850px' width='850px']]

//remove qoutation marks from html<p> element
  var p =document.getElementById("p");
  p.innerHTML = p.innerHTML.replace(/"/g, '');


//element id's list and select which inputs to change id
var elementId =["x","y","z","rotX","rotY","rotZ","scaleX","scaleY","scaleZ"];
var classNames =["position","scale","rotate"];
var mainElement =document.getElementsByClassName("matrixtable")[0];
var elements = mainElement.getElementsByTagName("input");

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
		    
        //create rotation matrices geometry related
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


//translate a cube to origin, add the rotation matrices to rotate it
//then retranslates it back to the original user provided coords
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



// creates a 3D cube
function create3DCube(position , scale, rotation, color) {
          
           // require x,y, and z coordinates from user
           if (!Array.isArray(position) || position.length !== 3) {
    throw new Error('The "coordinates" parameter must be an array with three elements.');
  }
          rotation = rotation || [0, 0, 0];
          position = position || [0, 0, 0];
          scale = scale || [0.5, 0.5, 0.5];
          color = color || "blue";

var point_attr = { withLabel: false, fixed:true, fillColor: 'red',label:{offset:[5, 5]}},
pol_attr = { borders: { strokeWidth: 0.5 }, fillColor: color },
		        
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
    point_attr = { fixed:true,size:2, name:namesr[i], label: { offset: [5, 5] } }
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
    faces.push(board.create('polygon',[points[facesArray[i][0]],points[facesArray[i][1]],
    points[facesArray[i][2]], points[facesArray[i][3]]], {fillColor: color, borders: 
    { strokeWidth: 0.5 }}));
  }
  
 
  var cubeProperties = {
    pointCoords: pointCoords,
    points: points,
    faces: faces,
    scale: scale,
    rotation: rotation,
    position: position,
    color: color,
    // transforms the 3Dcube
    transform: function(newPosition, newScale, newRotation, newColor) {
      this.position = newPosition || this.position;
      this.scale = newScale || this.scale;
      this.rotation = newRotation || this.rotation;
      this.color = newColor || this.color;

      //remove previous points and faces
      board.removeObject(this.points);
      for (var i = 0; i < this.faces.length; i++) {
        board.removeObject(this.faces[i]);
      }

      //create points and faces for new cube
      var newCube = create3DCube(this.position, this.scale, this.rotation, this.color);
      this.pointCoords = newCube.pointCoords;
      this.points = newCube.points;
      this.faces = newCube.faces;
    }
  };

  return cubeProperties;
}	
var cube2 = 
create3DCube({#MPosition#},{#MScale#},{#MRotation#},{#color#});
var cube1 = 
create3DCube({#MPosition_user#}, {#MScale_user#}, {#MRotation_user#},{#color_user#});

//debounce function for input optimization
const debounce = (func, delay) => {
  var timerId;
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
var position =cube1.position;
var scale=cube1.scale;
var rotation =cube1.rotation;

// sets input value equal to cube value here
for (var f=0; f<3;f++) {
    elements[f].value=position[f];
    elements[f+3].value=scale[f];
    elements[f+6].value= rotation[f];
}
//change inputfield id, classname, and add function to handle user input
for(var z=0; z< elements.length;z++) {
    elements[z].id=elementId[z];
    elements[z].placeholder=elementId[z];

    //give a className to inputs
    elements[z].className=classNames[0];
        
    //function to transform cube when input changes
    elements[z].addEventListener('input', debounce(function() {

    for(var i=0; i<3;i++) {
      position[i]= parseFloat(document.getElementsByClassName(classNames[0])[i].value);
      scale[i]= parseFloat(document.getElementsByClassName(classNames[0])[i+3].value);
      rotation[i]= parseFloat(document.getElementsByClassName(classNames[0])[i+6].value);

            }
            cube1.transform(position,scale,rotation);
            },500));
    
}
[[/jsxgraph]]

<p>[[input:ans1]] [[validation:ans1]]</p>
```

### PRT
![image](https://user-images.githubusercontent.com/43517080/218732173-727fe660-760a-4d39-9ec6-47804989068d.png)
