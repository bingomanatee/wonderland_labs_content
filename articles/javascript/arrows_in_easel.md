Everything is harder than it looks. Take the humble arrow. Just a triangle on a box. How hard can it be?

Well, unless you're a trig maschoist, very. Unless you have a kind framework like Easeljs. 

<iframe width="100%" height="300" src="//jsfiddle.net/bingomanatee/89q3mxgb/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>
If you want to view the entire project on a seperate tab, <a href="https://jsfiddle.net/bingomanatee/89q3mxgb/" target="arrows">Click Here</a>.

The shape object has a few useful properties that this fiddle showcases: 

1. You can move and rotate the shape which is the container for all the inner graphics tactical calls
2. You can set the opacity (alpha) for the entire shape through a global property (even hiding it entirely)
3. You can hook mouse events to the stage in a jQuery-like style. 
4. You synchronize updates to the stage when you want, via stage.update()
5. You can save previous drawing in a shape object so you don't have to re-call or re-compute every graphics step every frame. 

## Setting up Easel

There are two ways to start Easel (or for that matter any canvas stuff): 

1. capture a pre-existing canvas from the document and draw into it
2. add your own canvas element

The second method has the advantage of allowing us to resize the canvas, in this case to cover the full window, and thats what we are doing here:

``` javascript

var canvas = document.createElement("canvas");
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;
canvas.id = 'testCanvas';
var title = document.getElementById('title');
document.body.insertBefore(canvas, title);

canvas = document.getElementById('testCanvas');

var stage = new createjs.Stage(canvas);
var background = new createjs.Shape();
background.graphics.f('rgba(255,255,255,0.01)').r(0, 0, canvas.width, canvas.height);
stage.addChild(background);
stage.update();

```

we are in this case putting the canvas behind (both visually and in DOM order) a title block with some documentation.

## Drawing the Arrow

You really don't want to have to figure out the trig for what an arrow is like when it is rotated. Better to just draw it oriented to the right and rotate it from the start to the end point. Fortunately the shape has all the hooks for this: 

``` javascript

                var start = lastItem.point;
                var end = item.point;
                var arrow = new createjs.Shape();
                var arrowSize = _distance(start, end);
                var arrowRotation = _angle(start, end);
                arrow.graphics.s(item.lineColor())
                        .f(item.fillColor())
                        .mt(0, 0)
                        .lt(0, LINE_RADIUS)
                        .lt(arrowSize - ARROWHEAD_DEPTH, LINE_RADIUS)
                        .lt(arrowSize - ARROWHEAD_DEPTH, ARROWHEAD_RADIUS)
                        .lt(arrowSize, 0)
                        .lt(arrowSize - ARROWHEAD_DEPTH, -ARROWHEAD_RADIUS)
                        .lt(arrowSize - ARROWHEAD_DEPTH, -LINE_RADIUS)
                        .lt(0, -LINE_RADIUS)
                        .lt(0, 0)
                        .es();
                arrow.x = start.x;
                arrow.y = start.y;
                arrow.alpha = 1;
                arrow.rotation = arrowRotation;
                item.arrow = arrow;
                stage.addChild(item.arrow);

```

Not shown are the classic two-point distance calculations and the (degree) rotation function. As you can see you can use comparatively simple orthogonal math to draw the arrow. (not done but probably needed for production is to compensate for very short arrows, such that the arrowhead is behind the start point.) Then the canonical arrow is spun towards the target point. 

## Creating the point records. 

To make it somewhat interactive, I am letting the user click on the stage to set the from-to points of the arrow. They will be chained end-to-end so I save point by point data into a global array. The point data is a custom class which also manages to keep the colors from one arrow to another in the same general range. 

``` javascript

function _color(r, g, b) {
    r = Math.floor(r);
    g = Math.floor(g);
    b = Math.floor(b);
    return 'rgb(' + r + ',' + g + ',' + b + ')';
}

var COLOR_VARIANCE = 25;
/**
 * if an argument is passed in, returns a midtone in the range of 55..105
 * otherwise, returns a value from 0 to 255 in increments of COLOR_VARIANCE
 */
function _hue(past){
    if (arguments.length){
        // note - values below 20 are "bounced up" to prevent black arrows
        if (past < 20 || Math.random() > 0.5){
            return Math.min(255, past + COLOR_VARIANCE);
        } else {
            return Math.max(0, past - COLOR_VARIANCE);
        }
    } else {
        return Math.min(255, 55 + Math.floor(Math.random() * 150));
    }
}

function PointData(event, last) {
    this.point = new createjs.Point(event.rawX, event.rawY);
    if (last) {
        this.r = _hue(last.r);
        this.g = _hue(last.g);
        this.b = _hue(last.b);
    } else {
        this.r = _hue();
        this.g = _hue();
        this.b = _hue();
    }
}

PointData.prototype.fillColor = function () {
    return _color(this.r, this.g, this.b);
}

PointData.prototype.lineColor = function () {
    return _color(this.r / 2, this.g / 2, this.b / 2);
}

stage.addEventListener('click', function (event) {
    var last = data.length ? data[data.length - 1] : null;
    var newData = new PointData(event, last);
    data.push(newData);
    drawArrows();
    stage.update();
});
```

## Pushing graphics to the stage

the full drawArrows() script creates a single arrow shape and adds it to the stage:

``` javaScript

function drawArrows() {
    var lastItem = null;
    for (var i = 0; i < data.length; ++i) {
        var item = data[i];
        if (lastItem && item) {
            if (!item.arrow) {
                var start = lastItem.point;
                var end = item.point;
                var arrow = new createjs.Shape();
                var arrowSize = _distance(start, end);
                var arrowRotation = _angle(start, end);
                arrow.graphics
                        .s(_color())
                        .f(_color())
                        .mt(0, 0)
                        .lt(0, LINE_RADIUS)
                        .lt(arrowSize - ARROWHEAD_DEPTH, LINE_RADIUS)
                        .lt(arrowSize - ARROWHEAD_DEPTH, ARROWHEAD_RADIUS)
                        .lt(arrowSize, 0)
                        .lt(arrowSize - ARROWHEAD_DEPTH, -ARROWHEAD_RADIUS)
                        .lt(arrowSize - ARROWHEAD_DEPTH, -LINE_RADIUS)
                        .lt(0, -LINE_RADIUS)
                        .lt(0, 0)
                        .es();
                arrow.x = start.x;
                arrow.y = start.y;
                arrow.alpha = 1;
                arrow.rotation = arrowRotation;
                item.arrow = arrow;
                stage.addChild(item.arrow);
            }
        }
        lastItem = item;
    }
}

```

note the reverse for loop is probably waste as only a single arrow must be created in the current use case but its a good safety anyway.

## Adding some animation

Good but still a bit boring. Perhaps a little rotoscoping?

``` javascript


var onIndex = 0;
setInterval(function () {
    var alpha = 1;
    var orderedArrows = [];
    var increment = 0.25;
    if (data.length < 5) return;
    
    for (var i = onIndex; i >= 0; --i) {
        if (data[i] && data[i].arrow) {
            orderedArrows.push(data[i].arrow);
        }
    }
    for (var j = data.length - 1; j > onIndex; --j) {
        if (data[j] && data[j].arrow) {
            orderedArrows.push(data[j].arrow);
        }
    }

    for (var k = 0; k < orderedArrows.length; ++k) {
        var arrow = orderedArrows[k];
        if (arrow) {
            arrow.alpha = Math.max(0, alpha);
            alpha -= increment;
        }
    }
    ++onIndex;
    if (onIndex >= data.length) {
        onIndex = 0;
    }
    stage.update();
}, 50);

```

basically onIndex moves the "head of the snake" and the rest of the arrows are snaked through in circular list fashion. I found in practice it was kind of garish until there were at least five arrows. It looks really cool if you add a whole bunch of arrows.

## The full script

Here is the complete script:

``` javascript

var canvas = document.createElement("canvas");
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;
canvas.id = 'testCanvas';
var title = document.getElementById('title');
document.body.insertBefore(canvas, title);

canvas = document.getElementById('testCanvas');

var stage = new createjs.Stage(canvas);
var background = new createjs.Shape();
background.graphics.f('rgba(255,255,255,0.01)').r(0, 0, canvas.width, canvas.height);
stage.addChild(background);
stage.update();

var data = [];

function _shade() {
    return Math.floor(Math.random() * 255);
}

function _distance(from, to) {
    var dx = from.x - to.x;
    var dy = from.y - to.y;
    return Math.sqrt(dx * dx + dy * dy);
}

function _angle(p1, p2) {
    return Math.atan2(p2.y - p1.y, p2.x - p1.x) * 180 / Math.PI;
}

function _color(r, g, b) {
    r = Math.floor(r);
    g = Math.floor(g);
    b = Math.floor(b);
    return 'rgb(' + r + ',' + g + ',' + b + ')';
}

var COLOR_VARIANCE = 25;
/**
 * if an argument is passed in, returns a midtone in the range of 55..105
 * otherwise, returns a value from 0 to 255 in increments of COLOR_VARIANCE
 */
function _hue(past){
    if (arguments.length){
        // note - values below 20 are "bounced up" to prevent black arrows
        if (past < 20 || Math.random() > 0.5){
            return Math.min(255, past + COLOR_VARIANCE);
        } else {
            return Math.max(0, past - COLOR_VARIANCE);
        }
    } else {
        return Math.min(255, 55 + Math.floor(Math.random() * 150));
    }
}

var LINE_RADIUS = 10; // the short radius of the "line box"
var ARROWHEAD_RADIUS = 25; // the arrowhead radius;
var ARROWHEAD_DEPTH = 30;

function drawArrows() {
    var lastItem = null;
    for (var i = 0; i < data.length; ++i) {
        var item = data[i];
        if (lastItem && item) {
            if (!item.arrow) {
                var start = lastItem.point;
                var end = item.point;
                var arrow = new createjs.Shape();
                var arrowSize = _distance(start, end);
                var arrowRotation = _angle(start, end);
                arrow.graphics.s(item.lineColor())
                        .f(item.fillColor())
                        .mt(0, 0)
                        .lt(0, LINE_RADIUS)
                        .lt(arrowSize - ARROWHEAD_DEPTH, LINE_RADIUS)
                        .lt(arrowSize - ARROWHEAD_DEPTH, ARROWHEAD_RADIUS)
                        .lt(arrowSize, 0)
                        .lt(arrowSize - ARROWHEAD_DEPTH, -ARROWHEAD_RADIUS)
                        .lt(arrowSize - ARROWHEAD_DEPTH, -LINE_RADIUS)
                        .lt(0, -LINE_RADIUS)
                        .lt(0, 0)
                        .es();
                arrow.x = start.x;
                arrow.y = start.y;
                arrow.alpha = 1;
                arrow.rotation = arrowRotation;
                item.arrow = arrow;
                stage.addChild(item.arrow);
            }
        }
        lastItem = item;
    }
}

function PointData(event, last) {
    this.point = new createjs.Point(event.rawX, event.rawY);
    if (last) {
        this.r = _hue(last.r);
        this.g = _hue(last.g);
        this.b = _hue(last.b);
    } else {
        this.r = _hue();
        this.g = _hue();
        this.b = _hue();
    }
}

PointData.prototype.fillColor = function () {
    return _color(this.r, this.g, this.b);
}

PointData.prototype.lineColor = function () {
    return _color(this.r / 2, this.g / 2, this.b / 2);
}

stage.addEventListener('click', function (event) {
    var last = data.length ? data[data.length - 1] : null;
    var newData = new PointData(event, last);
    data.push(newData);
    drawArrows();
    stage.update();
});

var onIndex = 0;
setInterval(function () {
    var alpha = 1;
    var orderedArrows = [];
    var increment = 0.25;
    if (data.length < 5) return;

    for (var i = onIndex; i >= 0; --i) {
        if (data[i] && data[i].arrow) {
            orderedArrows.push(data[i].arrow);
        }
    }
    for (var j = data.length - 1; j > onIndex; --j) {
        if (data[j] && data[j].arrow) {
            orderedArrows.push(data[j].arrow);
        }
    }

    for (var k = 0; k < orderedArrows.length; ++k) {
        var arrow = orderedArrows[k];
        if (arrow) {
            arrow.alpha = Math.max(0, alpha);
            alpha -= increment;
        }
    }++onIndex;
    if (onIndex >= data.length) {
        onIndex = 0;
    }
    stage.update();
}, 50);

```