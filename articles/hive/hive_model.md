<a href="https://github.com/bingomanatee/hive-model"><img style="position: absolute; top: 0; right: 0; border: 0;" src="https://s3.amazonaws.com/github/ribbons/forkme_right_red_aa0000.png" alt="Fork me on GitHub"></a>

Hive-Model is a basic in-memory repository of data. It is used internally for [hive-mvc](hive_mvc/hive_mvc) runtime resources, and can be a stand-in for ad-hoc data that it is not critical to save to a permanent repo. (that being said - it is not designed or intended for production level data storage functionality.)

Hive-Model records are schemaless JavaScript objects; the only requisite is a primary key field -- by default, 'id'. If none is found, it will be appended out of the _make_pk method. No other requirements are made or applied to records. 

Since Hive-Model is not asynchronous, there are few situations where its calls cannot be made inline. Allowances for callbacks exist in the API, they are optional in all cases. They exist to enable mocking a true repo that has a callback API. 

## The Dataspace

hive-models are aggregated in a Dataspace. The Dataspace is a registry of hive-models, keyed by name. It is basically a "naked [component](hive_component)", and in fact, any component could be used as a Dataspace. 

## Creating a Model

The basic methods for creating a model is to pass the mixins, configuration and dataspace into the model factory.

A hive-model is a [hive-component](hive_component), so the first property is mixed into the output instance, and the second parameter is added to the _config [hive-configuration](hive_configuration) property.&apos;

``` javascript

var hive_model = require('hive_model');

var dataspace = hive_model.Dataspace();

var model = hive_model.model({name: 'foo', data: [
   {ID: 1, name: 'Ralph'},
   {ID: 2, name: 'Bob'}
]}, {_pk: 'ID'})

```

You don't have to define your dataset initially; you can also build it up.

``` javascript

var hive_model = require('hive_model');

var dataspace = hive_model.Dataspace();

var model = hive_model.model({name: 'foo'}, {})
model.put({id: 1, name: 'Ralph'});
model.put({id: 2, name: 'Bob'});

``` 

You don't even have to define keys; they will be added by the model as needed.

``` javascript

var hive_model = require('hive_model');

var dataspace = hive_model.Dataspace();

var model = hive_model.model({name: 'foo'}, {})
model.put({name: 'Ralph'});
model.put({name: 'Bob'});

``` 

## Hive-Model API

#### get(key, cb?): record

returns a single record whose primary key == the passed key.

#### put(record, cb?): record

inserts a single record into the model. Will replace existing content with the same primary key, or add a primary key if none exists. 

#### add(array, cb?): [records]

Adds a series of records; pipes to `add()`. 

#### delete(key | record, cb?): record

Removes a record from the model. If a record is passed, its ID is extracted and used to reduce the data set.

#### count(cb?): int

returns the number of records.

#### where(function | object, cb?): Query

a syntactic variation of find

#### find(function | object, cb?) : Query

Returns a Query(see below) of a subset of records. If a function is passed it is used as a filtering function via underscore's `_.filter`.

#### update_record(id, data, cb?) || update_record(record, cb?)

extends a record with the passed-in parameters. If the ID is not passed separately, it is extracted from the input data.

#### dump(directory | file_path, callback)

Writes a JSON version of the model to the file system. If the first argument doesn't end with `(model_name).json` it will be assumed to be a folder, and will affix that name to the path. Note - this is an async operation and the callback IS required. 

#### load(directory | file_path, callback)

Loads data from a JSON file.

## Query API

A Query is a subset of the original data, as an object instance. Note in all cases, the query receives the dataset/value of the operation, not a query. 

#### one(cb?), first (cb?), last(cb?): record

Returns a single record from the dataset. 

#### records(cb?): [records]

Returns the query dataset as an array. 

#### sort(key, [reverse]?, cb?): query

sorts the queries dataset, using underscore's _.sortBy

#### slice(from, [to?], cb?)): query

slices the dataset's results



- - - - - 
&apos; hive-model "breaks the standard" of putting values in _config and functions in the mixins, for the two key properties name and data; this is for efficiency.