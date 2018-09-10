After coming out of the Kit Cambridge (excellent) presentation on EMCA 6, I really wanted to know why we can't just drop in ECMA 6. 

And that led me to wonder, why all of these HTML5 features are not plug-inable. 

I think at this point the browser is just too big to use a release based plan. 

When you think about it browsers are basically a set of extremely discrete tasks:

1. network i/o
2. markup parsing and rendering
3. event broadcasting
4. script engines

and ideally, things like video, canvas, WebGL, SVG should really be packages that use the common framework and standards that the community provides, not a bundled set of features that a company creates. 

## Browsers are too important to use old world software practices. 

I know, nominally, browsers like Firefox and Chrome are open source. But in reality, the way that HTML 5 features and scripting engines like EMCA 6 roll out don't speak to the kind of crowdsharing practices you see in communities like Node and jQuery. 

When you think about it, the points of contact that JavaScript -- or Dart or any other programming language -- have with the page are very specific and easily abstracted:

* The scripting language needs a DOM traversal API, and the ability to manipulate DOM elements -- very well defined and known at this point
* It needs to react to dom events and mouse events
* It needs to be able to do IO to the network in some pretty well defined/beaten to death ways
* It needs to be able to message in and out of the HTML5 constructs -- video, workers, local data

If browsers were developed using community coding practices, then you could load any scripting language you like -- javaScript, CommmonJS, EMCA6, Java, Ruby, ObjectiveC -- based on doctype or meta configurations, as well as a particular version or release of WebGL, web video, local memory, etc. 

As long as the scripting language could pass a fairly straightforward set of tests for interaction and do so in a reasonably memory/CPU conservant manner, and obey the known security patterns, then any third party should be able to write and publish a standard scripting component for all the browsers -- whether it be javaScript 5, 6, C, Python, Ruby or the Microsoft stuff. 

The cliche that insanity is doing the same mistake over and over and expecting different results, then the current owners of browsers are clinically insane and we need to define browsers not as monolithic programs that get updated in releases, but as hosts for community developed pluggable resources that match a defined API and meet the test criteria developed by the community from these standards.

A Browser should be able to serve a page with a known set of scripting and multimedia components in much the same way as an HTML page is digestible regardless of where the image/CSS/JS files are located. 

There is no good reason why, for instance, every browser can't run the V8 javascript engine.