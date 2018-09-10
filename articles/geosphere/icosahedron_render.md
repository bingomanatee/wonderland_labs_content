This module renders data for Icosahedrons into PNG files.

![planet_3_sectors.png](/blog_image/planet_3_sectors.png)

the render methods revolve around a planet, which is a simple model of planetary data; largely an extension
of the ?[icosahedron dataset](icosahedron), with the exception that each point must have a color property that is an array of three 0..255 color values. 

The below example shows all the icosahedron-render methods acting in concert. Note that after the data is loaded from `icosahedron`, a color is added; in this case it is a randomized color that marks the sector(s) the point belongs to, as shown above. 

``` javascript

icor = require('icosahedron-render');
var ico = require('icosahedron');
var detail = 4;
ico.io.points(function (err, points) {

    ico.ico.faces(function (err, faces) {
            var point_data = points.points;

            function _c() {
                return Math.floor(Math.random() * 266);
            }

            var sector_colors = _.range(0, 20).map(function (s) {
                return  [_c(), _c(), _c()];
            });

            function _sectors_color(point) {
                if (!point.s) {
                    console.log('bad point: %s', util.inspect(point));
                    throw new Error('bad point');
                }
                var weight = 1 / point.s.length;
                var color = point.s.reduce(function (out, sector) {

                    var add = sector_colors[sector];
                    return out.map(function (channel, c) {
                        return channel + add[c] * weight;
                    })

                }, [0, 0, 0]);
                return color.map(function (value) {
                    return value / point.s.length;
                }).map(Math.floor);
            }

            point_data.forEach(function (point) {
                point.color = _sectors_color(point);
            });

            var planet = new icor.Polysphere(4000, 2000, point_data);
            icor.render_poly(planet, faces.faces, function (err, canvas) {

                icor.canvas_to_file(canvas, path.resolve(OUT_ROOT, 'planet_6_sectors.png'), function () {
                    pl_test.end();
                });
            })
        }
        , detail); // end ico.io.faces
}, detail); // end ico.io.points

```

### Caching render data

Since the 2d coordinates of each hexagon is the same at a given resolution and width/height is the same, you can cache this data as a JSON file; work in progress is integrating this cache with the rendering methods.