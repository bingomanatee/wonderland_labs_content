<a href="https://github.com/bingomanatee/easel-map"><img style="position: absolute; top: 0; right: 0; border: 0;" src="https://s3.amazonaws.com/github/ribbons/forkme_right_red_aa0000.png" alt="Fork me on GitHub"></a>

I am doing a hex-based mapping system in Easel.js and by using the "secrets of Easel" I achieved major fastness. Here's how YOU can achieve MAJOR FASTNESS in three easy steps.

![hexes.png](/blog_image/hexes.png)

## Cache only

The first thing to understand about easel is that each shape or container has the option to cache itself. When cached, a shape/container stores a snapshot of the drawn shape, rather than redrawing, operation by operation, the graphic every time the stage is updated. 

To cache the shape you have to know the boundary of the shape's extent. Fortunately this will be in the local coordinate system. However because of this, you may risk saving more pixels than you need, if the shape is scaled down. I.e., if you have a 500x500 square that is scaled by 0.2, a 10,000 pixel cache is a waste. So you can pass a fourth parameter, scale, which will save a lower / higher res as necessary, cache of the shape. 

The full cycle looks something like this:

``` javascript
 var extent = Math.ceil(this.hex_size) + 1;
 shape.cache(-extent, -extent, 2 * extent, 2 * extent, scale);
 return shape;
```

## Commence the cheating

So, if you have a huge grid and want to update it cell by cell, you could cache every cell individually. This is "better than nothing" but creates a lot of identical canvas references. 

What you can do instead is create a few reference cells (one for each color, say) and use their caches as a source for bitmaps. This means each cell will simply reference a much smaller subset of bitmaps, and this puts a real shot in the arm for your render times. 

``` javascript

        _make_hex: function (color) {
            var shape = new createjs.Shape();
            shape.graphics.f(color);
            var points = this.calc_points();

            shape.graphics.mt(_.last(points).x, _.last(points).y);
            _.each(points, function (point) {
                shape.graphics.lt(point.x, point.y);

            });

            var extent = Math.ceil(this.hex_size ) + 1;

            shape.cache(-extent, -extent, 2 * extent, 2 * extent);
            CACHED_HEXES[color] = shape.cacheCanvas;
        },

        fill: function (container, color, shape, scale, cache) {

            if (!CACHED_HEXES[color]) {
                this._make_hex(color);
            }

            var refresh = !shape;
            if (refresh) shape = new createjs.Bitmap(CACHED_HEXES[color]);
            shape.x = this.center_x() - (this.hex_size );
            shape.y = this.center_y() - (this.hex_size);
         //   shape.scaleX = shape.scaleY = 1/scale;
            container.addChild(shape);

            var extent = Math.ceil(this.hex_size) + 1;
            return shape;
        },
```

note - in this particular instance I did NOT pass scale to `cache(...)` because I want the bitmap to be properly scaled for the scene; I'd rather have a few extra pixels than have to do a lot of math to bring things into proper orientation -- though this might change. 

Also, note that I am using the cache layer of the platonic hexes as source for a new bitmap reference. This is a very powerful tool for transforming canvases to reusable sprite sheets, stamps, etc. These canvases can even have stages of their own.

## Filling in the gaps

At this point the FPS is better but still around 1-2 frames a second. This meant as I "Painted" color on the hex grid, there were major gaps (2-5 hexes) between the panted hexes even at a fairly coarse resolution. 

So I started calculating all the hexes between the last two hexes and painting "Hex lines" between them using a recursive midpoint calculator.

## DIY mouse events

Even with sprite optimization, the native event system of Easel is not ideal for a very busy environment. with several thousand hexes, using sprite-based mouseover detection gave around 2 to 0.5 FPS. 

However using exact mouse detection is not really necessary when you have near-circular sprites. So what I did was, instead, used a two-teir system in which I captured a native jQuery event and from there, found the closest *group* of hexes (Having found their rough boundaries for cache creation already); then used a simple map/reduce to find the nearest cell in the group based on the event's offsetX/Y and the cell's center. &ast;

At that point the FPS sprang up to 10-12 frames or so -- in any event, it did not seem like I could move my mouse fast enough in normal operation to create "gappy" painting at a coarse resolution. 

## The result

The drawing process is pretty damn fast, even with 6,000 hexes on the screen. It does get fairly sluggish at 16,000 hexes but thats partly because I'm redrawing the whole matrix with every click.

It can be seen at [this page](http://www.wonderlandlabs.com/art/hex)

-----------

&ast; For even more amazing speed I could have organized the cells into rows and columns and found the nearest hex using fuzzy indexing.