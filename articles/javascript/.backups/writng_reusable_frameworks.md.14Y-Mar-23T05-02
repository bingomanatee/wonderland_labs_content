I have written and read a lot of JS code designed to manage reusable JS frameworks. Here are a few thoughts as to how to make frameworks user friendly and maintainable

## First, test first 

If you don't start with testing by the time you realize you need it you will have committed to a lot of patterns, many of which may confound testing. I personally like node-tap and Mocha

## Use grunt to do the fussy stuff

Grunt is astoundingly good at doing mundane tasks; the UMD module for instance makes your code browser/node friendly. Additionally tasks like concatenation, minification et.all are very easy to set as repeated patterns in Grunt. 

## Use separate files for each class

With Grunt-concat in place there is no reason not to separate your code files into sensible granular units. 

## Use parameter objects for any method with three or more arguments

remembering the order that parameters are supposed to be passed in is a pain; no matter how much effort you put into making your API consistent it generally becomes easier to add them in as a hash over the long run for all but the simplest methods

## Curry early and often

The ability to call methods serially really defines a cool awesome modern JS framework. 

## Contain as much as possible inside Closures

You should put as little as possible into the global namespace; as a rule if you can force your framework to have one global name from which all the resources of your framework can be accessed, you'll have a much easier time of it.