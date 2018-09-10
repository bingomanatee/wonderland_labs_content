One of the absences I've been feeling around the react framework is the loss of the DI utility in Angular. For those who haven't got deep enough in Angular to do serious testing (And yes you _should_ feel bad!) the module framework probably feels like a huge timehole. However when you do get into areas of testing where mocking and stubbing become paramount, a [dependency injection](https://en.wikipedia.org/wiki/Dependency_injection)(DI) framework is a crucial part of the equation. 

## Dependency Injection.

Given that DI is broadly documented I'll do a "TLDR" here and say the idea behind it is that if you use a resource framework to get the components (classes, constants, classes) of your application from, you can then replace some or most of them with stand-ins that you "turk" for the purposes of testing. 

This operates a lot like a shop, like a car dealership. When you want a Honda Civic, you go to the civic dealership and choose the color, and the dealer gives you your civic of the right color. In reality, this is a process that triggers (or leverages) a lot of "gets" -- 

1. The dealer gets a car which a factory provides, 
2. ...with parts that were got from other factories
3. ... with parts that were manufactured from other resources, 
4. ... which were acquired by mining companies using tools that were got from factories

which you can drive down the rabbit hole long enough and you end up at the big bang. 

Where this model becomes useful is that from a code point, if you want to test a single factory, you can control its inputs, make a request, and determine if its outputs are what you expect. 

## Resourcefulness

I use the word resource a lot here; in this context a resource is anything you can assign to a single variable in javascript. That non-definition is important because in the DI context, you should be able to chuck anything out of the system that you need to share throughout the program's ecosystem. 

Examples include

* configuration strings
* functions
* objects or arrays (generally speaking, configuration clusters or in-memory collections
* classes
* singletons (state machines, models, etc.)
* Templates (in string or functional form

resources generally don't include raw data records, though these can be memoized and accessed through collections that are stored as resources.

Third party code like lodash and jQuery are sometimes piped in as resources as well. [Require.js](http://requirejs.org/) is a DI system that has specialized into this role. 

### Inversion of Control and other words that are big

Inversion of Control(IOC) is usually talked about in the context of DI. IOC is a bit of a quagmire, but the TLDR of it is that a construct (class) is like a living thing that is made up of organs, which may themselves be made up of other constructs, etc., and a code construct's behavior is governed by the nature of its organs. This means that a class's behavior is not just about the code written inside it, but is about the code it accepts as building blocks. 

The utility of this is that if you have a code block that reads and writes to files and you inject a file IO block into it from the DI framework, you can (for instance) alter the directory root of the file being written to through global configuration, or write into and out of a "virtual" file system by altering the file IO resource to your needs (say for testing); or even, wrap or filter every file being read/written by decorating or replacing the file IO resource. 

## React DI vs. Bottle.js DI 

the commonJS/EJS module system is to some extent a form of DI. However, there are features of an injection system that the JS module ecosystem do not allow; primarily the ability to use code to redirect or overwrite a resource that has been defined in DI. 

for instance, if you want a core module like `http` in node, you cannot override it for the purpose of testing. the code `const http = require('http') will always return node's core http class, and you can't tell node that "When the user asks for http, return MyMockHTTP stand-in, because I want my tests to access a mock resource, not a true internet call". 

DI in Javsacript means writing a scripting layer between the systems like file inclusion and module systems that provide resources in Node or the browser (or webpack et.al) and the endpoints that require resources. 

The purpose of DI is to: 

1. Allow the sharing of code resources through an ecosystem
2. Allow the decoration or replacement of resources during testing
3. allow resource to get other resources they need as dependencies or configuration values as they are accessed. 

Examples of the last point include

1. putting database access values into DI for your model classes to use
2. putting singletons such as state machines into DI for shared access across systems
3. putting base classes into DI for other classes to use

Just to create a frame of reference for this, [Angular's model of DI](http://antjanus.com/blog/web-development-tutorials/front-end-development/angularjs-dependency-injection-nutshell/) equates to this: 

1. Create a *module*, a namespace that is associated with the global `angular` object. (no relation to the node module/commonJS module)
2. Every resource associated with that module is spawned with a function that ties the end product to a name, and pulls in previously defined resources as dependencies. 
3. Modules can be composed into other modules, mixing/merging their definitions into the parent.

This last part is a real magical thing and a source of chaos and ambiguity; for instance, 

* you have a module `foo` which has resources 'alpha', 'beta', and 'gamma'. 
* you have a module `bar` which has resources 'beta', 'omega', and 'lambda'.
* you have a module `vey` which composes `[foo, bar]` as sources. 

What is the definition of 'beta' in vey? foo's beta? bar's beta? crash and burn? And why can't we just access them directly as bar.beta or foo.beta? grr. 

[Bottlejs](https://github.com/young-steveo/bottlejs) is a standalone microservice that does what angular's DI does, and uses most of the same words, because they both implement pretty much the same pattern.

In Bottlejs, you create bottles (analogous to Angular modules) that have methods (factory, service, etc.,) that create resources, and a `.container` object through which you can get them by name. 

## Some examples from the README: 

```` javascript
var bottle = new Bottle();
bottle.service('Beer', Beer);
// Later, when you need the constructed service, you just access the Beer property like this:
bottle.container.Beer;
```` 
What if the Beer service had dependencies? For example:

```` javascript

var Barley = function() {};
var Hops = function() {};
var Water = function() {};
var Beer = function(barley, hops, water) { /* A beer service, :yum: */ };
You can register services with Bottle#service and include dependencies like this:

var bottle = new Bottle();
bottle.service('Barley', Barley);
bottle.service('Hops', Hops);
bottle.service('Water', Water);
bottle.service('Beer', Beer, 'Barley', 'Hops', 'Water');
// Now, when you access bottle.container.Beer, Bottle will lazily load all of the dependencies and inject them into your Beer service before returning it.
````

Service Factory

If you need more complex logic when generating a service, you can register a factory instead. A factory function receives the container as an argument, and should return your constructed service:

```` javascript

var bottle = new Bottle();
bottle.service('Barley', Barley);
bottle.service('Hops', Hops);
bottle.service('Water', Water);
bottle.factory('Beer', function(container) {
    var barley = container.Barley;
    var hops = container.Hops;
    var water = container.Water;

    barley.halved();
    hops.doubled();
    water.spring();
    return new Beer(barley, hops, water);
});
````

## A real world example

I have a mongo model system; usually it will store data in a production repo, but in the case
of testing I want it to write to a junk database that I create and then destroy at the end of the testing process. 

here is my model root resource: 

```` javascript

"use strict";
require('babel-register');
const Bottle = require('bottlejs');
const userModelFactory = require('./UserModel').default;
const mealModelFactory = require('./MealModel').default;
const searchLogFactory = require('./SearchLog').default;
const searchCacheFactory = require('./SearchCache').default;
const initMongoose = require('./initMongoose').default;
const modelUtils = require('./utils/index').default;

const init = (mongooseURL) => {
    const bottle = new Bottle();

    modelUtils(bottle);

    bottle.factory('key', () => mongooseURL);

    bottle.factory('conn', function (container) {
        return initMongoose(container.key);
    });

    mealModelFactory(bottle);
    userModelFactory(bottle);
    searchLogFactory(bottle);
    searchCacheFactory(bottle);

    return bottle;
};

export default init;
````

The mealModelFactory looks like this:

```` javascript

"use strict";

import mongoose from 'mongoose';
import classifyWords from './utils/classifyWords';
import _ from 'lodash';

const mealModelFactory = (conn, classifyWords, TwitterPosts, tally, aggregate) => {

    let MealModel;
    const schema = new mongoose.Schema(require('./mealModel.json'), {collection: 'Meals'});
    schema.pre('save', function (next) {
        const now = Math.floor(new Date().getTime() / 1000);
        if (!this.created_unixtime) {
            this.created_unixtime = now;
        }
        this.updated_unixtime = now;
        next();
    });

    schema.statics.mealsForUser = (provider, username) => new Promise((resolve, reject) => {
            ...
    });

    schema.methods.scanPosts = function () {
        ...
    };

    schema.methods.scanPostsTwitter = function () {
        ...
    };

    MealModel = conn.model('Meals', schema);

    return MealModel;
};

export default (bottle) => {
    bottle.service('MealModel', mealModelFactory, 'conn', 'classifyWords', 'TwitterPosts', 'tally', 'aggregate');
};

````