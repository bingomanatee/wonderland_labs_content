I think objects have proven to be useful as far as they go but I would like a higher order of objects that can do things you seem to always need them to do. 

## Serialize better. 

Object properties should be able to serialize themselves for use in `toJSON()`. Just as we have `get` and `set` for es5 properties, we should have `ser`. 

Also, toJSON should be a fundamental method. I would however like to see it accept an array of property names (or just multiple args) for focused serialization; i.e., `userRecord.toJSON('name', 'id', 'age')`. 

For that matter, the whole JSON class should go away. `.serialize` should be innate to Object and `.parse` should be a method of String. (as in `String.parse('{"foo": 2}');

## Clone. 

Clone should be a fundamental method. Just as the `ser` metaMethod for properties would allow for easy contextual serializing, `clone` could be used for copying values from properties. Likewise, a `cloned(sourceObj){..}` handler would substitute for the constructor for the cloned object. 

### Example: 

``` javascript

var nextId = 0;
class User {
   constructor (name, friends) {
    this.id = ++nextId;
    this.name = name;
    this.friends = friends;
  }

  cloned( baseUser ) {
    this.id = ++nextId;
    this.name = baseUser.name;
    this.friends = baseUser.friends; // will be the cloned value of baseUser. 
  }

  set friends(friendList) {
   this._friends = friendList ? friendList.slice(0): [];
  }
  get friends () { return this._friends; }
  clone friends () { return this._friends.slice(0); }
}

``` 

## Be observable

a built-in observer patter would be really handy. something like

``` javascript

var foo = {power: 10};
var cancel = foo.observe('power', (p, pFrom) => console.log(`power of foo changed from ${pFrom} to ${p}));
...
cancel(); // stops observation. 

delete foo.power;
// power of foo changed from 10 to undefined
foo.power = 5
// power of foo changed from undefined to 5

```

## Be Deletable

You should be able to delete an object; this would not only destroy any observation (see above) but would allow you to force a replacement value to be substituted for every reference (default undefined). 

``` javascript

class User {
   constructor (name, friends) {
    this.id = ++nextId;
    this.name = name;
    this.friends = friends;
  }

  cloned( baseUser ) {
    this.id = ++nextId;
    this.name = baseUser.name;
    this.friends = baseUser.friends; // will be the cloned value of baseUser. 
  }

// friends is assumed to be an array of User objects
  set friends(friendList) {
   this._friends = friendList ? friendList.slice(0): [];
  }
  get friends () { return this._friends; }
  clone friends () { return this._friends.slice(0); }

  unfriend(user) {
    let newFriends = [];
    for (let friend of this.friends) {
      if (friend.id !== user.id) newFriends.push(friend);
    }
    this.friends = newFriends;
  }
}
User.delete = (){
  for (let user of this.friends) {
    user.unfriend(this);
  }
}
```

## Be Eventful

Events are well described in node's event module. I think they should be the deFacto for ALL objects, so we can take advantage of compiler level eventing. 

## Be stringable

Object.toString should by default equate to `JSON.serialize(myObject.toJSON())`, and as is the case now, should be overridable.