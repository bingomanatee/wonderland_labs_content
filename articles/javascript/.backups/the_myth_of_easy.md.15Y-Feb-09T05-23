There are a slew of blogs I've reading recently about how Angular is bad because it is complex. (http://okmaya.com/2014/03/12/the-reason-angular-js-will-fail/)[http://okmaya.com/2014/03/12/the-reason-angular-js-will-fail/] for example. It reminds me of something my friend Corey tells me frequently: "Every time someone uses the word 'Easy' it makes me feel afraid."

The fact is, there is significant horsepower under Angular's roof and a lot of it is not transparent to a new developer. And even if you don't go down the rabbit hole of custom directives and isolate scopes, even the superficial features of Angular are not bulletproof; for instance, I did a datagrid using basic Angular features in Personal Capital, and discovered it is quite possible to overwhelm repeat loops with high (>1,000) records. (the ng-ui grid takes care of this somewhat with row virtualization). 

The impulse to want everything to be easy, transparent and self managing on the web sphere is understandable. There is a lot of complexity under management on a web app -- network, client/server, race conditions, and the fact that the DOM is basically an insufficient way to model a visual paradigm, which is why groups like Famo.us are putting so much effort into patching it. 

However Backbone, the previous leader in managed single page applications, lost its throne for a reason. It was, by all real measures, easy; because it failed to address the complexity that naturally emerges from an event driven system when the developer takes wholesale responsibility for managing the event network of a complex page. I did work (again) at Personal Capital and saw the immense expansion of complexity that the DIY approach that Backbone fosters creates in a production application, and introduced Angular as an alternative to Angular because it significantly cuts down that hive of event management. 

## Yes its hard. You get paid to do hard work. 

Angular gets harder faster because it lets you overcome the easy problems faster; i.e., it allows you to develop the basic structure of a system so quickly, that you will reach the bottlenecks of your given system much faster than you will if you were to plough through the same problem with a Backbone approach. (see (http://victorsavkin.com/post/67496301619/building-models-in-backbone-and-angular)[http://victorsavkin.com/post/67496301619/building-models-in-backbone-and-angular] for details on this.)

Angular also takes while it gives; you can't (easily) use any templating system you might have gotten attached to with Angular (though 2.0 may change this as its decoupling the view and model from what I hear). And for all the giving it does in the view department, it doesn't have the dynamic event driven model system of Backbone; I'm running into a situation where I'm having to recreate the model system of Backbone in order to synchronize updates across controllers.  

Also, when it comes to directive programming, the API is not the most transparent I've ever seen, as is the scope/isolate system it exposes. 

To which I say, suck it up. A lot of brilliant minds have spent decades trying to make web development approachable, and generally its about robbing from Peter to pay Paul. Ruby simplified the API at a criminal cost to performance. Node solves performance, but obligates you to get closer to the heart of process management and error management. 

Angular does subsume a huge amount of code you'd have to crank out by hand with data binding, but at some times, this buries the processes that are most likely to hurt performance. 

The upshot of it is, whatever you do, you're going to have to take your brains out of your pants and solve non-trivial problems that the system you're using does not; this is called "earning your paycheck." 

Angular provides you with a profoundly deep toolset to answer a host of problems, but to use them, you will have do understand the why behind the how, watch some web videos, read some documentation, other peoples' blogs, and try and fail a lot. 

If you are an art major who dropped out of high school because algebra was hard, this will ruffle your feathers. If you actually studied for your field, or made up for it by doing your homework on the Internet, this is a bit less daunting. 

I've practiced development for several years and it keeps being as hard as it was when I started, because each revolution in process allows people to springboard past the formerly daunting tasks and dive straight into the cutting edge problems of the day. Ruby solves traditional server side form/submit apps so single page apps become the new frontier. 

Hard is what development is about; making hard look easy, and making it work for you, not against it. Hard means you will be staying til 10, losing the odd weekend, and hopefully, relying on more than your own personal mental resources to solve intransigent problems. 

Enjoy your work. Because it will be just as hard in five years as it is today, and its why simple people rely on you to be your best on a daily basis.