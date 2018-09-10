Vertices are the points that define points on a ?[Planet](planet). A vertex is a [Three.js Vector3](http://www.threejs.org) record of a point in space, with `x`, `y`, `z` coordinates. 

In a tessellated sphere the coordinates are all within the range of -1...1. And they are normalized such that they obey the coordinates of a sphere: `sqrt(x^2 + y^2 + z^2) = radius = 1`.&apos;

It also has a property `uv` which is a Vector2 object with `x`, `y` coordinates 0..1. 

Latitude or Longitude can be caluclated from the uv data:

``` javascript

var lat = 180 * (0.5 - vertex.uv.y);
var lon = 360 * vertex.uv.x;

```

For economy, other structures usually just store the ID of a vertex, rather than references to it. 

## Extras

#### planet {Planet}

A reference to the vertex's host [Planet](planet). 

#### data(field, [value?])

gets (or sets ?[vertex_data](planet) for the point. Note the data is not directly stored on the vertex object. 

#### index {int}

a number 0..[number of vertices] that represent their order of creation. Points 0..11 are the original "seed" points. the next 48 ?? points are generated at the next level of tessellation, and so on. 

* * * * * * *

&apos; Due to floating point errors, the points won't be exactly equal to 1, but they will be pretty close.