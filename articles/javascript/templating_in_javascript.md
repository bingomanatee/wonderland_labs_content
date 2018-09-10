Templating is an activity that people seem to get very "Magical" about. Templating is the process of producing a string that varies based on output that is either 

1. inserted into the DOM (in the client side context) or 
2. returned from an HTTP request (in the server side [node.js, rails, REST] context. 

How you go about templating varies a lot -- there are of course many, many ways to produce a template. 

## Functional Templates vs. OOP "Template objects"

A functional template is a function that you put input into and get a string out of. it is (as a rule) *(idempotent)[http://en.wikipedia.org/wiki/Idempotence]* -- that is, for a given set of input it returns the same output. &ast;

A template object is an OOP structure that you add data to both while constructed and over the course of executing your app, as you get more information from logic branches, databases, etc. 

It has a method that you use to extract strings from, but you may call this method more than once as you iterate over data, change values, etc. 

## Implicit templates vs. file templates

Some template systems inject the template in code, while others leave the template as a file (that other code injects). Most MVC systems, Rails, Express views, etc. use this practice. The theory is that it makes it easier for designers to work with your environment. (the reality is, many designers are pretty freaked out at anything with variables in it. )

It is also considered good "separation of concerns" to have separate template files. 

## Custom scripting vs. native scripting

This is the single biggest question in any templating system -- does it allow you to use whatever language you are coding in or does it have its own "Micro language". In my experience, custom/DSL languages of templating systems are the most frustrating element in working with a templating system. Creating a language is a very complex task that implies you have predicted all future control uses of anyone that uses the system and provided structures that satisfy them. 

As an example of a templating system that allows native constructs, EJS (the javascript equivalent of Ruby's ERB) allows full embedded javascript. A side effect of that in javascript is that you can inject "helper functions" in the same manner as you inject variable/data information into your template, and use them in your templates. 

## Real DOM vs. encoded DOM

Some templating systems think that dom is something evil that should be encoded into an abstraction all its own. This is so wrong I won't get into it in detail; given that one major reason to template is to expose the templates to designers, creating a domain language for HTML DOM is guaranteed to make their head explode. I greatly prefer the sane languages that do not encode DOM in another abstraction layer. 

## A great engine illustrating this:

[Grann's template choose](http://garann.github.io/template-chooser/) shows these alternatives pretty clearly. Its worth noting that the second choice, "How much logic should it have?" will eliminate half the contestants and that is pretty sad. 

## My Two choices

I prefer two systems for templating, both of which use essentially the same annotation: EJS and Underscore Templating. Both languages allow you to use true HTML and full Javascript. EJS has the ability to include(inject) template blocks from the file system, but both start out with a string template injected into them. 

The resulting object is a function that returns strings when data is passed in, and can process full Javascript - and yes, I repeated that, because its that important. 

----------

&ast; (with allowances for things like use of the Date() to render time/date data...)