There are three schools of thought when it comes to bundling Angular project files for production. (four, if you count "bring in a bunch of separate files and load them all at once" which really gives you unacceptable performance; which I don't. ) 

1. unify them all up with gulp/grunt in a single minified file.
2. Use require.js to bring in files as you need them
3. Do the same with Webpack (which I've not used but hear good things about). 

When your project grows to the point where 1 seems unacceptable, 2 or 3 are the options to consider. I'm going to outline 2, and focus on specifically the relationship between Angular and Require.js' module systems. 

## Separate and Equally Fussy

A module system, in abstract, does several things:

1. Maps a code component to a file, 
2. Assigns the file a unique label. 
3. Loads that resource into memory when it's requested by label.
4. Does this recursively to include any dependencies. 

### Angular's module definition

One of the most taxing things about angular's module system is that the syntax for **defining** a module is *very similar to* the syntax for **accessing that module** later on. One of them -- the module definition -- can only be done once. 

When you define a module in Angular you set it's name, and its dependencies, as such: 

``` javascript

angular.module('clowncar', ['ngCookies', 'ngBootstrap', 'ngResource']);

```

The most common place people see modules is the root sandbox -- or application definition, associated with `<div ng-app="circus">...</div>` which is the big giant collection of resources that make up your application as a whole. Its good to note here that: applications are modules, but not all modules are applications. 

Modules in angular are the fundamental system that glues Angular together. Modules are how you group specific resources for export to other systems. Some modules are written by the angular team to solve specific tasks; some are bundled with the core angular.js file. Some bring in other tools like cookies services, REST access (through resource), or third party utilities (like the angular bootstrap libraries or the ui router tools. 

If you haven't used require, you might be familiar with writing modules; that is, writing one per application. you write "circusApp" once, then bring in other services under that tent and don't divide your app up any further. 

Or you can go to the other extreme and make every resource (service, factory, etc.) attached to its own module, then define your circusApp to use those modules as in: 

``` javascript

angular.module('clownCarModule', []).service('clownCar', ...);
angular.module('clownRestModule, ['ngResource']).service('clownRest', ...);
....

angular.module('clownApp', ['clownCarModule', 'clownRestModule'...]);
```

## Require.js

Require.js is an implementation of the CommonJS API, named after its inventer, rapper Common, who developed it in his website. <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/9.13.11CommonByLuigiNovi2.jpg/220px-9.13.11CommonByLuigiNovi2.jpg" align="right" /> It's also the basis for node.js.

Require.js is a non-denominational module loader for Javascript. There are several stylistic variations for using require.js, one that uses the form `var myClownCar = require('myClownCar');`; the format I'm going to use here loads your dependencies in the outer definition of a module, using the `define(..)` syntax. Fortunately this is fairly similar to Angular's style -- except where it's not:

```
(file "resources/circus/clowncar.js")
define(['reqClown', 'reqUnderscore'], function(Clown, _){

});

```

It's worth noting some differences here between require and Angular:

1. there is no name tagged to the module here; that is done elsewhere in a configuration file
2. the resources' default "name" is it's file path -- that is you could refer to it as 'resources/circus/clowncar' if it doesn't have a nicer map in the configuration. 
3. Unlike angular's inclusion binding, the envelope function is a peer of the array of inclusions, not an element of it. 
4. The binding method 'define' is a global function, not a class method, where all of Angular's definition/constructions hangs off the global namespace 'angular'.

## Bringing the two things together

In using Require with Angular you'll find that the way the two systems handle files and resources will have something of an inconsistent mapping. 

* Require.js has a 1:1 mapping between files and resources. The file system IS it's map of the modules. 
* Angular.js doesn't have any use or introspection into the file system -- all of its mapping is done with explicit string names and you can have modules and services defined in any file system you want as long as you eventually satisfy all the prerequisite loading before actually using a component. 

You can see from this why Require satisfies a need in Angular; without Require, you *become* require.js, manually linking your resource files in HTML `<script...>` includes or doing some script based munging of all your scripts into a single file. This is a task that many other dev-side script systems will take on, but at the cost of a single continually growing ball of files that are not all necessarily needed at launch. 

In using require, you'll only load the files you need, when you need them. The downside is, each resource becomes an HTTP request, and the better you factor your app into small single task resources, the more of them you'll have. 

The easiest way to synchronize Angular and Require is to define every resource (service, factory, controller, etc.) as its own module. In that way, you'll have perfect symmetry between require.js and Angular js' module systems.

![1-1 angular require loading](/blog_image/symmetry-a-r.png)

This gives you a very mentally homogeneous process for including resources; if you want a component, you require.js link-in its file, and in the consuming module, angular-link-in its angular-named analog. Note in these diagrams, I use `ang/req` prefixes to highlight when we are talking about an angular module in the angular namespace and a require module in the require namespace; I'm not recommending this be used in production. 

What this system sacrifices is the ability to use the angular module system as its meant to be used: to group related resources together, and to start to think about which clumped resources could ultimately end up in the same file. 

If for instance the module definition is kept discrete, and it's themed in a meaningful way with the kind of resources you expect to be using at once, you can start to organize and prepare to merge files that might as well be loaded at once (as a single munged file) because the resources those separate files contain are going to all be needed at the same time, and breaking them into separate files just slows down the app loading time. 

Here by way of contrast is the same system, with a single module for all related components.

![discrete-a-r.png](/blog_image/discrete-a-r.png)

If you opt to use common angular modules between resources you can also centralize which dependencies they have in the common module; and if you start noticing a clumping of resource dependency, you can separate your module dependency in Angular based on their requirements. 

This gives you the opportunity to focus on how to reduce the number of files you'll eventually need to pull in through require based on shared characteristics.