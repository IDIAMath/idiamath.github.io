---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

title: IDIAMath
usemathjax: true
theme: minima
permalink: /
---

# Tutorials (Question Examples)

The following tutorials are written to illustrate concrete questions
that could be given to students in higher education.
We aim to discuss the STACK and JSXGraph features used as well as the
motivation for the question, but there is room for improvement, so 
please give feedback.

Each tutorial will provide the Moodle XML code for the question,
so that you can easily import it in your own server and test it,
as well as the discussing of the design.

The questions are not really indended to be ready for use.  
We reckon that most teachers will want to tailor the questions to
their students and use the questions primarily as examples to build
on.

## Visualising functions in two variables

+ Question [match-properties](question/match-properties.md).
  Give an arbitrary function with given properties, with visual aid.
+ Question [Tune 3D Function](question/tune-3d-function.md)
    Tune parameters of a function to match another function visually
+ Question [Tune 3D polynomoial](question/tune-3d.md)
    Minimal working example of question using jsxgraph

# Sketches (questions under construction)

## Visualising functions in two variables

+ Question 1 [Interpret partial derivatives](question/partial-derivative.md)
+ Question 6 [Select matching function](question/select-matching-function.md)
+ Question 7 [Select matching partial derivative](question/select-matching-partial-derivative.md)

## Calculus of functions in two variables

### Functions of two variables
+ Question [Find the function term](question/2D_parametric_surfaces_graphs.md) when the graph is drawn
### Differentiation / stationary points / Hessian
+ Question [select-extremal](question/select-extremal.md)
  Select maxima/minima on a plot

In the following questions the tangent plane is shown
+ Question [stationary point](question/2D_function_stationary.md) based on trig function
+ Question [stationary point](question/2D_function_stationary_legendre.md) based on Legendre polynomial

In optimization the local 2nd order behavior of a function hast to be analysed. This is addressed by these questions
+ Question [Hessian positive definit](question/2D_function_negative_definite_hessian.md) 
+ Question [Hessian negative definit](question/2D_function_negative_definite_hessian.md) 

### Integration Domains in 2D
+ Question [Polar Coordinates (Matching)](question/2D_polar_coordinates_matching_algebraic.md) (fit the reference volume by entering intervals for $$r$$ and $$\phi$$


### Integration Domains in 3D
+ Question [Cylindrical Coordinates (Matching)](question/3D_cylindrical_coordinates_matching.md) (fit the reference volume by dragging sliders)
+ Question [General Coordinate Transformation](question/3D_cylindrical_coordinates_matching.md) (this is like a template for 3D Transform)
+ Question [Spherical Coordinates (Matching)](question/3D_spherical_coordinates_matching.md) (fit the reference volume by dragging sliders)
+ Question [Spherical Coordinates (Rotating)](question/3D_spherical_coordinates_rotating.md) (fit the reference volume by entering parameter intervals)

## Visualising curves in 3D
+ Question [Circular curve](question/3D_curve_matching_circular.md): Find the parameters of $$t \mapsto \begin{pmatrix} r \cdot \cos(t) \\ r \cdot \sin(t) \\ h \cdot \cos(n \cdot t - \phi) \end{pmatrix}.$$
+ Question [Elliptical curve (helix)](question/3D_curve_matching_elliptic_helix.md): Find the parameters of the curve given by $$t \mapsto \begin{pmatrix} 
x_0+ a \cdot \cos(t) \cdot \cos(\alpha) - b \cdot \sin(t) \cdot \sin(\alpha) \\ 
y_0+ a \cdot \cos(t) \cdot \sin(\alpha) + b \cdot \sin(t) \cdot \cos(\alpha) \\ 
h \cdot t
\end{pmatrix}.$$

+ Question [Elliptical curve](question/3D_curve_matching_elliptic_helix.md): Find the parameters of the curve given by  $$t \mapsto \begin{pmatrix} x_0+ a \cdot \cos(t) \cdot \cos(\alpha) - b \cdot \sin(t) \cdot \sin(\alpha) \\ y_0+ a \cdot \cos(t) \cdot \sin(\alpha) + b \cdot \sin(t) \cdot \cos(\alpha) \\ h \cdot \cos(t)\end{pmatrix}.$$

+ Question [Rotating a curve about two axis](question/3D_rotation_two_axis_solo.md) This question trains the imagination of rotating objects around two axis of the coordinate system.

## Visualising solids in 3D

+ Question 8 [3d-cube-transformations](solids/3d-cube-transformations.md)

## Vector fields in 3D

+ Question  [Curl of a vector field](question/3D_vectorfield_curl.md) Given is a vector field (as formula and plotted). The student has to choose the right representation for the curl. The filed is selected from a list of given fields.

+ Question  [Vector field of given curl](question/3D_vectorfield_curl_given.md) Given is a curl of a vector field (as formula and plotted). The student has to choose the right representation for a vector field, that is fits to the curl. The filed is selected from a list of given fields.


## Adaptive Questions
+ Question [Rotating a curve about two axis](question/3D_rotation_two_axis_adaptive.md) This question trains the imagination of rotating objects and guides the student through the topic.
+ Question [Curl of a given vector field](question/3D_vectorfield_curl_adaptive.md) This question trains the computation of the curl of a vector field. This implementation demonstrates the usaga of a single JSXGraph object in an adaptive setting.


## Additional Material

Here will find applets demonstrating JSXGraph in different applications. [JSXGraph Examples](JSXGraphExamples/JSXGraphExamples.md) This material is usefull for the HELM material as well.

A huge collection you will find at the [JSXGraph homepage](https://jsxgraph.org) and the example data base [there](https://jsxgraph.uni-bayreuth.de/share/).
## HTML test
+ [Test of html-link with jsxgraph](question/htmltest.html)



## Notes

+ [JSXGraph](JSXGraph) sandbox

## Documentation from Other Sources

+ [JSXGraph in Moodle](https://moodle.oulu.fi/question/type/stack/doc/doc.php/Authoring/JSXGraph.md)
