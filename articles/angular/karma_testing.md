I have found increasingly that TDD is the only way to reasonably accomplish significant tasks without creating a huge wall at the 90% point. Angular is unique in that it has opinionated built-in testing as an integral part of the suite. This is yet another indication that Angular is built with serious apps in mind. 

Unfortunately for those starting up the scaffolding can be kind of extreme. Fortunately there are grunt [build tools in yeoman](https://www.npmjs.com/package/grunt-angular-builder) that I very much recommend. These tools aren't just about testing. they

1. Create a whole project from zip that includes
   * nice folder structures for directives, views, vendor scripts
   * routing 
   * an index.html file that brings in your bower components 
2. Gives you command line tasks for building your development app into distributable files (one javascript file, one css file and one HTML file
3. Lets you create controllers, services and directives with yo-command line tasks; the corresponding test scaffolding is also created
4. Gives you a real time test app that reloads the HTML in the browser when the source changes
5. Lets you run tests through command line directives (grunt karma or grunt test).

## Writing the Tests

Angular can be tested in a variety of systems but Jasmine and Karma are the best documented system (and the one that comes with the builder). The builder tasks include a Phantom driven test system to simulate running your scripts in the browser. 

There is a lot of documentation about how tests are supposed to work... the [Jasmine Documentation](http://jasmine.github.io/2.3/introduction.html) gives you all the usual hooks. However I wanted to cast some attention to the development process and the steps that are required to use data and mocking in your tests.

### Writing for testability

One of the best ways to keep your app testable is to write services that focus on particular areas of concern and allow dependencies to be injected. This in general means very little complex code should be present in controllers; the controller scripts should as a rule just bridge events through to services. 

#### Leaning on Services

As a note -- there used to be two different systems in Angular (Services and Factories) that defined patterns of injection. Factory patterns have been kind of deprecated in Angular and Service is now the standard for building reusable components

Services in Angular are basically just fundamental ways of associating a value with a name. 
*Services* are injected into *modules*. In showing what a service is, its useful to show what it is in comparison to the other fundamental components in Angular. 

* **Directives and Controllers** exist to transmute elements in templates/html into dynamic components. They do this by combining templates (which are put into the DOM when they are loaded) and controller scopes, which define variables exposed through said templates. As such they are tied to the way Angular digests DOM.
* **Filters** are functions that exist as helpers to process values in the DOM. one classic one is `date`; in the template, `{{ article.publishDate  date:'d MMMM yyyy'}}` will appear as `August 2015`. 
* **Modules** are top level namespaces. Modules can have other modules as dependencies, which allows one module to avail itself of another modules' resources. They are larger packages that contain services, directives, filters and controllers. Your Application is a service, and your controllers, directives, filters and services are all local to your application. 

**Services** are code blocks that don't directly tie in or get driven off DOM/HTML/template content. They exist only within the namespace of a particular module, or any module that includes their module. They are designed to provide nice testable-type activities like getting data, reusable business logic, helper functions etc. 

#### In Practice: My Role Playing Game application

I am doing a project that is a manual and proof of concept of a desktop role playing game, SUITS. 

* I have **Directives** that pull markdown and run it through Showdown to translate it into HTML. 
* I have **Services** that pull in game data (skill lists, weapons) and expose it for use in controllers and directives. For instance, one service reads markdown files from disk; another splits CSV content into usable tables. 
* I have a **Controller** that displays articles, that drive article selection through URL values. Other controllers exist to simulate the game play. 

The more I progressed, the more specific my services became; larger classes were broken up in to sub-functional services connected to primary classes through the [Facade](http://en.wikipedia.org/wiki/Facade_pattern) pattern. For instance, the character class used to manage inventory within its swelling codebase; then that code became too bulky and I broke the inventory into a CharacterInventory class that both concentrated inventory related activity into a separate service and provided adaptive functions that it decorated over the characters' controller. 

### A sample real-world test

Each character has a hand. This hand is abstracted into a service class. Hand depends on suitsPlayingCards -- the deck -- which in turn depends on a vendor library playingCards. 

While in theory every service but the one under test should be mocked or stubbed, in practice, I just used a method to inject card values into suitsPlayingCards to get realistic results from the Hand. 

Hand contains an array of cards, exposed through the cards() method. Each card has a rank and a (not tested here) suit that is a one or two-level string 'A', '2','10', 'K'... . 
These tests use _.pluck to map card arrays into arrays of the cards' rank. 

These tests not only compare the output values and the current values of the hand but in parts, whether cards that are discarded actually end up in suitsPlayingCards' discarded cards collection.

Here is the test methods for hand:

``` javascript

'use strict';

describe('Service: Hand', function () {

  // load the service's module
  beforeEach(module('SuitsApp'));

  // instantiate service
  var Hand, suitsPlayingCards;
  beforeEach(inject(function (_Hand_, _suitsPlayingCards_) {
    Hand = _Hand_;
    suitsPlayingCards = _suitsPlayingCards_;

  }));

  var myHand;

  beforeEach(function () {
    suitsPlayingCards.reset();
    suitsPlayingCards.injectValues('2,3,6,5,9,4,7,9,10'.split(','));
    myHand = new Hand(suitsPlayingCards, 4, suitsPlayingCards.drawCards(4));
  });

  it('#count', function () {
    expect(myHand.count()).toEqual(4);
  });

  it('#cards', function () {
    expect(_.pluck(myHand.cards(), 'rank')).toEqual('2,3,6,5'.split(','));
  });

  describe('#discardCard', function () {
    it('should be able to discard a nonpresent card without problems', function () {
      var otherCard = suitsPlayingCards.drawCard();
      myHand.discardCard(otherCard);

      expect(_.pluck(myHand.cards(), 'rank')).toEqual('2,3,6,5'.split(','));
      expect(suitsPlayingCards.discardedCards()).toEqual([]);
    });
    it('should be able to discard a present card', function () {
      var otherCard = myHand.cards()[0];
      myHand.discardCard(otherCard);

      expect(_.pluck(myHand.cards(), 'rank')).toEqual('3,6,5'.split(','));
      expect(suitsPlayingCards.discardedCards()).toEqual([otherCard]);
    });
  });

  describe('#removeCard', function () {
    it('should be able to remove a nonpresent card without problems', function () {
      var otherCard = suitsPlayingCards.drawCard();
      myHand.removeCard(otherCard);

      expect(_.pluck(myHand.cards(), 'rank')).toEqual('2,3,6,5'.split(','));
      expect(suitsPlayingCards.discardedCards()).toEqual([]);
    });
    it('should be able to remove a present card', function () {
      var otherCard = myHand.cards()[0];
      myHand.removeCard(otherCard);

      expect(_.pluck(myHand.cards(), 'rank')).toEqual('3,6,5'.split(','));
      expect(suitsPlayingCards.discardedCards()).toEqual([]);
    });
  });

  describe('#refill', function () {

    it('should allow you to override max cards', function(){
      myHand.refill(5);
      expect(_.pluck(myHand.cards(), 'rank')).toEqual('2,3,6,5,9'.split(','));
    });

    it('should not change the hand if it is full', function () {
      myHand.refill();
      expect(_.pluck(myHand.cards(), 'rank')).toEqual('2,3,6,5'.split(','));
    });

    it('should add two cards if the hand is two cards short', function () {
      var cards = myHand.cards();
      myHand.discardCard(cards[0]);
      myHand.discardCard(cards[1]);
      myHand.refill();
      expect(_.pluck(myHand.cards(), 'rank')).toEqual('6,5,9,4'.split(','));
    });

  });

  describe('#highestCardUnder', function () {

    beforeEach(function () {
      suitsPlayingCards.reset();
      suitsPlayingCards.injectValues('2,K,7,A,5,10'.split(','));
      myHand = new Hand(suitsPlayingCards, 4, suitsPlayingCards.drawCards(4));
    });

    it('should be 7 with value 10', function () {
      expect(myHand.highestCardWithin(10).rank).toEqual('7');
    });

    it('should be K with value 4', function () {
      expect(myHand.highestCardWithin(4).rank).toEqual('K');
    });

    describe('face card preference', function () {

      beforeEach(function () {
        suitsPlayingCards.reset();
        suitsPlayingCards.injectValues('A,K,3,5'.split(','));
        myHand = new Hand(suitsPlayingCards, 4, suitsPlayingCards.drawCards(4));
      });

      it('should be 3 with value 4 if preference is -1', function () {
        expect(myHand.highestCardWithin(4, -1).rank).toEqual('3');
      });

      it('should be K with value 4 if preference is 1', function () {
        expect(myHand.highestCardWithin(4, 1).rank).toEqual('K');
      });
    })
  })

});

```

##### Notable takeaways:

1. Its okay and necessary some times to create "back door" methods in your data classes to force known values. In this case, playing cards aren't retrieved through http/files -- they autogenerate randomly. 
Because of this, you can't use the provider system to force given results. An alternative method would have been to mock out the playingCards service to give known results but that felt a little heavyhanded. 
2. For the most part each `it` statement has a single expectation. In cases where a particular sequence needs to be tested this rule is broken. Case in point, the discard method which both puts cards into suitsPlayingCards' discard collection and removes it from the hand. Each effect could be tested with separate `it` statements but that just bloats the code. 
3. These tests are focused and specifically about the rank of the cards; suits could be injected as well if they were necessary but there isn't any current gameplay mechanic where they're relevant so we aren't paying attention to them here. 
4. `describe` and `it` are mixed freely for the method level tests; when more than one `it` is required per method, or where specific setup code is required for a test, describe is used. 

### Handling rest/file driven services

One thing that the Angular $http system does is focus all your resource-driven activity through a separate class. Angular comes with an $httpBackend service that intercepts given URLs for testing purposes and allows you to force given paths to have given results. For example a CSV datafile I use
can be mocked out as:

``` javascript

  var httpBackend, rootScope, _;
  beforeEach(inject(function (_$httpBackend_, $rootScope, lodash) {
    httpBackend = _$httpBackend_;
    rootScope = $rootScope;
    _ = lodash;

    var data = "attribute,name,Description\n" +
      "WL,Acting,\"Ability to imitate, deceive, and conceal your motivations and emotional state.\"\n" +
      "MI,Administration,Knowledge of systems and practices of organization and accounting.\n" +
      "MI,Archery,Use of bows and crossbows\n" +
      "MI,Artillery,\"Use of indirect weaponry, including cannon, bombs, rockets, and siege engines\"";

    httpBackend.whenGET('/data/csv/character generator/Sheet 1-Skills.csv').respond(data);
  }));

```

note the end line that associates the data block with a given URL. It will return this result 
no matter how many requests are made upon it. 

#### Why Mock Data?

You can, as is done in Ruby, provide data through actual data sources for testing (i.e., a test database). There are several reasons this isn't ideal. 

For one thing, Database interaction is one of the slowest things you can do in code

Also, unlike Ruby which operates as a relatively low level with SQL , in frontend code, the data is usually boiled down into JSON format (or at least some serial format such as in my case, CSV/markdown). Because of this doing the gyrations necessary to generate JSON for each test run doesn't add any integrity to the code under test and just adds significant slowdown to the test execution

The down side is that you will get false positives if your schema changes on the backend and your code does not (or false negatives if they are kept in sync) and you may find re-grinding out mock data tedious.  

### Mocking services 

The immense hive of services and deep dependencies can be intercepted to control and channel the information feeding into your systems under test. There are two basic strategies to use here:

1. **Intercept the dependent data.** As described above you can use all your classes as written and just provide intercepts to the remote data sourcefiles through `$httpBackend`. 
2. **Mock the entire service.** A given service can be completely replaced within a testing cycle; this is (almost entirely) the big gain you get by using integrated testing suites with Angular. Here is an example of providing a replacement service: 

`````
  beforeEach(module(function ($provide) {
    $provide.service('weapons', function () {
      return _mockWeapons;
    });
  }));
````

this replaces the entire weapons service with a _mockWeapons object that is written in and exists within the testing scope to present a known set of weapons. 

One good thing about service level mocking is that you're generally doing so in front of the digestion code, so you may end up doing a bit fewer updates as your schema changes. A bad thing about service level mocking is that the farther your diverge from the actual application code in your tests the easier it is for bugs to slip through them.

### Writing Test Friendly Services

The Character class was a great example of service bloat and teat writing. It originally had tons of methods for inventory control, skill management, and card/hand management. At the app evolved it became clear that each of these activities deserved their own component. As a quick summary here are a few key methods from the final (current) class structure:

```

Character
  - hand : CharacterHand
       - refill()
       - cards()
  - actionCard: CharacterActionCard
    - value()
    - choose()
  - items: CharacterItems
    - _items[Item]
    - add(itemDef, count)
    - remove(itemDef, count)
    -
  - skills: CharacterSkills
    - rank(skillDef) : int
    - _skills[Skill]
    - has(skillDef) : bool
    - add(skillDef, count)

```

This is a minute subset of the functionality these classes cover but they show how massive hunks of functionality can be clumped into task handlers with focused responsibility. 

#### Adapting facades

While subobject methods can be called directly -- `character.items.add(item, 3)` -- this becomes both tiresome and harder to maintain. Its easier often to create root level methods that extend the parent classes' functionality, as in 

``` javascript


    CharacterSkills.adapt = function(proto){
      _.extend(proto, {
        addSkill: function (skillName, level) {
          this._skills.put(skillName, level);
        },

        skillRank: function (skillDef) {
          return this._skills.rank(skillDef);
        },

        skillLevel: function (skillDef) {
          return this._skills.level(skillDef);
        }
      });
    };

```

There is some slightly "dirty" stuff going on here in that I'm referring to Characters' private properties (_skills) in an external class. This could just as easily be boiled into the definition of Character itself, but this adaption pattern concentrates all the skill stuff in one place.

Then in the parent class, pass the prototype through these decorator functions:

```

      function Character(params) {
        params = params || {};

        this.id = ++id;
        this.name = params.name || 'Character ' + this.id;
        this.attrs = _.defaults(params.attrs || {}, _defaultAttrs);
        this._hand = new Hand(suitsPlayingCards, 4).refill();
        this.actionCard = new CharacterActionCard(this);
        this._skills = new CharacterSkills(this, params.skills);
        this._items = new CharacterItems(this);

        this.colleges = params.colleges || [];
        this.state = {
          shock: 0, damage: 0, trauma: 0
        };
        this.summary = {
          // transient/state scratchpad
        };
      }

   Character.prototype = Object.create(EventEmitter.prototype);

      _.extend(Character.prototype, {


    //  various root level methods
      });

      CharacterItems.adapt(Character.prototype);
      CharacterSkills.adapt(Character.prototype);
      CharacterActionCard.adapt(Character.prototype);

```

The component tests use significantly reduced stand-ins for the character; for the most part the only requirement for the character is that it be event friendly and that it have attrs (personal attributes like Speed and Mind); the bulk of the work is kept internal to the facade classes: 

``` javascript

'use strict';

describe('Service: CharacterItems', function () {

  // load the service's module
  beforeEach(module('SuitsApp'));

  // instantiate service
  var CharacterItems, Item, EventEmitter;
  beforeEach(inject(function (_CharacterItems_, _Item_, _EventEmitter_) {
    CharacterItems = _CharacterItems_;
    Item = _Item_;
    EventEmitter = _EventEmitter_;
  }));

  var character, items, ropeItem, shirtItem;

  beforeEach(function () {
    character = new EventEmitter();
    ropeItem = new Item('Rope');
    shirtItem = new Item('Shirt');
    items = new CharacterItems(character);
    items.add(shirtItem, 2);
  });

  it('#has', function () {
    expect(items.has(shirtItem)).toBeTruthy();
    expect(items.has(ropeItem)).toBeFalsy();
    expect(items.has(shirtItem.name)).toBeTruthy();
    expect(items.has(ropeItem.name)).toBeFalsy();
  })

  it('#count', function () {
    expect(items.count('Rope')).toEqual(0);
    items.add(ropeItem);
    expect(items.count('Rope')).toEqual(1);
    expect(items.count(ropeItem)).toEqual(1);
  });

  describe('#add', function () {

    it('should recognize count param', function(){
      expect(items.count(shirtItem)).toEqual(2);
    });

    it('should sum repeated adds', function(){
      items.add(ropeItem);
      expect(items.count('Rope')).toEqual(1);
      items.add(ropeItem);
      expect(items.count('Rope')).toEqual(2);
    });
  });

  describe('#reduce', function () {
    it('should remove one item if no count', function () {
      items.reduce(shirtItem);
      expect(items.count(shirtItem)).toEqual(1);
    });

    it('should throw errors if removing nonexistent items', function () {
      expect(function () {
        items.reduce(ropeItem);
      }).toThrow();
    });

    it('should not have shirts if all are removed', function () {
      items.reduce(shirtItem, 2);
      expect(items.count(shirtItem)).toEqual(0);
      expect(items.has(shirtItem)).toBeFalsy();
    });
  });

});

```

#### Notes

1. The class Hand is an exception to the naming pattern because its intended to be used in multiple contexts associated with things other than characters; potentially as a generalized set. 
2. Unlike Backbone, which relies on a specialized model class, Angular accepts any type of data (POJOS, arrays, whatever) and leaves digestion to you. The good thing is that means there is a lot less cruft code around your data which is kept lean and mean. The bad thing is that if you want advanced features like events on your model classes you must add them yourself. I do so here using a client-friendly flavor of Node.js's EventEmitter.
3. Because Item is a very simple class that doesn't do any I/O file operations I use it directly without any mocking; if it were an activeRecord I'd probably have mocked it or provided httpBackend standins for the data source. 

### Directives with TemplateURLs

One of the major snags I ran into is that directives with TemplateURLS do not work out of the box with the generated Angular. [This Video](https://vimeo.com/90876119) documents how to rig your test setup better than any other one I'd come across.