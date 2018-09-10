Hive-MVC is designed to work on top of an existing express.js site. The only caveat is that at this point, the only templating engine that is currently active is EJS. 

Hive-MVC boostraps __after__ the express app has connected. It adds routes to express, so no specific "use" tasks are required . 

Here is an example of the bootstrap script in `app.js`: 

``` javascript

 /**
 * Module dependencies.
 */

var express = require('express')
	, http = require('http')
	, path = require('path')
	, util = require('util')
	, mvc = require('hive-mvc')
	, passport = require('passport')
	, mongoose = require('mongoose');

var app = express();
var PORT = 80;

app.configure(function () {
	app.set('port', PORT);
	app.set('views', __dirname + '/views');

// note, binding ejs to HTML files
	app.set('view engine', 'ejs');
	app.engine('html', require('ejs').renderFile);

	app.use(express.favicon());
	app.use(express.logger('dev'));
	app.use(express.bodyParser());
	app.use(express.methodOverride());
	app.use(express.cookieParser('fuzzy little pizzas'));
	app.use(express.session());
	app.use(passport.initialize());
	app.use(passport.session());
	app.use(app.router);
	app.use(express.static(path.join(__dirname, 'public')));

});

app.configure('development', function () {
	app.use(express.errorHandler());
});

server = http.createServer(app);
server.on('close', function () {
	console.log('======== closing server');
});

server.listen(app.get('port'), function () {

/**
* here is the custom Hive-MVC script:
*/
        // if you are using mongoose, bind a required version to the apiary's configurations. 
	mongoose.connect('mongodb://localhost/wll');
	var apiary = mvc.Apiary({mongoose: mongoose}, __dirname + '/frames');

	console.log('initializing apiary for port %s', PORT);

	apiary.init(function () {
        // at this point, the apiary has loaded its resources, 
        // but has not begun to respond to routes. 
		console.log('serving');
		apiary.serve(app, server);
	});
});


```