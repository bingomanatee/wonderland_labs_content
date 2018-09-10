Icosahedron takes and caches data for a fractalized dodecahedron Based on [this document](http://www.esrl.noaa.gov/research/themes/global/pdf/IcosahedralGrid.pdf), the Icosahedron data is an ideal way to represent and model planetary force data. Unlike the traditional "lat/lon" grid, you don't end up wasting a lot of resources on polar data just because 360&deg; is 360&deg, whether at the pole or at the equator. 

The Icosahedrons can be sliced in several ways. 

## By detail

Icosahedral data can be fractalized to many different ?[levels of detail](detail). At the basic level of detail(zero) you have 12 points and 20 faces. At the highest level of detail stored, (6), you have 40,962 points, each about 126 km from each other. 

## By sector

The points are grouped into sectors based on the original 20 sectors; that is, at any level of resolution, each point belongs to one or more sectors.&ast; 

The advantage of sectors is that you can run a simulation on a single sector at any depth you want, and only have to process 1/20th of the planets data at once; so even at six levels of detail, you are processing c. 2,050 points at once. (and you could potentially fractalize that data locally if higher resolution is needed. 

## Data methods

The point data is stored as a JSON data block. 

The data can be accessed via callback functions on the module: 

``` javascript 
var ico = require('icosahedron');
   ico.io.points(function(err, points){ 
   var point_data = points.points;
... }, depth, sector)
```

for each point the following fields are available:

* **ro**: the "real order" of the point
* **c**: the float coordinates of the point [x, y, z]. This is a "normalized" vector with radius of 1, so each value is in the -1... 1 range.
* **uv**: the "uv" mapping of the point[u, v] in 0..1 float.
* **n**: the ro ids of the neighboring points. the cardinal 12 points have 5 neighbors, all other points should have six neighbors. 
* **s**: the sector ids; 1-5 ints in the 0..19 range. 

The original (detail 0) data is: 

``` json

{"points": [
    {
        "ro": 0,
        "c": [-0.5257311121191336, 0.85065080835204, 0],
        "uv": [0.5, 0.8237918088252166],
        "n": [1, 10, 11, 5, 7],
        "s": [4, 0, 1, 2, 3]
    },
    {
        "ro": 1,
        "c": [0.5257311121191336, 0.85065080835204, 0],
        "uv": [1, 0.8237918088252166],
        "n": [0, 5, 7, 8, 9],
        "s": [9, 1, 5, 2, 19]
    },
    {
        "ro": 2,
        "c": [-0.5257311121191336, -0.85065080835204, 0],
        "uv": [0.5, 0.17620819117478337],
        "n": [10, 11, 3, 4, 6],
        "s": [11, 16, 7, 12, 17]
    },
    {
        "ro": 3,
        "c": [0.5257311121191336, -0.85065080835204, 0],
        "uv": [1, 0.17620819117478337],
        "n": [2, 4, 6, 8, 9],
        "s": [14, 10, 11, 12, 13]
    },
    {
        "ro": 4,
        "c": [0, -0.5257311121191336, 0.85065080835204],
        "uv": [0.75, 0.32379180882521663],
        "n": [11, 2, 3, 5, 9],
        "s": [6, 10, 15, 11, 16]
    },
    {
        "ro": 5,
        "c": [0, 0.5257311121191336, 0.85065080835204],
        "uv": [0.75, 0.6762081911747834],
        "n": [0, 1, 11, 4, 9],
        "s": [0, 6, 15, 1, 5]
    },
    {
        "ro": 6,
        "c": [0, -0.5257311121191336, -0.85065080835204],
        "uv": [0.25, 0.32379180882521663],
        "n": [10, 2, 3, 7, 8],
        "s": [12, 17, 8, 13, 18]
    },
    {
        "ro": 7,
        "c": [0, 0.5257311121191336, -0.85065080835204],
        "uv": [0.25, 0.6762081911747834],
        "n": [0, 1, 10, 6, 8],
        "s": [9, 2, 3, 8, 18]
    },
    {
        "ro": 8,
        "c": [0.85065080835204, 0, -0.5257311121191336],
        "uv": [0.08810409558739168, 0.5],
        "n": [1, 3, 6, 7, 9],
        "s": [9, 14, 13, 18, 19]
    },
    {
        "ro": 9,
        "c": [0.85065080835204, 0, 0.5257311121191336],
        "uv": [0.9118959044126083, 0.5],
        "n": [1, 3, 4, 5, 8],
        "s": [14, 10, 15, 5, 19]
    },
    {
        "ro": 10,
        "c": [-0.85065080835204, 0, -0.5257311121191336],
        "uv": [0.4118959044126083, 0.5],
        "n": [0, 11, 2, 6, 7],
        "s": [4, 7, 17, 3, 8]
    },
    {
        "ro": 11,
        "c": [-0.85065080835204, 0, 0.5257311121191336],
        "uv": [0.5881040955873917, 0.5],
        "n": [0, 10, 2, 4, 5],
        "s": [4, 0, 6, 16, 7]
    }
]}

```

&ast; (specifically: the original 12 points belong to 5 sectors each, being the corners of the original faces; any higher resolution points belong to either two different sectors, as they are on the border of the sectors, or only one sector.