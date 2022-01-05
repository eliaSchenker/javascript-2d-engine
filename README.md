# 2D Javascript Render Engine
The 2D Javascript Render Engine makes drawing on the canvas easy.<br>
It also makes converting from world space to canvas space really easy.<br>
This readme explains how to use the Renderer.

## Integrating the Render engine
To add the render engine to your site, simple copy the script in the same folder and add it using a script tag:
```html
<script src="Renderer.js" type="text/javascript"></script>
```

## Creating the Renderer object
When creating the Renderer object you must pass a canvas, the camera X Size and the camera Y Size in the constructor (the camera size will be explained in the paragraph):
```javascript
let canvas = document.getElementsByTagName("canvas")[0];
//Create the renderer by passing the canvas, the camera X Size and the camera Y Size
let renderer = new Renderer(canvas, 10, 10);
```

## The Camera
When initializing the Renderer object you must pass the camera size are the dimensions of the camera. These will then be displayed on the camera. The camera size and position can be changed very simply by accessing the respective variables in the object:
```javascript
renderer.cameraXSize = 10; //The X-Size of the camera
renderer.cameraYSize = 10; //The Y-Size of the camera
renderer.cameraPosition = new Vector2(0, 0); //Set the position of the camera
```
The user is able to interact with the camera (zooming, and dragging)

## Vectors
The Vector2 object represents either a point in 2d-space or a vector.<br>
Creating a Vector2 is very easy:
```javascript
let myVector = new Vector2(10, 10);
```
The Vector2 contains various built-in functions:
```javascript
//Calculate the distance between two points:
let myPoint = new Vector2(0, 10);
let distance = myPoint.distanceTo(new Vector2(0, 20)); //Will return 10
//Calculate the angle to another point (in radians)
let angle = myPoint.angleTo(new Vector2(0, 20)); //Will return π/2
//Move a point in direction of a angle (in Radians) by a distance
let angle = Math.PI / 2; //90 Degrees
let distance = 2;
let newPoint = myPoint.moveAtAngle(angle, distance);
//Move a point towards another point by a distance
let distance = 2;
let otherPoint = new Vector2(10, 10);
let newPoint = myPoint.moveAtAngle(otherPoint, distance);
//Rotate a point around another point by an angle (in Radians)
let angle = Math.PI / 2;
let centerPoint = new Vector2(10, 10);
let newPoint = myPoint.moveAtAngle(centerPoint, angle);
//Vector functions:
//Calculate the angle of a vector (in radians)
let myVector = new Vector2(1, 1);
let angle = myVector.getAngleRadians(); //Will return 0.7853...
//Calculate the angle of a vector (in degrees)
let myVector = new Vector2(1, 1);
let angle = myVector.getAngleDegrees(); //Will return 45 Degrees
//Calculate the magnitude (length) of a vector
let myVector = new Vector2(1, 1);
let magnitude = myVector.getMagnitude(); //Wil lreturn 1.41421
```

## Drawing objects
There are different types of objects which you can draw (e.g circles, rectangles...)<br>If you want to draw objects you need to add them to the render queue of the renderer. The render queue must be emptied before every redrawing.<br>
Example:
```javascript
renderer.reset_render_objects(); //Reset the render queue

//Render a circle
/* Circle has a radius of 5, a position of (0, 0), a color of "#FF0000" and is filled (true) */
renderer.toRenderObjects.push(new CircleRenderObject(5, new Vector2(0, 0), "#FF0000", true));

//Render a rectangle
/* The rectangles bottom left position is (0, 0), it's width is 10, it's height is 5, it has a color of "#0000FF" and is not filled (false) */
renderer.toRenderObjects.push(new RectRenderObject(new Vector2(0, 0), new Vector2(10, 5), "#0000FF", false));

//Render a line
/* The lines has a start position of (0, 0) an end position of (2, 2), a width of 1 and a color of "#000000" */
renderer.toRenderObjects.push(new LineRenderObject(new Vector2(0, 0), new Vector2(2, 2), 1, "#000000"));

//Render an arrow
/* The arrow has a start position of (2, 2), an end position of (4,4) and a color of "#00FFFF" */
renderer.toRenderObjects.push(new ArrowRenderObject(new Vector2(2, 2), new Vector2(4,4), "#00FFFF"));

//Render a polygon (takes an array of positions instead of just one)
/* The polygon has the following positions: (-2, -2), (-4, -2), (-5, -3) and is  filled (true) and has a color of "#FFFF00"*/
renderer.toRenderObjects.push(new PolygonRenderObject([new Vector2(-2, -2), new Vector2(-4, -2,), new Vector2(-5, -3)], false, "#FFFF00"));

//Render a text
/* The text has a font-size of 25px and a font of Arial, it says "Hello World", is aligned to the left and has a color of "#000000" */
renderer.toRenderObjects.push(new TextRenderObject("25px Arial", "Hello World", "left", "#000000"));
```
When you are done filling the render queue with various objects you can simply render them using
```javascript
renderer.render_frame();
```
## Interaction Events
The Renderer Class adds the ability to add interaction events to your objects.
These events include
<ul>
    <li>Mouse click event</li>
    <li>Drag Event</li>
    <li>Mouse up event</li>
</ul>
Use the addInteractionEvents function on a renderobject to add the events in the following order: onClickEvent, onMouseDragEvent, onMouseUpEvent.<br>If you don't want to use any of the events, simply pass it as undefined<br>
You must bind the function with "this" so that the variables in the function are read out of the right context (more info on bind: <a href="https://developer.mozilla.org/de/docs/Web/JavaScript/Reference/Global_Objects/Function/bind" target="_blank">Mozilla Developer Wiki</a>)<br>
The mouse position of the click, drag or up event will always be passed by the function.<br><br>
Here is an example of adding a drag event to a circle:

```javascript
//Create a simple circle object
let circleObject = new CircleRenderObject(2, new Vector2(-5, 5), "#000000", true);
        
//Add an interaction event to the circle, which lets the user drag it
//The onClickEvent is undefined, the drag event has a function and the onMouseUpEvent is also undefined

circleObject.addInteractionEvents(undefined, (function(e) {
    circleObject.position = e; //Set the new position to the mouse position
}).bind(this), undefined); //You must bind "this" on your function to let the event use your context (your objects and variables)

//Add the circle to your render queue
renderer.toRenderObjects.push(circleObject);
```

## Examples
An example can be found in the example.html file.<br>
Here are some simulations which use this library: <a href="https://eliaschenker.com/physics-simulations" target="_blank">Simulations</a>

<br><br>
&copy; Elia Schenker 2021