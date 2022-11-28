# Canvas

## 1 Basic usage

### 1)Basic usage of canvas

At the end of this page, you will know how to set up canvas 2D context and have drawn a first example in your browser.

The `<canvas>` element

```
<canvas id="tutorial" width="150" height="150"></canvas>
```

### 2)The rendering context

The `<canvas>` element creates a fixed-size drawing surface that exposes one or more rendering contexts, which are used to create and manipulate the content shown. In this tutorial, we focus on the 2D rendering context. ==Other contexts may provide different types of rendering; for example, WebGL uses a 3D context based on OpenGL ES.==

The canvas is initially blank. To display something, a script first needs to access the rendering context and draw on it. The `<canvas>` element has a method called `<getContext()`, used to obtain the rendering context and its drawing functions. `getContext()` takes one parameter, the type of context. For 2D graphics, such as those covered by this tutorial, you specify `“2d”` to get a `CanvasRenderingContext2D`.

```
const canvas = document.getElementById('tutorial');
const ctx = canvas.getContext('2d');
```

###  3) Checking for support

```
const canvas = document.getElementById('tutorial');

if (canvas.getContext) {
	const ctx = canvas.getContext('2d');
	// drawing code here
} else {
	// canvas-unsupported code here
}
```

### 4) A simple example

```
const canvas = document.getElementById("canvas");
if (canvas.getContext) {
	const ctx = canvas.getContext("2d");
	
	ctx.fillStyle = "rgb(200, 0, 0)";
    ctx.fillRect(10, 10, 50, 50);
    
    ctx.fillStyle = "rgba(0, 0, 200, 0.5)";
    ctx.fillRect(30, 30, 50, 50);
}
```

## 2 Drawing shapes with canvas

By the end of this article, you will have learned how to draw rectangles, triangles, lines, arcs and curves, providing familiarity with some of the basic shapes.

### Drawing rectangles

Unlike SVG, `<canvas>` only supports two primitive shapes: rectangles and paths (lists of points connected by lines). 

First let’s look at the rectangle. There are three functions that draw rectangles on the canvas:

```
fillRect(x, y, width, height)
// Draws a filled rectangle
strokeRect(x, y, width, height)
// Draws rectangular outline
clearRect(x, y, width, height)
// Clears the specified rectangular area, making it fully transparent
```

Each of these three functions takes the same parameters. `x` and `y` specify the position on the canvas (relative to the origin) of the top-left corner of the rectangle. `width` and `height` provide the rectangle’s size.

Rectangular shape example

```
function draw() {
	const canvas = document.getElementById('canvas');
	if (canvas.getContext) {
		const ctx = canvas.getContext('2d');
		
		ctx.fillRect(25, 25, 100, 100);
		ctx.clearRect(45, 45, 60, 60);
		ctx.strokeRect(50, 50, 50, 50);
	}
}
```

![img](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAJYAAACWCAYAAAA8AXHiAAAAAXNSR0IArs4c6QAABK9JREFUeF7tncFuE1AMBNMvR3w5XOBSEWkXZ1K7Gs55tjs7XVUVUj8e/pMAQOADmOlICTwUSwkQAoqFYHWoYukAQkCxEKwOVSwdQAgoFoLVoYqlAwgBxUKwOlSxdAAhoFgIVocqlg4gBBQLwepQxdIBhIBiIVgdqlg6gBBQLASrQxVLBxACioVgdahi6QBCQLEQrA5VLB1ACCgWgtWhiqUDCIF3ifULud6h/0sAzx1f8OcrV6z/VYB5h+eOL1AsxozhVDx3fIFiDRVgnuO54wsUizFjOBXPHV+gWEMFmOd47vgCxWLMGE7Fc8cXKNZQAeY5nju+QLEYM4ZT8dzxBYo1VIB5jueOL1AsxozhVDx3fIFiDRVgnuO54wsUizFjOBXPHV+gWEMFmOd47vgCxWLMGE7Fc8cXKNZQAeY5nju+QLEYM4ZT8dzxBYo1VIB5jueOL1AsxozhVDx3fIFiDRVgnuO54wsgsd51NxNrP/XV/7Ub54cvUKzeon+8UKwnGM+BeYkOrxtyjp+N9brwyUmK9UWN9YNM9Qtm//y0U7EU6yUaKlaIkf6O+9tYnwMJz1vzsWdfB83v5QC+y89YitWpgeeOL3jTrxsUS7E6Ak8+/fkbQrE6rHih4AtsrCpxf8aqcD0e9A+fNlYXCF4o+AIbq0rcxqpw2VgpLsVKSdlYFSnFqnDZWCkuxUpJ2VgVKcWqcNlYKS7FSknZWBUpxapw2VgpLsVKSdlYFSnFqnDZWCkuxUpJ2VgVKcWqcNlYKS7FSknZWBUpxapw2VgpLsVKSdlYFSnFqnDZWCkuxUpJ2VgVKcWqcNlYKS7FSknZWBUpxapw2VgpLsVKSdlYFSnFqnDZWCkuxUpJ2VgVKcWqcNlYKS7FSknZWBUpxapw2VgpLsVKSdlYFSnFqnDZWCkuxUpJ2VgVKcWqcNlYKS7FSknZWBUpxapw2VgpLsVKSdlYFSnFqnDZWCkuxUpJ2VgVKcWqcNlYKS7FSknZWBUpxapw2VgpLsVKSdlYFSnFqnDZWCkuxUpJ2VgVKcWqcNlYKS7FSkm9ubHKs9Z+3D+EGUbzrr+lE56z/mOKFUZEixWecfZj5/h9lz/SdNaY8HDFegLqHJgw8Hd97Bw/G+tdasz2KNabGmsWk6/xQsEXQL9uUI0ZATx3fIFizQyAXuO54wsUC1JjNhbPHV+gWDMDoNd47vgCxYLUmI3Fc8cXKNbMAOg1nju+QLEgNWZj8dzxBYo1MwB6jeeOL1AsSI3ZWDx3fIFizQyAXuO54wsUC1JjNhbPHV+gWDMDoNd47vgCxYLUmI3Fc8cXKNbMAOg1nju+QLEgNWZj8dzxBYo1MwB6jeeOL1AsSI3ZWDx3fIFizQyAXuO54wsUC1JjNhbPHV8w+/p9fZWAYl1NbvndirU8oKvnKdbV5JbfrVjLA7p6nmJdTW753Yq1PKCr5ynW1eSW361YywO6ep5iXU1u+d2KtTygq+cp1tXklt+tWMsDunqeYl1NbvndirU8oKvnKdbV5JbfrVjLA7p6nmJdTW753Yq1PKCr5ynW1eSW361YywO6ep5iXU1u+d2KtTygq+cp1tXklt+tWMsDunrebxL2BKZs33jeAAAAAElFTkSuQmCC)

### Drawing paths

we take some extra steps:

1. First, you create the path.
2. Then you use drawing commands to draw into the path.
3. Once the path has been created, you can stroke or fill the path to render it.

```
beginPath()
// Creates a new path. Once created, future drawing commands are directed into the path and used to build the path up.

Path methods
// Methods to set different paths for objects.

closePath()
// Adds a straight line to the path, going to the start of the current sub-path.

stroke()
// Draws the shape by stroking its outline.

fill()
// Draws a solid shape by filling the path's content area.
```

The first step to create a path is to call the `beginPath()`. Internally, paths are stored as a list of sub-paths (lines, arcs, etc.) which together form a shape. Every time this method is called, the list is reset and we can start drawing new shapes.

> Note: When the current path is empty, such as immediately after calling `beginPath()`, or on a newly created canvas, the first path construction command is always treated as a `moveTo()`, regardless of what it actually is. For that reason, you will almost always want to specifically set your starting position after resetting a path.

The second step is calling the methods that actually specify the paths to be drawn. We’ll see these shortly.

The third, and an optional step, is to call `closePath()`. This method tries to close the shape by drawing a straight line from the current point to the start. If the shape has already been closed or there’s only one point in the list, this function does nothing.

> Note: when you call `fill()`, any open shapes are closed automatically, so you don’t have to call `closePath()`. This is not the case when you call stroke().

### Drawing a triangle

```
function draw() {
	const canvas = document.getElementById('canvas');
	if (canvas.getContext) {
		const ctx = canvas.getContext('2d');
		
		ctx.beginPath();
		ctx.moveTo(75, 50);
		ctx.lineTo(100, 75);
		ctx.lineTo(100, 25);
		ctx.fill();
	}
}
```

![img](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAABkCAYAAABw4pVUAAAAAXNSR0IArs4c6QAAAdtJREFUeF7t3MFqwlAURdH45e2nF4UKUq0meS/Zg+VcONzljhPxsnilLnBJrTFmARL7EAABErtAbI5CgMQuEJujECCxC8TmKARI7AKxOQoBErtAbI5CgMQuEJujECCxC8TmKARI7AKxOQoBErtAbI5CgMQuEJujECCxC8TmKKQF8gWkA/K1LMs3kAbIDeM6Bcj5IHcMIDEMIOeCPJTxO8Uj6xyUpxgKiWEAOR7kZRkeWUEMhRyH8rYMhQQxFDIf5eMyFBLEUMg8lNVlKCSIoZDxKJvLUEgQQyHjUHaXoZAghkL2owwrQyFBDIVsRxlehkKCGApZjzKtDIWsx7i+A8i2u01911QUP3LYZjcNBcg2kGmPLyDbQaagANkHMhwFyH6QoShAxoAMQwEyDmQICpCxILtRgIwH2YUCZA7IZhQg80A2oQCZC7IaBch8kFUoQI4B+RgFyHEgH6EAORbkLQqQ40H+RQFyDshLFCDngTxFAXIuyB8UIOeDPKAAaYDcUYB0QG4oQFog/i8r5gEESO0CsT2+Q4DELhCboxAgsQvE5igESOwCsTkKARK7QGyOQoDELhCboxAgsQvE5igESOwCsTkKARK7QGyOQoDELhCboxAgsQvE5igkBvIDtzAxM0G5O0wAAAAASUVORK5CYII=)

### Moving the pen

