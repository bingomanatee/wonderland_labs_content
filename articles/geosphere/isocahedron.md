Icosphere (and company) are modules revolving around the processing of global data. 

As is common in Node, what started out as a single closed system is now three (and counting) modules.

## `icosahedron` 

?[icosahedron](icosahedron) is a module that pre processes and provides data on points and sectors. It has a series of scripts that digests a Three.js Icosahedron and saves its' point and face data into JSON data at various ?(levels of detail)[detail]. Currently icosahedron data from level 0..6 have been cached. 

## `icosahedron-render`

?[icosahedron-render](icosahedron_render) is a module that renders icosahedron data out to canvases/PNG files. 

## `icosahedron-data`

?[icosahedron-data](icosahedron_data) uses clusters and zeromq(Ømq) to manage and process planetary data through a series of discrete processes. This takes full advantage of multiprocessors (and potentially, multi-server) systems to distribute the processing of planetary data to all available cpus/servers.