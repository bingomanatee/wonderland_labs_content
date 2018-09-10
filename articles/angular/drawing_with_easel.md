<a href="https://github.com/bingomanatee/easel-paint"><img style="position: absolute; top: 0; right: 0; border: 0;" src="https://s3.amazonaws.com/github/ribbons/forkme_right_red_aa0000.png" alt="Fork me on GitHub"></a>

I have a need to create a drawing program in Canvas/Easel. you can try it yourself at http://wonderlandlabs.com/art/draw. 

![Easel_Paint_sm.png](/blog_image/Easel_Paint_sm.png)

## Project Requirements

* Drawn elements must be listable and editable, as with the layer palette in Adobe programs. 
* Elements will be stored and retrieved through a remote repository. 
* Elements can "compound" -- i.e., an element can be made up of multiple instances of other elements, as in a forest of trees and flowers, each at a different size and shape and potentially with a color tint.

## Technologies

I am welding [Easel.js](http://www.createjs.com/#!/EaselJS), an Adobe Canvas API and Angular.js. 

* The toolbar icon buttons are managed in Angular, as is the permanent storage of drawn elements. 
* The rendering is via Easel. 
* A set of custom "business objects" is attached to Angular through its factory injection coding. 

Most of the "Rock and roll" here is done in Easel/Canvas. However the business classes and the buttons are done in HTML5/Angular. Grunt is used to sew up the directive code. 

The Readme at https://github.com/bingomanatee/easel-paint has other implementation details 

**FAIR WARNING*** this project is under rapid development; the code at Github will most likely reflect a more progressive version of the codebase than this article covers. (the commit as of this writing is 8503ae1274cede02d12bdd4acd159bbe41e35e1e). 

### What is Easel.js

The [Easel.js documentation](http://www.createjs.com/Docs/EaselJS/modules/EaselJS.html) is accurate but it could be a lot more explanatory. Here is a "High level" pass of the object hierarchy of Easel, and the "why" behind it.

![Easel.js.png](/blog_image/Easel.js.png)

Technically speaking, Easel is a single component of a larger suite, "create.js", which Adobe is using to stay viable after Flash. Easel uses a fairly deep system of objects and inheritance. and it takes a while to get the whole picture. 

**Why abstract Canvas?** Easel gives you up front a pre-made way to represent graphics primitives in a nestable, reusable manner. 

* **nestable** Through container nesting, you get to use frame of reference goodness -- rotating, offsetting, scaling, even hiding or showing a host of objects by manipulating their container. 
* **reusable** Rather than writing a draw function that you call over and over to update your canvas, you simply create a tree of objects, tweak them and call `stage.update()` to render the result. 
* **event sensitive** Because there is no DOM in Canvas, Easel "Fakes" a dom, allowing you to attach click events to "virtual shapes" in the canvas.

Easel adds all sorts of goodness like rotoscoping sprites, caching, bitmap filters and similar tools. Many of these things can of course be directly had coded, but in general you get things done a lot faster by reusing Easel than you would starting from zero. 

#### createjs.Stage

the `Stage` class gives Easel access to drawing to an HTML canvas. While you can map a Stage instance to an in-page canvas (and must, to do anything visible to the reader), you can also use stages on "private/buffer" canvases, as you might want to do to work with video, or create textures in three.js. 

To create a stage, just call `new createjs.Stage(canvasElement)`. You must pass it a raw dom element -- not a jQuery object. 

The methods of stage you will use most often is `update()`, which updates the canvas with whatever combinations of shapes and containers you have in your Stage at the moment, and `addChild(displayObject)` which adds a new element to the stage's children. 

**A note on drawing in Easel and Canvas** Canvases are at the most basic level bitmaps; they allow you to paint new data into them, but they don't instantiate any permanent collection of objects as part of the drawing process. Drawing at its most fundamental is simply changing numbers in an array, and there is no "magic" linking or memory going on between one element of the array and another.  

Easel creates that "memory" by retaining the collection of objects it draws, and the order in which they are drawn. That way, when you alter one of the objects by moving it to the right, you don't have to take care to "erase" it yourself from the canvas; the `stage.update()` command will by default, erase the canvas, then redraw every object in sequence, with its current attributes. 

You can, if you wish, wrest control of this process. If for instance you want to draw new objects OVER the current canvas , you can set `stage.autoClear` to false. that way, drawn objects will leave "trails" of themselves as they move. 

For the purposes of this demo, however the clear and redraw process that is native to Easel is the desired behavior. 

** DisplayObject Layering in the Stage: ** Objects are drawn in the order that they occur in the "children" collection of the stage (recursively for child collections). There are ways to re-order children (see the [Container](http://www.createjs.com/Docs/EaselJS/classes/Container.html) documentation for details), but the easiest way to control layering is to add a series of stage as child objects, then add shapes to *those* containiers. If you have "container A" and "container B" added to the stage (in that order), you can add shapes to both containers and be assured that no matter what order they are added in, "container A"'s shapes will always appear beneath "container B"'s shapes.

#### createjs.Container

A container is essentially a collection of DisplayObjects. `DisplayObject` is the parent of all things visual in Easel.js; it is the base class for `Stage`, `Shape`, `Container`, `Bitmap` and a whole lot more. Containers can contain other containers for deep nesting. 

Container (which is `Stage`'s base class) adds parent/child behavior to the mix; it is itself an ordered collection, with the usual `addChild(displayObject)`, `children`, `parent` properties/methods.

It is worth noting that it is more than an abstract collection; as a DisplayObject child class, it has position(`x`, `y`), scale(`scaleX`,`scaleY`), `rotation`, `alpha`, `visible`, and all the usual goodness. So you can use Containers to move, rotate and scale all its' child objects at once. That is, it is itself a frame of reference. 

At the lest, this lets you orient all its child shapes to the 0,0 space, and position the group using the containers' x and y properties.

### Shapes and Graphics

The Shape represents a collection of drawing commands. Any number of drawing commands can be issued from the same shape. that is, you can draw a blue circle, a red rectangle, some text, and a gradient polygon. 

This is executed through the shapes' `graphics` property. The Graphics property can be injected through the `new Shape(graphics)` constructor pattern but it is a lot simpler to just instantiate `new Shape()`, which will have a graphics property created for it on the fly. 

Drawing is accomplished through calling various draw primitive methods of the graphics property. 

The below code is a routine I use to draw the "little black boxes" around my graphics sprites. 

``` javascript


 var shape = new createjs.Shape();
 this.box_container.addChild(shape); 
// this == an outer class instance I use to organize my drawing program.
//...
 shape.graphics.f('rgba(0,0,0,0.66)').r(0, 0, HANDLE_SIZE, HANDLE_SIZE).es();

```

Note that **three things** must be done right to view a shape

1. It must be added to a stage, or to a collection that has been added to a stage, through the `addChild(my_shape)` method. (`my_stage.addChild(my_shape)`). 
2. Some combination of shape.graphics methods must have been called to give you something to see. Specifically one or more cycles: &ast;
    * a `beginFill`[`f`] or `beginStroke`[`s`] command with a valid color string
    * one or more `rect`[`r`], `moveTo`[`mt`], `lineTo`[`lt`] or other "shape making" commands
    * optionally an `endFill`[`ef`] or `endStroke`[`es`] command must be issued; Easel will automatically close shapes/strokes, but if you draw more than one shape or stroke you have to close the previous one before beginning the new one. 
3. the `stage.update()` command must be called to trigger a re-painting of the canvas.

&ast; There are of course many more ways to paint using the Graphics class, but the "beginFill/Stroke, draw, endFill/Stroke" is a very common pattern. 

## The task at hand: creating a drawing program

![paintUML.png](/blog_image/paintUML.png)

A drawing program in general has three (or more) layers. 

1. A **background** layer -- grids, guides, rulers, and other "reference" visual elements. In this case we will have a simple grid. 
2. A **drawing** layer. This is where we will add drawn elements to. 
3. A set of **control** layers. This is where you add "helper" tools to the active element:
   * draw boxes for resizing draw objects
   * polygon overlays for point manipulation
   * a color palette layer for choosing a color from a canvas based dialog

The classes we'll use to manage these elements will be a directive (`paintEditor`) to draw the canva and control buttons, and two business objects, `Paint_Manager` and `Point_Manager_Shape`. 

### Paint_Manager

Paint_Manager creates and maintains the stage and a list of shapes, represented by Point_Manager_Shape instances.

The constructor shows the primary structural elements of the paint program:

``` javascript

       function Paint_Manager(params) {
                this.scope = params.scope;
                var canvas = $(params.ele).find('canvas.paint-canvas')[0];

                this.canvas = canvas;
                canvas.width = this.screen_width();
                canvas.height = this.screen_height();

                this.stage = new createjs.Stage(canvas);
                this.shapes = [];

                this.make_grid();
                var self = this;
                snap = function (n, f) {
                    if (f) return n[f] = snap(n[f]);

                    return n - (n % self.grid_size());
                };

                this.make_frame();

                this.make_draw_container();

                Paint_Manager_Boxes(this);

                this.add_button_bindings();

                this.add_form_bindings();

                this.palette = Color_Palette(this, this.margin() * 2, this.margin() * 2,
                    this.screen_width() - (4 * this.margin()),
                    this.screen_height() - (4 * this.margin())
                );

                this.init_leap();

                this.update();
            }

```

## Class composition through injection

The injection of Angular, while obtuse, gives a good mechanism for loading compositional objects. For instance, the manager class (itself a factory) relies on other factories to supply its' dependant components: 

``` javascript

(function (window) {

    var DEFAULT_SCREEN_WIDTH = 500;
    var DEFAULT_SCREEN_HEIGHT = 300;
    var DEFAULT_GRID_SIZE = 20;
    var DEFAULT_SCREEN_MARGIN = 50;

    angular.module('Paint').factory('Paint_Manager',
        function (Paint_Manager_Grid, Paint_Manager_Shape, Paint_Manager_Polygon, Paint_Manager_Boxes, Paint_Manager_Leap, Color_Palette) {

            function Paint_Manager(params) {
                this.scope = params.scope;
                var canvas = $(params.ele).find('canvas.paint-canvas')[0];

                this.canvas = canvas;
                canvas.width = this.screen_width();
                canvas.height = this.screen_height();

                this.stage = new createjs.Stage(canvas);
                this.shapes = [];

                Paint_Manager_Grid.call(this);

...
            }

            Paint_Manager.prototype = {
            
...

            };

            return function (scope, ele) {
                return new Paint_Manager({scope: scope, ele: ele});
            }

        })
})
    (window);

```

note that the function that is returned by the factory is a proxy for the constructor of the Paint_Manager class, contained in proxy. 

The definition of Paint_Manager_Grid is contained in another factory:

``` javascript
(function (window) {

    var GRID_COLOR = 'rgba(0,0,0, 0.25)';

    angular.module('Paint').factory('Paint_Manager_Grid', function () {

        return function () {
            console.log('making grid');

            var width= this.screen_width(true);
            var height = this.screen_height(true);
            var grid_size = this.grid_size();

            console.log('width: ', width, 'heght:', height, 'grid_size:', grid_size);
            var grid_shape = new createjs.Shape();
            grid_shape.x = grid_shape.y = this.margin();
            var g = grid_shape.graphics.s(GRID_COLOR).ss(1);

            for (var x = 0; x <= width; x += grid_size){
                g.mt(x, 0).lt(x, height);
            }

            for (var y = 0; y <= height; y += grid_size){
                g.mt(0,y).lt(width, y);
            }

            g.es();

            this.stage.addChild(grid_shape);
            this.stage.update();

        }

    })
})(window);

```