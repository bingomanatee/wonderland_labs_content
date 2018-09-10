In my first stabs at React I realized that store management and communication is one of the areas of React where complexity could accrete. Looking around at Flux alternatives, I found redux to be the library with the most approachable API and not coincidentally a significant following. 

However in my opinion it is a bit scattered in that it has three discrete but tightly bound components:

1. The Action definitions: a set of names and factories for action definitions
2. The State function: a pure function that combines previous state and action input to update the state (generally, with a switch)
3. The Store, an observer that decorates the state function and adds hooks for observation. 

All these three things are meaningless, and I found myself running in triangles updating the state and store in two places. I also found the experience of having one central function a very unmodular experience.

To reduce the overhead and modularize the redux store I created redux-store, in NPM. 

Some of the things I like about this approach is:

* You can create one or more actions from inside components at any time after the store has been created. 
* You can add sub-stores -- i.e., use stores as values of the state object -- and access them directly or indirectly. 
* The entire system is based off a single object which has all the functionality and methods needed to poll or change state. 

Here is the README:

# redux-store

**A store/action subsystem for redux**

[![npm status](http://img.shields.io/npm/v/redux-store.svg?style=flat-square)](https://www.npmjs.org/package/redux-store) [![Dependency status](https://img.shields.io/david/bingomanatee/redux-store.svg?style=flat-square)](https://david-dm.org/bingomanatee/redux-store)

## example

`npm i redux-store`

```js
const reduxStore = require('redux-store')
```

## api

### `main(arg[,opts])`

## install

With [npm](https://npmjs.org) do:

```
npm install redux-store
```

## Introduction

This class internalizes the store, state and action trilogy of redux in a single objects. 

Actions are created off the `store.addAction` method, which unlike a switch based system allows for 
actions to be added dynamically by client processes. 

## Store 

Store is a self contained redux-based state machine. It internalizes the state function and allows for 
dynamic addition of actions. 

It decorates an internal redux store, exposing 

* getState
* subscribe
* dispatch

with all the expected behaviors from redux. 

### Expanded (lazy) dispatch

Dispatch accepts an object, which must have a type (string) property. It also accepts two expanded signatures:

``` javascript

myState.dispatch(TYPE, object);

myState.dispatch(TYPE, KEY, VALUE);

```

So al three of these calls have the same effect:

``` javascript

myState.dispatch({type: 'ADD', value: 4});
myState.dispatch('ADD', {value: 4});
myState.dispatch('ADD', 'value', 4);

```

### Constructor

Store's constructor is the initial state. its optional; if ommitted initial state is empty object. 

### addAction

Add Action defines a legal action that can be taken on the store. 

Store exposes

* addAction(name, handler)

to allow for on-the-fly definitions of actions that can be dispatched. the handler is a method with the signature

``` javascript

(action, state) => { }

```

... where action is the dispatched object and state is the previous state. 

The handler returns a new object that updates one or more fields of the state; it is analogous to the content of a setState call in react. 

### Chaining

`addAction` and `dispatch` are chainable (return self) allowing:

```
var state = new State({value: 1});
var afterState = state.addAction('ADD', (action, state) => {value: state.value + action.value})
.dispatch('ADD', 'value', 4)
.dispatch('ADD', 'value', 6);

// afterState.value = 10;

```

### Nesting States

States can have states as properties of their state. (I know this is a bit screwy but I can't figure out a better way to say it.)

As seen in the spec: 

``` javascript 

    var start = {
        one: new Store({value: 0}).addAction(ADD, ADD_METHOD)
    }

    var store = new Store(start);

    nestedTest.same(store.getState(), {one: {value: 0}});

    store.dispatch(`one.${ADD}`, {add: 5});

    nestedTest.same(store.getState(), {one: {value: 5}});
    
```

note, `getState()` recursively boils down any sub-properties into their current getState() values. If you want to access 
a property of state that is a known state, you must call `myStore.getState(true)` that disables this post-process.

Also, as long as you dispatch actions from the root state, all actions should alert subscribers, 
whether the action ultimately updates the root state or the child state. i.e.,

``` javascript

var dispatchCount = 0;

var start = {
    one: new Store({value: 0}).addAction(ADD, ADD_METHOD)
}

var store = new Store(start);

var u = store.subscribe(() => ++dispatchCount);

store.dispatch(`one.${ADD}`, {add: 5});

nestedTest.equal(dispatchCount, 1);

```

... however `store.getState(true).one.dispatch('ADD', 'value', 5)` would not alert any subscribers to store. 
This is intentional to allow local subscription to state's substates when that is a desired behavior. 

#### dispatching to a nested state

If property 'foo' of state is a subState and it has an action 'bar', calling `myState('foo.bar') will apply the action 'bar'
to `state.foo`. 

## license

[MIT](http://opensource.org/licenses/MIT) © [David Edelhart](http://wonderlandlabs.com)