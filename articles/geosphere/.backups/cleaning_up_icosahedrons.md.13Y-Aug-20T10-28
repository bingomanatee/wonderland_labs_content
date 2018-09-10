One of the goals is for geosphere Icosahedrons to be resolution independent -- that is, for each point to either map directly to a parent/child, or to be mappable to a set of parents.

For this to be true, the identity of each point has to map between high and low resolution geometries. That is, two points with the same index, such as the 12 original cardinal points, should have the same coordinates. 
At one point this was true, but due to the newer algorithm Three.js uses to pop out Icos, there is no longer a direct relationship between every point and its index. 

I have created a remapping module to solve this task by assigning a "real_order" to each point and re-scrambling the points to be properly ordered. (this is a work in progress). 

I am saving the data to Mongo, then to file (JSON, Binary) as well as organizing the points by sector in the hopes that  the sectors can be computed in parallel.