A network of points is a collection of ?[Vertices](vertices) that represent a specific resolution. The vertices are decorated into nodes which have connections to neighboring nodes, and other use-specific data. 

To generate networks from planets, execute the following code: 

#### generating networks

``` javascript

planet = new Planet();
planet.init_iso(2);
planet.init_networks();

var network_0 = planet.networks[0];
var network_1 = planet.networks[1];
var network_2 = planet.networks[2];
```

Network nodes have a parent-child relationship between nodes at the higher level. There are methods for processing this relationship:

#### network.simplify(iterator [,init] [,fin]) 

Runs a map over all the nodes in the network, calling the iterator with `iterator(node, child)` parameters. The second and third parameters are called before and after the simplify call is executed. 

#### network.inherit(iterator [,init], [fin])

Runs a map over all nodes in the network, calling the iterator with `iterator(node, parent)` parameters. The second and third parameters are called before and after the inherit call is executed.