Planets combine a Three.js icosahedron and metadata for each point. It also stores arbitrary metadata around each vertex, and provides easy access to each vertice's neighbor. 

The Planet's vertices are Three.js Vector3 ... with a few extra features. 
Each vertex has
* a planet property for self-reference
* an `index` (positive int} 
* a `data(key, [value?])` method that is a hook to `Planet.vertex_data(..)` described below. 

## Efficiency / Data Storage

At this point, all the data for planet vertices are stored in a local, in-memory hash. In the future, as storage/scale increases, it will probably be necessary to shim in a formal data store for data storage in place of the memory store for planets. 

## Hammering things into hexes

The current planet is designed to store all data indexed by vertex. At this point the data is being reduced to a single scalar value (or in a few places, an array of scalars) for that vertex; however, if there is need to store detailed data at a higher resolution than the planet's vertices provide, you can always store original data with original coordinates in each vertex via `Planet.vertex_data_push(index, key, value)` with object hashes for values. 

``` javascript
var GeoSphere = require('GeoSphere');

var planet = new Planet(3);

var data = [
  {lat: 12.20, lon: -20.5, value: 2},
   {lat: 100, lon: 50, value:3} ...

];

data.forEach(function(item){
    var point = GeoSphere.util.lat_lon_to_Vector3(item.lat, item.lon, true);
    var vertex = planet.closest_point(point);
    planet.vertex_data_push(vertex.index, item);
});

```

## API

### Constructor

#### Planet(depth) 

creates a planet tessellated to the number of iterations passed in as the constructor
They have the following properties: 

#### iso [Three.js icosahedron]

#### Planet.vertices(index: boolean) [{Three.js Vector3}]

a function that returns the vertices(Vector3); each vertex has an index property (int) and a uv (Vector2) sub-property. 
Note the vertices are normalized, regardless of the planet's desired radius. 

if index == true, returns an array of ints `[0..(#vertices - 1)]`. 

#### Planet.vertex(index: int) 
returns a single vertex. 

#### Planet.vertices(fn|boolean)
there are three uses for this method:

1. Get the indexes of all the vertices (by passing `true`).
2. Get all the vertices as an array (no arguments)
3. To run a "map" function over each vertex(by passing a function). 

### Data related functions
Vertex data is stored in a separate data collection than the iso. There are many methods for getting/setting vertex data:

#### vertex_data(index, key, [value?])
if two arguments are passed in, returns the value of vertex data stored for a given vertex. If three arguments are passed in, sets the value for a given key and index. 

``` javascript

var planet = new Planet(0); // will have 12 points

planet.vertex_data(0, 'elevation', 500);
var ele_0 = planet.vertex_data(0, 'eleation') // == 500.

```

#### vertex_data_push(index, key, value)
Pushes a value into an array stack. NOTE: if the current data for the key is NOT an array it is LOST. 

#### Planet.vertex_data_all(key, value)

Assigns the same value to each vertex for a given key. Warning: do not pass in objects or arrays because each vertex will have the same literal object.

### Drawing a planet

#### Planet.draw_triangles(h, w, cb) | Planet.draw_triangles(h, w, file_path, cb)

Reads the 'color' data value for each vertex, which can be 

* a single value
* an array of three values `0..1`
* an array of four values `0..1`. 
* a Three.js Color object

This method is __extremely fast__ for planets where the amount of visual space for each point is big enough that you can see the hexagonal pattern. 

note - there are two permutations to this method. One passes the canvas to the callback (for further modification), and the other draws the image to the filesystem directly. 

#### Planet.draw_map(h, v, cb) 

Draws the poles; because of the way the triangle map algorithm works, it leaves strange gaps at the poles, that this method complements. This is an internal method that serves to complement draw_triangles for the draw method. 

#### Planet.draw(h, v, cb) | Planet.draw(h, v, file_path, cb);

Blends draw_triangles and draw_map. Like draw_triangles, can pass s canvas to the callback, or draw the canvas directly.