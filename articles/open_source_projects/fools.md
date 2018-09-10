Fools is a set of functional recipes for very common architectural patterns. 

Some of these patterns are very close to javascript logical patterns (**if**, **case**, **while**, etc.). 

Others take on more invested tasks like a weighted ranking, (`rate`), sorting things into numeric brackets(`rank`), finding matching `pairs` or building blocks for a rules based system (`gauntlet`, `fork`, `until`). Even looping over a multidimensional dataset is easier with `loop`. 

## Why? 

The original goal for Fools was to create a rules based system for Javascript (Functional rOOLS). However along that road I found a few patterns that were cluttering up my code and begged for a more systematic approach. These are very much "sugar stuff". For instance the common deep tree

``` javascript

for (var i = -10; i <= 10; ++i){
  for (var j = -100; j <= 100; ++j){
    for (var k = -100; k <= 100; ++k){
      console.log('i: ', i, 'j: ', j, 'k: ', k);
    }
  }
}
```

really eats up a lot of visual space; 

``` javascript
Fools.loop(function(iter){ console.log('i:', iter.i, 'j:', iter.j, 'k:', iter.k);})
.dim('i', -10, 10).dim('j', -100, 100).dim('k', -100, 100)();
```
is much more compact. Even better, Fools.loop returns a function which can be executed in any place a function is appropriate. You can even return a memo from it:

``` javascript
Fools.loop(function(iter, memo){
  if (iter.i == 0 || iter.j == 0 || iter.k == 0) {
    memo.push(iter);
  }
  return member;
})
.dim('i', -10, 10).dim('j', -100, 100).dim('k', -100, 100)([]);
```

This very simple iterator will return an array of all points in the matrix that are along the axes. 

## Rating Things

Another example of a common task encapsulated in fools is rating things. Say you want to find the nearest point to a coordinate. The amount of code required to find the closest point is significant but `rate` is a very healthy shorthand for this kind of cumulative comparison. 

In this case we are using the rating to emit a value that is the distance to the base point, meaning the closest point will have the lowest rating. 

``` javascript

var point = {x: 3, y: 4, z: 5};

var rate = Fools.rate()
    .prop('x', function(x){
        return Math.abs(x - point.x);
    }, 1)    .prop('y', function(y){
        return Math.abs(y - point.y);
    }, 1)    .prop('z', function(z){
        return Math.abs(z - point.z);
    }, 1);

var points = _.map(_.range(0, 50), function(){
    return {
        x: Math.round(Math.random() * 10),
        y:  Math.round(Math.random() * 10),
        z:  Math.round(Math.random() * 10)
    }
});

console.log(points);

rate.add(points);

console.log('nearest point to %s: %s', util.inspect(point), util.inspect(rate.worst()));

console.log('within 3  to the point: ', util.inspect(rate.select(0, 3)));

```

results in something like:

``` javascript
[ { x: 3, y: 7, z: 4 },
  { x: 4, y: 4, z: 0 },
  { x: 4, y: 6, z: 2 },
// ...
  { x: 1, y: 0, z: 10 },
  { x: 9, y: 7, z: 1 },
  { x: 1, y: 3, z: 1 },
  { x: 9, y: 2, z: 9 } ] // 50 values

nearest point to { x: 3, y: 4, z: 5 }: { rating: 0.3333333333333333, data: { x: 3, y: 5, z: 5 } }
within 3  to the point:  [ { rating: 1.3333333333333333, data: { x: 3, y: 7, z: 4 } },
  { rating: 2, data: { x: 4, y: 4, z: 0 } },
  { rating: 2, data: { x: 4, y: 6, z: 2 } },
  { rating: 2, data: { x: 4, y: 7, z: 3 } },
  { rating: 2.6666666666666665, data: { x: 6, y: 0, z: 4 } },
  { rating: 2.3333333333333335, data: { x: 5, y: 8, z: 6 } },
  { rating: 2, data: { x: 1, y: 8, z: 5 } },
  { rating: 1.6666666666666667, data: { x: 4, y: 2, z: 3 } },
  { rating: 0.3333333333333333, data: { x: 3, y: 5, z: 5 } },
  { rating: 2, data: { x: 1, y: 2, z: 7 } },
// ...
  { rating: 2, data: { x: 4, y: 5, z: 1 } },
  { rating: 1.3333333333333333, data: { x: 1, y: 3, z: 4 } },
  { rating: 2.3333333333333335, data: { x: 5, y: 1, z: 3 } },
  { rating: 2, data: { x: 0, y: 3, z: 7 } },
  { rating: 2.3333333333333335, data: { x: 1, y: 3, z: 1 } } ]
```

## Defining Behaviors using Gauntlet and Fork

A recent experiment I did combining n-space, fools and Famo.us, illustrates how great Fools is at defining comprehensible expert system/behavior gates:

``` javascript


        this.behavior = Fools.gauntlet()
            .add(function(goat, good) {
                var check_death = Fools.fork(function(goat) {
                    return goat.hunger() > goat.options.maxHunger;
                })
                    .then(function(goat) {
                        debugger;
                        goat.die();
                        // stop
                        good();
                        return goat;
                    })
                    .else(function(goat) {
                        // continue
                        return goat;
                    });

                Fools.fork(function(goat) {
                    return !goat.options.alive;
                })
                    .then(function() {
                        // end of line
                        good();
                        return goat;
                    })
                    .else(
                    check_death
                )(goat);
            })
            .add(function(goat, good) {
                if (goat.options.fainted > 0) {
                    --goat.options.fainted;
                    good();
                }
                return goat;
            })
            .add(function(goat, good) {
                if (Math.random() > 0.95) {
                    goat.options.fainted = Math.random() * 10;
                    goat.mod.setTransform(goat._transform(), {duration: 300});
                    good();
                }
                return goat;
            })
            .add(function(goat, good) {

                var keep_eating = Fools.fork(function(goat) {
                    return goat._is_eating;
                })
                    .then(function(goat) {
                        good();
                        goat.eat();
                        return goat;
                    })
                    .else(function(goat) {
                    })
                    .err(function(err) {
                        console.log("goat err: ", err);
                    });

                var eat = Fools.fork(function(goat) {
                    return goat.canEat();
                })
                    .then(function(goat) {
                        goat.eat();
                        good();
                    }).else(function(goat) {
                        goat.stopEating();
                    });

                Fools.fork(function(goat) {
                    return  (goat.hunger() > 3);
                })
                    .then(eat)
                    .else(keep_eating)
                    .err(function(err) {
                        console.log('goat err 2 ', err);
                    })
                (goat);
            })
            .add(function(goat, good) {
                if (!goat._is_eating) {
                    goat.walk();
                }
                good();
            });

```

### The Gauntlet pattern

Fools.gauntlet enforces a pattern of delegation; it allows you to define a set of gates, each with a unique handler. 
The handler has the signature `.add(function(goat, good) {..})`. These switches act like "functional case statements": If the handler calls `good()`, then the handler stops processing of the input; if not the next handler (defined by another `.add(..)` statement) is executed in turn. 

To reiterate, Fools.gauntlet returns a function with the above-defined behavior. In Goats, this function is called every time a goat has to choose a mode. 

### The Fools.fork pattern

If Fools.gauntlet is the "functional case", then Fork defines the "functional if"; when called in nested form, it allows a bit more manageability for branching logic. 

``` javascript
                var check_death = Fools.fork(function(goat) {
                    return goat.hunger() > goat.options.maxHunger;
                })
                    .then(function(goat) {
                        debugger;
                        goat.die();
                        // stop
                        good();
                        return goat;
                    })
                    .else(function(goat) {
                        // continue
                        return goat;
                    });

                Fools.fork(function(goat) {
                    return !goat.options.alive;
                })
                    .then(function() {
                        // end of line
                        good();
                        return goat;
                    })
                    .else(
                    check_death
                )(goat);
```

In the above extract, the second fork is created; and if the test (goat is not alive) is false, then the check_death branch is executed to see if the goat SHOULD be dead...

Note that in these branches, when a "stopping point" (a dead goat) is found, the good() method is called to prevent further activity. (as dead goats are not likely to have any...)

The resulting example can be found [here](http://bingomanatee.github.io/goats/); the source can be found in [this example in the n-space repository.](https://github.com/bingomanatee/n-space/tree/master/goats_and_wolves)