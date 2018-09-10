First a bit of theory. 

## The Facade Pattern

The facade pattern is where a subclass does work for a parent class behind the scenes.

<a class="embedly-card" href="https://en.wikipedia.org/wiki/Facade_pattern">Facade pattern - Wikipedia, the free encyclopedia</a>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

This is useful:

* to reduce bloat in parent classes
* to keep the parent class clean by keeping variables solely devoted to state or the function of the private class out of the scope of the parent
* to provide reusable functionality to a set of similar but unrelated classes
* to allow for the testing and maintenance of the subclass independent of the parent.

Sometimes you access the methods of a child class inside methods of the parent. Other times, as in the case here, you just want to pass on the functionality of the child and add it to the parent. This is not always necessary as the methods can be directly accessed, but it does reduce code bloat. 

## My Case Study

I'm developing a card based role playing game for which each character has a hand of conventional playing cards. This is an RPG but it might as well be a poker simulation; characters play cards from their hands, draw cards from the deck (represented by a singleton called `suitsPlayingCards`), and may need to find the best card or cards for a particular task. 

### The classes 

The publically exposed classes are: 

* Character, a player constructor. 
* Hand, a collection of cards
* playingCards, a third party library that represents a deck of playing cards.
* suitsPlayingCards, a library that acts as a singleton, bridging playingCards to the game and having methods for filtering and reducing cards to numeric values. 

The cards themselves are a weak class -- basically a POJO object with properties like `rank`, `suit`', etc. What the class doesn't have is a method to reduce the cards to usable values for comparison; which makes sense as thats intrinsically a part of a game system, not the card in question. So this task is the responsibility of methods of suitsPlayingCards. they are given local ids to make identically valued cards unique. 

#### Hand

Hand has methods like 

*`count()` (int)
*`refill()` 
*`removeCard(card)` -- removes a given card from the hand. 
*`draw()` -- pulls cards from suitsPlayingCards into the hand

The hand also has properties set at construction like `maxCards` that allows refill() to know how many cards to pull in. The hand can begin with cards injected in, or with cards drawn from suitsPlayingCards, or empty. 

#### Character

The Character has a property hand, which is set to new Hand({..}). Its set to a maximum of three cards, however upon initialization, you get four cards, one of which you must play out immediately face up as the action card to determine the order of action.

This means you can call 

``` javascript

var c - new Character();
console.log('start cards', c.hand.count()) // == 0
c.hand.refill(4) // overpopulating the hand to 1
console.log('cards after refill', c.hand.count()) // == 4
c.actionCard = c.hand.cards()[0];
c.hand.removeCard(c.actionCard();
console.log('cards after refill', c.hand.count()) // == 3

```

There is more complex (and internalized) business logic here but this shows the basic procedure.

It also shows what a drag it is to introspect into the property all the time. 

## Writing an adaptive bridge into the character

It'd be a lot easier to be able to call `c.refill()` directly. Even better if it was named something helpful like `c.refillHand()`. 

You can write bridging methods like:

``` javascript

Character.prototype = {
refillHand: function(n){ return this.hand.refill(n); }
}

```

however in production this kind of repetitive boilerplate clogs the codebase. In fact its pretty easy to write a function that procedurally extends the prototype of Character to allow for use of hand methods directly; in fact its no great task to map the names of the child class to a more helpfully named method to be appended to the parent. 

We do this with a utility function adapt. adapt is a metafunction that produces a method that bridges the functionality of the child onto the parent. 

``` javascript
'use strict';

/**
 * @ngdoc service
 * @name SuitsApp.adapt
 * @description
 * # adapt applies a facade that delegates functions of a subproperty from the main class.
 * Service in the SuitsApp.
 */
angular.module('SuitsApp')
  .service('adapt', ['lodash', function (_) {
    /**
     * @param config {object} with the following properties
     *  base {function/constructor}
     *  methodsToAdapt {[String]} optional; if passed, only extends a methodsToAdapt of the bases' properties
     *  methodAliases {object} optional; if passed, creates synonyms of subobject properties to be used in the base.
     * @param nameOfBaseInParent {string} the name of the property  containing the base instance
     * @returns {Function}
     */
    function adapt(config) {
      var methodAliases = config.methodAliases;
      var onConflicts = config.hasOwnProperty('onConflicts') ? config.override : 'warn';
      var singleton = !!config.singleton;
      var base = config.base;

      if (!base) {
        throw new Error('attempt to adapt without a base')
      }

      var methodsToAdapt = config.methodsToAdapt || (function () {
          var methods = [];
          var source = singleton ? base : base.prototype;
          for (var prop in source) {
            if (source.hasOwnProperty(prop)
              && !/^_/.test(prop)
              && _.isFunction(source[prop])) {
              methods.push(prop);
            }
          }
          return methods;
        })();

      return function (parentClass, nameOfBaseInParent) {

        if (config.hasOwnProperty('nameOfBaseInParent')) {
          nameOfBaseInParent = config.nameOfBaseInParent;
        }
        if (!nameOfBaseInParent && !singleton) {
          throw new Error('attempt to adapt without a nameOfBaseInParent');
        }

        var proto = parentClass.prototype;

        if (!proto) {
          console.log('bad input', parentClass);
          throw new Error('no prototype ');
        }

        _.each(methodsToAdapt, function (methodName) {
          if (methodName == 'adapt') {
            return;
          }
          var parentMethodName;

          if (methodAliases && methodAliases.hasOwnProperty(methodName)) {
            parentMethodName = methodAliases[methodName];
          } else {
            parentMethodName = methodName;
          }

          // is thre a pre-existing method of the same name? clash alert.
          if (proto[parentMethodName]) {
            if (onConflicts == 'warn') {
            } else if (!onConflicts) {
              throw new Error('attempt to onConflicts method in base class: ', +parentMethodName);
            }
          }

          if (singleton) {
            proto[parentMethodName] = function () {
              if (!base[methodName]) {
                throw new Error('cannot find method ' + methodName + ' in this.');
              }
              var args = _.toArray(arguments);
              return base[methodName].apply(base, args);
            }
          } else {
            proto[parentMethodName] = function () {
              var subObject = this[nameOfBaseInParent];
              if (!subObject) {
                throw new Error('there is no ' + nameOfBaseInParent + ' in this', this);
              }
              if (!subObject[methodName]) {
                throw new Error(['cannot find method ', methodName, 'in this.' + nameOfBaseInParent, '(', subObject, ')'].join(' '))
              }
              var args = _.toArray(arguments);
              return subObject[methodName].apply(subObject, args);
            }
          }
        })
      }

    }

    return adapt;
  }]);

```

Then in the Hand service, we attach the output'd method as a static method of the Hand function:

``` javascript

function Hand(){...}

Hand.prototype = {...}

Hand.adapt = adapt({
  base: Hand,
  methodAliases: {
    count: 'cardCount',
    refill: 'refillHand'
        }
});

```

This will attach all public methods of Hand to the Character class; some of the methods will be renamed because of the value of map. 

At this point we can now call `myCharacter.cardCount()` and it will delegate to `myCharacter.hand.count()`.

### Injecting the parent into the child object

As it happens, Hand is a well focused class that requires little knowledge of either the parent class (Character) or any of its sibling subobjects. When you do need better introspection on the part of the child object, you can inject the parent into the child, as seen in the Character constructor:

``` javascript

      var _id = 0;

      function Character(params) {
        params = params || {};

        this.id = ++_id;
        Character.index.set(this);
        this.name = params.name || 'Character ' + this.id;
        this.attrs = new CharacterAttrs(this, params.attrs);
        this.hand = new Hand({count: 3, empty: true});
        // note - hand is a basic collection of cards, more general 
        // than the other facade classes, and doesn't require the character as input
        this.actionCard = new CharacterActionCard(this);
        this.cooldown = new CharacterCooldown(this);
        this._skills = new CharacterSkills(this, params.skills);
        this._items = new CharacterItems(this, params.items);
        this._cff = new CharacterFriendFinder(this);
...
        });
...

      
      Hand.adapt(Character, 'hand');
      CharacterFightBrain.adapt(Character,'_characterFightBrain');
      CharacterActionCard.adapt(Character, 'actionCard'); *

```

## The result: 


``` javascript

var c - new Character();
console.log('start cards', c.cardCount()) // == 0
c.refillHand(4) // overpopulating the hand to 1
console.log('cards after refill', c.cardCount()) // == 4
c.actionCard = c.cards()[0]; 
// note -- because its not explicitly mapped, 
// Character.hands.count is mapped directly to Character.count. 
c.hand.removeCard(c.actionCard();
console.log('cards after refill', c.cardCount()) // == 3

```

While it may not save a lot of space, it does do what we want to do with the facade, which is break the (bad) pattern of accessing internal subobject methods from the parent scope. 

### Miscellaneous concerns

One concern in self-modifying code is order of operations. Since dependencies should resolve themselves based on their own subobjects, dependency should work itself out. Given that we don't link to the subObjects' methods until after instantiation, this should not be a problem but it is definitely a consideration. 

This code is written to be flexible; for instance, the child class won't generally know what it's called in the parent context, this linkage is only made upon final execution of the adapt method. This means for instance you could have two or more child instances, and alias them to different names if you wanted to. 

Also although private methods are automatically hidden, you may want to reduce the profile of the linked methods further by manually assigning `methodsToAdapt`. Take care when using `methodAliases` and methodsToAdapt together; the methodsToAdapt should still use the child classes names, not their aliases. Also, even if you include a method in methodAliases, it must still be present in methodsToAdapt or it won't be included. 

#### The default `methodsToAdapt`

Unless manually set, methodsToAdapt will be populated based on:

* the properties of the Bases' prototype <span>**</span>
* the property must be mapped to a function
* the property name must not begin with an underscore.

If you want to expose a method that begins with underscore as in `_state`, you must manually include it (and all other desired methods) in methodsToAdapt. 

You *can* depend on the child class's adapt to have been executed before it is passed out of the service definition scope. This means you can (in theory) remap methods which are themselves produced through adaption of the classes' child methods. 

### Singleton mapping

While the primary focus of adaption here is to adapt child class methods, you can also map static library methods of singleton objects onto a class' prototype by passing singleton as true in the configuration. If this is true, every child instance will be able to access singleton methods as if they were methods of their own prototype. The execution scope will be that of the singleton, so if self-reference (this) is required you will have to inject it in the argument list when calling it. 

## Custom Adaption

You may find automatic code manipulation makes you uncomfortable. If this is true you can always write your own adapt method and attach it to the constructor function. This is the original model for adaption in my code (and I'm trying to deprecate it) but if you are more comfortable with 

``` javascript

      CharacterItems.adapt = function (proto) {
        _.extend(proto, {

          weapons: function () {
            return this._items.weaponData();
          },

          hasWeapons: function () {
            return this._items.hasWeapons();
          },

          removeWeapons: function () {
            _.each(this.weapons(), function (weapon) {
              this.removeItems(weapon.name);
            }, this);
          },

          itemCount: function (name) {
            return this._items.count(name);
          },

          hasItem: function (name) {
            return this._items.has(name)
          },

          removeItems: function (name) {
            return this._items.remove(name);
          },

          removeItem: function (name) {
            var count = (arguments.length > 1) ? _.last(arguments) : 1
            this._items.reduce(name, count);
          },

          addItem: function (item, count) {
            this._items.add(item, count);
          },

          items: function (asArray) {
            return this._items.inventory(asArray);
          }
        });

```

The advantage here is the adaption is much more transparent. The disadvantage is it makes for much more boilerplate, and it hard-codes the child objects' name into the method. Also, manual adaption functions are much less careful about sanity checks and do not react to overwriting existing methods. Note also, the *prototype* is being passed in here, as opposed to the constructor function.

----------

* note the actual codebase has a different signature because the adapt utility is a new addition to the codebase.
** or the base itself if the singleton flag is set