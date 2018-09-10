I've been using a lot of test frameworks in various companies and Tap in my own work, and the more I do so, the more I come to appreciate Tap. Tap is a [protocol for testing](https://testanything.org/), and node tap is maintained by  [Isaac Z. Schlueter](http://blog.izs.me/), one of Node's founders. 

All test frameworks work -- to the promises they make. However testing is a very specific process and how it works is just as important as that it works. 

All the test frameworks other than Tap are context-centric; that is, they assume the test framework has booted up and placed constructs in scope ('it', 'describe', 'expect', etc. ). This means you cannot directly run a test file as a node run; you must run a test runner and target the test file. 

With Tap tests are totally self contained; you can simply call 'node './mytestfile.js'. You can step through it in an ide (Webstorm) debugger, etc. both the assertions and the test end condition is attached to a context instance, which is loaded from a standard node module. In sum, there is a lot less magic and context switching. You could even embed a test switch in the code itself for direct CI. 

This also means there is no mutation of base objects and addition of handlers to core classes and values to produce ruby style magic. 

Here is a sample set of model tests in tap, 
`babel-register` is required because the model files are written in babelized es6 (with imports etc.) but this file is not. 
note the tap library is loaded in this page context, not from some hidden parent context. This makes this file completely self contained and runnable. 


``` javascript

"use strict";
require('babel-register');
```

note the tap library is loaded in this page context, not from some hidden parent context. This makes this file completely self contained and runnable. 
```` javascript

const initMongoose = require('../../models/initMongoose').default;
const umf = require('../../models/UserModel').default;
const tap = require('tap');
const util = require('util');

````

-- note -- when I started this article I didn't realize that tap actually *DOES* have `before`/`beforeEach`/`after`/`afterEach` hooks. While I don't feel like rewriting around these hooks, the approach below is legitimate anyway. 

(As an alternative to before hooks...) I created a pre/post fixture generator; it's called directly before each test and returns a fixture with usable values from a promise. 
it also has a terminate function that does cleanup stuff and `.end()`s the passed-in test context. The inner testDB polls and waits for the database to connect; not strictly necessary, but useful to allow for inspection of connections that cannot be made.

```` javascript

const initTestDB = () => new Promise((resolve) => {
    let conn;
    let dbName;
    let UserModel;

    const n = Math.floor(Math.random() * 1000000);
    dbName = `userModelTestCollection${n}`;
    conn = initMongoose(`mongodb://localhost:27017/${dbName}`);

    const terminate = (test) => new Promise((resolve, reject) => {
        conn.db.dropDatabase((err) => {
            if (err) {
                test.fail(err.message);
                return reject(err);
            }
            conn.close();
            test.end();
            resolve();
        });
    });

    const testDB = () => {
        if (conn.readyState != 1) {
            setTimeout(testDB, 200);
        } else {
            UserModel = umf(conn);
            resolve({
                UserModel,
                conn,
                dbName,
                terminate
            });
        }
    };
    testDB();
});

```` 

the tests are produced from a factory by tap. Each factory produces a test object that runs until `testContext.end()` is called. That in this case is delegated to the terminate call above. each `test([name], injectFn)` is analogous to a suite or a describe; but its a concrete call on an in-context resource, so no magic. 

the assertions are done on that context object passed to the injectFn; again, a concrete reference to an object in context. 

``` javascript

tap.test('UserModel', (umTap) => {
    umTap.test('can serialize a user', function (sTap) {
        initTestDB().then((context) => {
            const UserModel = context.UserModel;
            const PHOTOS = ["http://www.foo.com/bar.png", "http://www.foo.com/vey.png"];
            var user = new UserModel({
                username: 'foo',
                displayName: 'Foo Johnson',
                provider: 'twitter',
                photos: PHOTOS,
                twAuth: {
                    tokenSecret: 'foo',
                    token: 'bar'
                }
            });

            var data = user.toObject();
            delete data._id;

            UserModel.serialize(data)
                .then(id => {
                    UserModel.findById(id, (err, user) => {
                        if (err) {
                            return sTap.fail(err.message);
                        }

                        sTap.equal(user.username, 'foo');
                        sTap.equal(user.displayName, 'Foo Johnson');
                        context.terminate(sTap);
                    });
                }, (err) => {
                    util.log('error from getting id: ' + err.message);
                    sTap.fail(err.message);
                });
        }).catch((err) => {
            util.error('error bootstrapping mongo:' + err.message);
        });
    });
    umTap.test('can deserialize a user', function (suTest) {
        initTestDB().then((context) => {
            const UserModel = context.UserModel;
            const PHOTOS = ["http://www.foo.com/bar.png", "http://www.foo.com/vey.png"];

            var user = new UserModel({
                username: 'foo',
                displayName: 'Foo Johnson',
                provider: 'twitter',
                photos: PHOTOS,
                twAuth: {
                    tokenSecret: 'foo',
                    token: 'bar'
                }
            });

            var data = user.toObject();
            delete data._id;

            UserModel.serialize(data)
                .then(id => {
                    UserModel.deserialize(id)
                        .then(dsUser => {
                            suTest.equal(dsUser.username, user.username);
                            suTest.equal(dsUser.displayName, user.displayName);
                            context.terminate(suTest);
                        });
                });
        });
    });

    umTap.test('can create a user', function (cuTest) {
        initTestDB().then((context) => {
            const UserModel = context.UserModel;
            const PHOTOS = ["http://www.foo.com/bar.png", "http://www.foo.com/vey.png"];

            var user = new UserModel({
                username: 'foo',
                displayName: 'Foo Johnson',
                provider: 'twitter',
                photos: PHOTOS,
                twAuth: {
                    tokenSecret: 'foo',
                    token: 'bar'
                }
            });

            user.save(err => {
                if (err) {
                    cuTest.fail(err.message);
                }
                UserModel.findOne({username: 'foo'}, (err, foo) => {
                    cuTest.equal(foo.username, 'foo');
                    cuTest.equal(foo.displayName, 'Foo Johnson');
                    cuTest.equal(foo.provider, 'twitter');
                    cuTest.equal(foo.photos[0], PHOTOS[0]);
                    cuTest.equal(foo.photos[1], PHOTOS[1]);
                    cuTest.deepEqual(Array.from(foo.photos), PHOTOS);
                    cuTest.equal(foo.twAuth.tokenSecret, 'foo');
                    cuTest.equal(foo.twAuth.token, 'bar');
                    context.terminate(cuTest);
                });
            });
        });
    });
    umTap.end();
});

```

Some of the differences are stylistic to be sure, but the most concrete one to me is that tap doesn't assume or create any specific contextual requirements or environmental prerequisites; nor does it require a third library for assertions. And the termination of the test is explicit and directly under your control. 

These strengths are significant as I've seen a lot of the problems test generation seems to encounter have to do with shifting context, mysterious functions called into scope, and magical conventions like `this.timeout(40000)` that depend on education around the hidden contexts. Even helper utilities like `beforeEach` and `afterAll` abstract the process and the order of execution, especially as they interact with similar methods in different scopes. 

Forcing a single path inside each test scope that you manually maintain removes a lot of mystery and potential opacity from the process. Where bootstrapping code is necessary, that code is your own and directly under your power to define and control. Without `before/after` conventions you are forced to manage context wholly within the scope of the tests in question, including the problem of wrapping scope collision.

## Abstracting Boilerplate

Your create/conclude fixture code can be abstracted into its own shared library for ease of maintenance. Note how the specific model factory to produce the database model under concern is injected into the promise factory. 

```` javascript

"use strict";

require('babel-register');
const initMongoose = require('../../../models/initMongoose').default;

export default (modelFactory) =>
    new Promise((resolve) => {
        let conn;
        let dbName;
        let Model;

        const n = Math.floor(Math.random() * 1000000);
        dbName = `eyfTestDatabase${n}`;
        conn = initMongoose(`mongodb://localhost:27017/${dbName}`);

        const terminate = (test) => new Promise((resolve, reject) => {
            conn.db.dropDatabase((err) => {
                if (err) {
                    test.fail(err.message);
                    return reject(err);
                }
                conn.close();
                test.end();
                resolve();
            });
        });

        const testDB = () => {
            // @TODO: check for error
            if (conn.readyState !== 1) {
                setTimeout(testDB, 200);
            } else {
                Model = modelFactory(conn);
                resolve({
                    Model,
                    conn,
                    dbName,
                    terminate
                });
            }
        };
        testDB();
    });

````

For what its worth, here is the model code under test in this case:

```` javascript

import mongoose from 'mongoose';

export default conn => {
    try {
        let UserModel;
        const schema = new mongoose.Schema(require('./userModel.json'), {collection: 'Users'});

        schema.statics.findOrSaveTwitter = data => new Promise((resolve, reject) => {
            UserModel.findOne({provider: 'twitter', username: data.username}, (err, user) => {
                if (user) {
                    resolve(user.toJSON());
                } else {
                    var uModel = new UserModel(data);
                    uModel.save();
                    resolve(uModel.toJSON());
                }
            });
        });

        schema.statics.serialize = (data) => new Promise((resolve, reject) => {
            UserModel.findOne({provider: 'twitter', username: data.username}, (err, oldUser) => {
                if (oldUser) {
                    resolve(oldUser._id);
                } else {
                    new UserModel(data).save((err, user) => {
                        err ? reject(err) : resolve(user._id);
                    });
                }
            });
        });

        schema.statics.deserialize = (id) => {
            return new Promise((resolve, reject) => {
                UserModel.findById(id, (err, user) => {
                    if (!user) {
                        return reject({message: 'cannot find user ' + id});
                    }
                    user = user.toJSON();
                    err ? reject(err) : user ? resolve(user) : reject(new Error(`cannot find user ${id}`));
                });
            });
        };

        UserModel = conn.model('Users', schema);
        return UserModel;
    } catch (err) {
        console.log('error in UserModel:', err);
    }
};

````