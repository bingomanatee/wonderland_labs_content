As mentioned in Resources, data models in Hive are very "hands off". There are almost no API requirements for a model instance -- it is just a javascript object, on which any sort of API can be applied. For instance, you can embed a Mongooose collection Model as a Hive model.

The only requirement for the model object is that it have a name field, so that it may be retrieved by name from the model. 

This is the basic API for defining a model in Hive:

``` javascript

module.exports = function (apiary, cb) {
// your custom code here
cb(null, my_model);
};

```

The Apiary is available to the model class as a resource, for accessing configurations etc. The model is passed out of the model definition up to the Apiary on bootstrap.