This is a project I did to take a task I seem to find myself redoing in context after context: managing items in 2/3D spaces in which each item has a coordinate (in whole numbers), much like content in a minecraft like system.

## The World is Enough

A N-Space world is defined using the constructor: `new NSPACE.World({i: [-10, 10], j: [-4, 10]})`. The configuration object defines the dimensions of the world and the minimum and maximum extent of each axis. There are no rules limiting the axes -- any letter may be used to identify the axes, and any integer values (in ascending order) are used to define the minimum and maximum values (inclusive). 

Any number of axes can be added to a world. 

Each cell of the world contains a registry which can be used to hold any arbitrary content. The only limitation is that when the content is added it is given a string type that is used when retrieving it from the world. 

``` javascript

var foo = {...};

var world = new NSPACE.World({x: [0, 10], y: [0, 5]});

world.add(foo, 'fthings', {x: 2, y: 2});

var foosAt2x2 = world.getRegistry({x:2, y:2}).get('fthings');
// foosAt2x2 = [foo];

```

## Members

Although anything can be added to N-Space worlds, members are specifically designed to be included in worlds; its constructor 

``` javascript
      var m = new NSPACE.Member(mType, world, loc, stackLimit)
```

allows for one-stroke activity (like moving from cell to cell) that takes multiple steps when done manually(removing from one cell, adding to another). 

## Forever wanderer

One of the frequent tasks of members is to move from cell to (adjacent) cells based on business logic. WanderBots accomplish this using a member property and a set of rules to decide the bot's next destination.