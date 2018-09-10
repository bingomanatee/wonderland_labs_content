The Apiary is the central sandbox of Hive-MVC. 

It is a singleton for your application and has a host of attached properties:

* The express server and app.
* The ?[Actions](actions), via `apiary.Action.list`
* The ?[hives and frames](hives_and_frames), via `apiary.Frame.list` and `apiary.Hive.list`.
* The ?[resources](resources), via `apiary.Resource.list`
* a ?[dataspace](hive/dataspace) of models

Having a central sandbox makes it easier to manage site resources and to extract them when necessary for testing. It is what allows you to define resources in one hive or frame and have them accessible sitewide. 

The ?[bootstrapping](bootstrapping) documentation describes how to create and initialize an Apiary instance. For the most part the apiary acts in the background; however, it is made available in the factory for each [resource](resources) so that its global dictionaries can be made available in the construction and execution of resources. 

In each action you can access the Apiary by calling `this.get_config('apiary')`. 

## API

The Apiary has several properties and collections.

#### model(name {string})

Retrieve a named model. Models are all loaded by the time apiary.init() is called, so they are available to all ?[mixins](resources). 

#### apiary.Context(request, resource, action)

Creates a context instance. Operates in the background for all request responses, but useful for creating contexts when ?[testing actions](testing_actions). 

#### apiary.Action(mixins, config), apiary.Action.list

The factory for new actions. Useful in ?[testing](testing_actions) to create a "stub" action to respond to a context, when you want to test a [view_resource](view_resources);

``` javascript
apiary.Resource.list.find({TYPE: 'view_helper', name: 'message_view_helper'}).one(
		function (err, view_helper) {

					var action_stub = apiary.Action({}, {});
					var context = apiary.Context({session: {}, method: 'get'}, {}, action_stub);
					context.add_message('a message', 'info');

					t.deepEqual(context.$session('messages'), [
						{
							"text": "a message",
							"key":  "info"
						}
					], 'messages in storage');

					action_stub.respond(context, function (err, out) {
						var out = context.$out.valueOf();
						view_helper.respond(context, out, function () {

							t.ok(out.helpers.has_messages(), 'helper has messages');
							var messages = out.helpers.messages();
							t.deepEqual(messages, {info: ['a message']}, 'messages are ordered');
							t.ok(!out.helpers.has_messages(), ' helper no longer has messages');
							t.end();
						});
					});
});

```

but mainly useful for its' list property that is a ?[model](hive/hive_model) that gives you the ability to retrieve actions by relative path as in 

``` javascript

var createAccount2action = apiary.Action.list.get('/api/hives/newaccount/actions/createAccount2');

```

#### apiary.Resource, apiary.Resource.list

The registry of resources; mainly useful for retrieving a named resource for testing.

## Events and Application Item Filters

The Apiary is a node event handler; so any event responsiveness you want to do with the Apiary can be done with standard Node event processing.

Also, there are special processes for ensuring that every ?[Frame, Hive](hives_and_frames) and ?[action](actions) are processed by a given function. These are kind of "async iterators" which process any existing resource now, and any item loaded in the future. 

* ** apiary.on_hive(handler)**
* ** apiary.on_frame(handler)**
* ** apiary.on_action(handler)**

`handler` in this case is a function designed to receive an element of the given type. It is called immediately on each registered element, and then called every time an element of that type is added. 

This is good insulation against, say, an application that dynamically extends its handlers via interactions, configuration, responses to the data model, etc.  

In the Mixin `response` context you can be sure that all the resources have been loaded prior to execution; however it is just safer to trust evented emission of these resources than to poll the apiary lists directly, in the event that some code in the future dynamically creates resources.