ES6 is large. But in using it I've found a distinct subset of things that are really useful and need to know. Here they are in cheat sheet form.

## var -iations

`var` -- the variable declarer -- has two replacements:

* `let`, which is equal to var in most cases, initializes a variable that can be changed (as was true for var).
* `const` assigns a value to a variable that cannot be changed (mostly). 

### where const CAN be changed

Assigning a simple scalar (string or number) to a const ensures it cannot be changed. However, complex objects or arrays, while they cannot be changed wholesale, can have their *member* values change; you can still add to the array, call array methods like `reverse()` or `slice()` that mutate the subject, etc. So, `const` is not a completely immutable construct in some ways. 

``` javascript

let a = 3;
a += 2;
console.log('a: ', a); /* outputs a: 5 */

const b = 3; 
 // and it always will
 
 const c = [a, b];
 console.log('c:', c);
 c.reverse();
 console.log('c now:', c);

```

(see [this jsFiddle](https://jsfiddle.net/bingomanatee/awtLmog7/) )

## Arrow (Lambda) functions

Arrow functions are a very compact functional notation, kind of the '(bool)?ifThen:ifFalse' version of a function. 

```` javascript

function double(value) {
   return 2 * value;
}

// is equal to 

const double = (value) => {
return (value) => 2 * value;
}

// and equal to 

const doubleShort = value => 2 * value;
```` 

* for single line functions, the braces(`{}`) are optional. Also the return is optional (implied) as in ruby.
* If you only have one argument the parentheses(`(argument)`) are optional, though I tend to include them ayway for clarity
* Lambda functions are not closures; this inside lambda === this outside lambda. 

```` javascript

function Worker(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
  let capitalizeMe = () => {
    this.firstName = `${this.firstName.substr(0,1).toUpperCase()}${this.firstName.substr(1)}`;
    this.lastName = `${this.lastName.substr(0,1).toUpperCase()}${this.lastName.substr(1)}`;
  }
  this.capitalize = function () {
     setTimeout(capitalizeMe, 2);
  }

  this.name = () => `${this.firstName} ${this.lastName}`;
}

let worker = new Worker('bob', 'jones');

console.log('worker: ', worker.name());

worker.capitalize();

setTimeout(() => console.log('worker: ', worker.name()), 100);
/**
output:
worker:  bob jones
worker:  Bob Jones
*/
````
(see [this jsfiddle](https://jsfiddle.net/bingomanatee/enfw4p5k/) )

note how the floating function "capitalizeMe" adopted the "this" of the worker context. A regular function in a timeout loses context completely (try it!).

**HOWEVER**: lambdas cannot be bound!

```` javascript
const context = {bars : 3};

function fooFn () {
console.log(this.bars + 1);
}

fooFn.bind(context)();

const foo = () => console.log(this.bars + 1);
foo.bind(context)();
```` 
(see [this jsFiddle](https://jsfiddle.net/bingomanatee/v11gqoq2/) )

works in the first case (function bound) but not the second. So, if you are using a function in a context that redefines "this" you do NOT want to use an arrow function. 

## Compact object notation ("enhanced object literals")

is typing `{foo: foo, bar:bar}` wearing down your fingers? no more! If your variable names match your desired parameter names,  you can use this shorthand:

```` javascript
const name = 'Slick';
const lastName = 'Shady';

const longObj = {name: name, lastName: lastName};
const shortObj = {name, lastName};

console.log(JSON.stringify(longObj));
console.log(JSON.stringify(shortObj));

/**
result: 
{"name":"Slick","lastName":"Shady"}
{"name":"Slick","lastName":"Shady"}
*/
````
(see [this jsFiddle](https://jsfiddle.net/bingomanatee/fowkx4tk/) )

## for..of:  `for(let member of arrayValue) {}`

iterating through arrays no longer requires a long form `for()` construct or an `.each` function. 
You can now use `for let value of myArray {...}` format. This doesn't give you access to the index of the current element, of course. You an iterate over arrays, strings, and other "iterable" things -- but sorry, not objects.

``` javascript
const alpha = 2;
const beta = 3;
const name = 'slim shady';

const dataArray = [alpha, beta, name];

for (let value of dataArray) {
	console.log('data value:',value);
}
```

see [The Mozilla documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of) for more examples.

## For further reading

the babeljs.io site has an [es6 tutorial](https://babeljs.io/docs/learn-es2015/) that is a good reference for es6. The new client site uses babel as a pipeline/shim to deliver es6 now. 

Note, the node [release versions are mostly es6 friendly](https://nodejs.org/en/docs/es6/).