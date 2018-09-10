The "We'll test it when we have time" code of MVP engineering leaves chaos and orders of magnitude more work in its wake than it justifies by short term results. The benefits of treating testability as a "Post launch" concern are significant, especially considering that writing testable and tested code once you get used to it only add a fraction of overhead to your project, where taking the low road has significantly higher costs to overall productivity and scalability. 

Here are a few considerations that I find taking into consideration up front make long term testable code writing much easier. 

## Always insulate native code with application level accessors. 

If your code used raw native code systems to retrieve and set values then mocking and intercepting those information pathways becomes impossible. This may end up requiring wholesale rewrite of codebases when testing becomes a priority. Here are a few examples of code patterns that confound abstraction and testing:

### 1: Retrieving key values from native functions

Calling methods like (in Javascript) `Math.random()` or new `Date().getTime()` directly prevents you from injecting test-driven values at a later point. 

Say you want to test this method: 

````language=javascript

function anyMember(array) {
  const index = Math.floor(Math.random() * array.length);
  return array[index];
}
````

Certainly it will do its job; but if you had a random function as an injectable/mockable resource:

````language=javascript

function anyMember(array) {
const index = Math.floor(provider('random')() * array.length);
return array[index]
}

````
then your test can create a deterministic result from random:
```

provider.mock('random', () => 0.2);
expect(provider('anyMember')([2,4,6,8,10]).toEql(6);

```

Similarly if you hard code SQL endpoints and retrieval into closed methods 

```
def getUser(id) {
  SERVER_ID= 'http://api.wonderlandlabs.com:3200'
  USER_ID = 'apiUser'
  USER_PW = 'apiPass'
  conn = Postgres.connect(SERVER_ID, USER_ID, USER_PW)
  conn.query('SELECT * FROM public.users WHERE id = ?', id).first
end
```
even in a testing suite, calling getUser(4) will always hit a remote network endpoint; you'd have to mock and stub Postgres library methods itself which is pretty crappy design. If instead you had a library that accesses those resources:

```
def getUserI(id)
   WonderlandAPI.getById(id, 'user')
end
```

you can mock out your own a single endpoint of your own code (much cleaner) which prevents you from littering your own code with messy mocking out of third party low level calls. 

... WIP
