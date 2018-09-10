Resources are an integral part of the hive-mvc architecture. A resource resembles a Node module. However, unlike modules, that are

* (usually) stored in NPM 
* use the node CommonJS system to load 

* harvested from various resource folders in the frames by the [apiary](apiary) initialization process
* are indexed by name and type, 
* available globally to the running site at runtime. 

Each type of resource has a unique signature. The only thing all resources have in common is two properties: a name and a type. The type is inferred from the name of the containing folder, and the name can be inferred from the file name of the resources.

They are all indexed in the apiaries' Resource.list, a Hive-Model instance. (see ?[hive-model](hive/hive_model) for details) 

## the resources folder

resources are contained within a folder in a [frame](frames, hives), or [actions](actions) folder named `resources`. 

Inside the resources folder can be one or more folder named `models`, `mixins`, or `view_helpers`. 

Inside those folders are the resource scripts themselves. Any javascript file in this directory will be parsed as a resource. You can however put files of other types (html, json, other folders) to house content for these helpers. 

## model resources

A model resource is designed to manage a set of data. This can be 
* a wrapper for a file based repo (as with the `blog_article` model
* a gateway to a SQL/NOSQL repo (mongoose, PostGres, MySQL)
* a memory model ([hive-model](hive/hive_model))
* a handler for a REST datasource (passport.js)

There is no API requirement of a model resource. However there is a requirement that the resource script follow this basic format:

``` javascript

module.exports = function (apiary, cb) {
// your custom code here
cb(null, my_model);
};

```

and like all resources, my_model has to have a unique name property. 

If for some reason this creates a problem with your model system, you can wrap your model in an object. 

``` javascript

module.exports = function (apiary, cb) {
// your custom code here
cb(null, {name: 'foo_model', model: myFussyModel});
};

```

Models are available globally through a `model(name)` method of actions and the apiary. 

## View-Helpers

View-helpers are covered in detail [here](view_helpers). 

``` javascript

module.exports = function (apiary, cb) {

	var helper = {

		test: function (ctx, output) {
		    return true;
		},

		weight: 0,

		respond: function (ctx, output, done) {
			ctx.link = _.template('<a href="<%= href %>"><%- text %></a>');
                        done(); // note - no parameters required for this callback
		}
	};

	cb(null, helper); // note - MUST pass the manifest to this callback. 
};

```

## Mixins

Mixins allow you to define 
The mixin resource type's respond method is executed once, after all the resources/actions have been loaded but before the site begins responding to HTTP requests. They are called in order of their weight property. 

This is the signature of the mixin resource:

``` javascript

module.exports = function (apiary, cb) {
	cb(null, {
		name:    'gamma',
                weight: 10,
		respond: function (done) {
                   // mess with the apiary, actions, etc. 
                 }
          }); // end callback
}


```

One common use case for mixins is to harvest and reapond to values in hive, frame or action configurations.

Here, for instance, is a mixin that is part of the members ACL system. It pulls member_actions and member_roles properties from frames or hives into the in-memory ACL task models. 

``` javascript

module.exports = function (apiary, callback) {

	callback(null, {
		name:    'update_member_actions',
		respond: function (done) {

			var role_model = apiary.model('member_role');
			var member_model = apiary.model('member_action');

			apiary.Frame.list.all().records(function(err, frames){

				var gate = Gate.create();

				frames.forEach(function(frame){
				//	console.log('frame: %s', util.inspect(frame, false, 0));
					if (frame.has_config('member_actions')){
						frame.get_config('member_actions').forEach(function(action){
							member_model.find_or_add_action(action, gate.latch());
						})
					}

					if (frame.has_config('member_roles')){
						frame.get_config('member_roles').forEach(function(role){
						//  	console.log('adding role %s', util.inspect(role));
							role_model.find_or_add_role(role, gate.latch());
						});
					}

				});

				gate.await(done);
			});

		}
	})

}; // end export function

```