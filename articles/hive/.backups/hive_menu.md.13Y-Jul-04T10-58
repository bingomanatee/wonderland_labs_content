Hive-Menu is a very simple memory-model for hierarchical collections of links. 

It has two base classes: Menu and Item. 

* Menu has an items array that contains other menus and items. 
* Item is a "leaf" link definition. 

You can define a menu structure in a single construct

``` javascript

	var menu = new hive_menu.Menu({
		name:  'foo',
		title: 'Foo',
		items: [
			{title: 'Alpha', name: 'alpha', link: '/alpha', weight: 1},
			{title: 'Beta', name: 'beta', meta: {active: true}, link: '/beta', weight: 0}
		]});

	var menu_json = menu.toJSON();

	t.deepEqual(menu_json,  {
		"name" : "foo", 
		"weight" : 0, 
		"title" : "Foo", 
		"items" : [{
			"name" : "beta",
			"weight" : 0, 
			"title" : "Beta", 
			"link" : "/beta" ,
			"type": "item",
			meta: {active: true}
		},{
			"name" : "alpha",
			"weight" : 1, 
			"title" : "Alpha", 
			"link" : "/alpha" ,
			"type": "item",
			"meta" : {
			}
		}] ,
		"type": "menu",
		"meta" : {
		}
	}, 'basic menu json');

	var beta = menu.item('beta');

	t.deepEqual(beta.toJSON(), {
		"name" : "beta",
		"weight" : 0,
		"title" : "Beta",
		"link" : "/beta",
		"type": "item",
		meta: {active: true}
	}, 'found beta');

	beta.hide = true;

	 menu_json = menu.toJSON();
	t.deepEqual(menu_json,  {
		"name" : "foo",
		"weight" : 0,
		"title" : "Foo",
		"items" : [{
			"name" : "alpha",
			"weight" : 1,
			"title" : "Alpha",
			"link" : "/alpha",
			"type": "item",
			"meta" : {
			}
		}],
		"type": "menu",
		"meta" : {
		}
	}, 'basic menu json - after hiding beta');
```

or you can build it up incrementally

``` javascript
	var menu = new hive_menu.Menu({
		name: 'foo',
		title: 'Foo'
	});


	menu.add({title: 'Alpha', name: 'alpha', link: '/alpha', weight: 1});
	menu.add(new hive_menu.Item({title: 'Beta', name: 'beta', meta: {active: true}, link: '/beta', weight: 0}));

	var menu_json = menu.toJSON();

	t.deepEqual(menu_json,  {
		"name" : "foo",
		"weight" : 0,
		"title" : "Foo",
		"items" : [{
			"name" : "beta",
			"weight" : 0,
			"title" : "Beta",
			"link" : "/beta" ,
			"type": "item",
			meta: {active: true}
		},{
			"name" : "alpha",
			"weight" : 1,
			"title" : "Alpha",
			"link" : "/alpha" ,
			"type": "item",
			"meta" : {
			}
		}] ,
		"type": "menu",
		"meta" : {
		}
	}, 'basic menu json');

	var submenu = new hive_menu.Menu({'title': 'Rick', name: 'rick', weight: 0.5});
	submenu.add({link: '/rick', title: 'RickItem'});

	menu.add(submenu);
	var smj = menu.toJSON();

	t.deepEqual(smj, {
		"name": "foo",
		"weight": 0,
		"title": "Foo",
		"items": [
			{
				"name": "beta",
				"weight": 0,
				"title": "Beta",
				"link": "/beta",
				"type": "item",
				"meta": {
					"active": true
				}
			},
			{
				"name": "rick",
				"weight": 0.5,
				"title": "Rick",
				"items": [
					{
						"name": "",
						"weight": 0,
						"title": "RickItem",
						"link": "/rick",
						"type": "item",
						"meta": {}
					}
				],
				"type": "menu",
				"meta": {}
			},
			{
				"name": "alpha",
				"weight": 1,
				"title": "Alpha",
				"link": "/alpha",
				"type": "item",
				"meta": {}
			}
		],
		"type": "menu",
		"meta": {}
	}, 'built up menu');

```

## Functionality

hive-menu has only two dynamic abilities: 

1. the ability to hide specific menus/items by setting the 'hide' property to true
2. the fact that `toJSON()` returns content sorted by weight

All other functionality has to be established with outside code.