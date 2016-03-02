## SVG Matrix and Javascript

### HTML
```html
<svg id="the-svg">
    <polygon id="polygon1" points="0 0 0 100 100 100 100 0"></polygon>
</svg>
```

### Get the Elements
```javascript
var svg, polygon;
svg = document.getElementById("the-svg");
polygon = document.getElementById("polygon1");

```

### SVG Matrix Transformations in Javascript
```javascript
var identityMatrix, m1, m2, m3, m4, m5, m6, m7, m8, m9, m10, matrix;

// The IdentityMatrix is {a:1, b:0, c:0, d:1, e:0, f:0}
identityMatrix = svg.createSVGMatrix();		

m1 = identityMatrix.translate(42,333);
m2 = identityMatrix.rotate(90);
m3 = identityMatrix.rotateFromVector(1, 1);
m4 = identityMatrix.scale(2);
m5 = identityMatrix.scaleNonUniform(2, 1);
m6 = identityMatrix.skewX(0.5);
m7 = identityMatrix.skewY(0.7);
m8 = identityMatrix.flipX();
m9 = identityMatrix.flipY();
m10 = identityMatrix.inverse();

// or directly:
m1.e = 42;
m1.f = 333;

// Rotate around a point (read from right to left)
matrix = identityMatrix.translate(30, 50).rotate(90).translate(-30,-50);

// Matrix Multiplication
m12 = m4.multiply(m1);

```
	

### Apply Matrix on SVGPoint
The Matrix can be applied on single points (**SVGPoints**) 
```javascript
var point; 

point = svg.createSVGPoint();
point.x = 3;		// set the coords of that point
point.y = 4;		
point = point.matrixTransform(matrix); // apply matrix on that point
// the original point is translated, rotated, etc whatever the matrix says.

```

### Apply Matrix on Elements (g, rect, circle, polygon,...)

```javascript
// 1. create a SVGTransform
var transform = polygon.transform.baseVal.createSVGTransformFromMatrix(identityMatrix);

// 2. append transform on polygon (transform="matrix(1 0 0 1 0 0)")
polygon.transform.baseVal.appendItem(transform);

// 3. From now on set the matrix of the element like that
polygon.transform.baseVal.getItem(0).setMatrix(m4);

// Get the SVGMatrix
polygon.getCTM();

```

### getScreenCTM
```javascript
var m, x, y; 

//Gets the transformation matrix from the current user units to the screen coordinate system.
m = svg.getScreenCTM(); 	

x = svg.getScreenCTM().e	// get x-pos of svg-element in current browser 	
y = svg.getScreenCTM().f 	// get y-pos of svg-element in current browser
// paddings and margins of body and svg element are considered

```


### Get Mouse Coordinates 
Convert the mouse coordinates (in screen-pixels) into the global space of your SVG document
Change the element "**svg**.getScreenCTM()" to whatever element you need.
```javascript
var pt, globalPoint; 

pt = svg.createSVGPoint();
pt.x = evt.clientX;
pt.y = evt.clientY;
globalPoint = pt.matrixTransform(svg.getScreenCTM().inverse());

```

### SVGTransform (transform="translate(1, 1) scale(2)")
Create, change and get transform-items of an element

```javascript
var transform;

// create a transform from a matrix
transform = polygon.transform.baseVal.createSVGTransformFromMatrix(identityMatrix);
transform.setRotate(33, 100, 100);

// append a transform
polygon.transform.baseVal.appendItem(transform);

// Get the first SVGTransform
transform = polygon.transform.baseVal.getItem(0);

polygon.transform.baseVal.getItem(0).setTranslate(0,36);

polygon.transform.baseVal.clear();			// removes all items from baseVal, Matrix is still applied?

// Merge several transforms to one single Matrix
polygon.transform.baseVal.consolidate();	
//<g transform="scale(1, 1) rotate(-30 10 25)"> --> <g transform="matrix(?,?,?,?,?,?)">

```




### Links
[https://www.w3.org/TR/SVG/coords.html](https://www.w3.org/TR/SVG/coords.html)<br>
[How to extract position, rotation and scale from matrix SVG -(Stackoverflow)](http://stackoverflow.com/questions/16359246/how-to-extract-position-rotation-and-scale-from-matrix-svg)
[Understanding Matrix in SVG - (Stackoverflow)](http://stackoverflow.com/questions/31281515/understanding-matrix-in-svg)




