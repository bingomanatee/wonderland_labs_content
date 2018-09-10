Sails.js is doing what every n00b tries to do when they first get hooked on node: Make a Rails for node.

The classic response from the node community for people who do this (myself having been one of them is that they are solving a problem that doesn't exist. This is a natural response from someone who has never really "got" what the demands of a production site are. Express (or sinatra) is a great layer for those with a thin business layer and a strong client that basically just needs REST and a little auth. 

The days of the monolithic server are declining, and as someone who spent a decade plus on server side apps, I say good day. But the fact is, scaling and process management on the server layer for true production scale apps is still a concern as is rapid prototyping and best practices, and leaving a large amount of the application structure to individual conceit, no matter how conceited the individual, is a recipe for long term pain. 

Rails has done a good job of establishing standards for server side code and injecting testing as a scientific method of reducing regression; The cost of that has been accepting one of the slowest languages in creation as an industry standard, in an industry that for the most part equates speed with money. (and with [good cause](http://www.stevesouders.com/)).

Node.js delivers speed as well as a homogeneity of codebase) undeliverable by Ruby/Rails and a whole new set of architectural assumptions that dissolve the basis for monolithic applications; given that is Sails.js a step back to the bad old days (of now) of the monolithic application where everyone is using the same system and it becomes hopelessly embedded in the enterprise?

I hope so because [Sails.js](http://sailsjs.org) is nearly perfect. 

## Sails Delivers

Sails is a great structural artifact that gives you most of the gains of Rails up front -- 

* database abstraction (that is noSQL friendly)
* console based generators
* preprocessors for actions
* scaffolding
* configurable layouts
* a clear, comprehensive standard for directory structure

and stuff it doesn't

* out of the box web sockets
* a better comprehension of client side demands
* The ability to use existing express mixins

and probably a lot more I haven't unearthed yet. 

## Except that it still is not quite production ready yet

Sails is very very good but also, very new and needs a little kicking around until it is robust enough to be used in a moneymaking situation. If you are a hobbyist or are doing proof of concepts, it is absolutely the best thing you can get your hands on right now. If your goal is to work fast and develop miraculous tools, it is great. 

The reason I am cautious are a few noticeable but significant issues; such as when it starts up it delivers assets while it is loading as opposed to after it has loaded, meaning you might get your HTML/action working before the CSS/javascript are available, giving you hunky broken pages. 

Also the documentation is good but not comprehensive. For instance, the REST scaffolding won't work unless you create a controller for a given model; this isn't something I found out in docs, it was the result of experiments that ate up a significant amount of time. 

As an individual, I am happy to do these experiments and take one for the team; as a professional I don't want to be caught in situations where these shortcomings damage the reputation of me and my team. 

So while Sails is a definitive achievement long needed and is headed in the right direction, I say, jump on, use it privately, and wait for it to mature to the point where we can truly say that node.js has no peers and no competition in the web sphere, because that day is not long in coming.