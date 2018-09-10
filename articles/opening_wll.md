<a href="https://github.com/bingomanatee/wonderland"><img style="position: absolute; top: 0; right: 0; border: 0;" src="https://s3.amazonaws.com/github/ribbons/forkme_right_red_aa0000.png" alt="Fork me on GitHub"></a>

The source code for this site is now open to the public. I have not taken the time to license it formally, but it is my hope that others will find it a useful base for their own work. 

note, that the node modules on which it depends are not included, so you will have to `npm install` in both the root and several frames to get the modules in place. 

Also, there are configuration files that are not bundled in the repo, for things like oAuth keys. 

Check out https://github.com/bingomanatee/wonderland_labs for the source. 

## Module (server side) dependencies

The site is hosted on Ubuntu/Linode and developed under MacOSX. It uses node 10.x. 

Several modules are used sitewide for functionality, as documented in package.json

1. **hive-menu** provides the superstructure for the sidebar navigation menus. These menus are built up through view-helpers that use the member ACL to vary the menu by the users' permissions. 
2. **hive-model-mongoose** (and of course **mongoose**) is used as (one of) the repo system. Note that the site is very "database light"; only member identities and social data are stored in the mongo db. 

## The frames

Frames are reusable components that the hive-mvc framework depends on. 

There are several frames that are designed to be modular and reusable. And there are other ones too :D

### The Member frame

Membership is accomplished via twitter or facebook logins. it uses passport.js so other oAuth providers should be easy to add; you could even use passport.js' "local" api to store login details locally.

The member frame includes a full ACL system. The definitions of roles and actions are harvested from frame configuration files. 

It depends on a Mongo database and mongoose to store members. 

### The Social Frame

This is a very new frame devoted to displaying social data off your oauth account. It is fairly new (rough).

### The Hive-Layout frame

I waffled back and forth about putting this in core; you can't really have a feasible site without a layout engine. But at this point the ability to embed layouts around your content is a specific frame. 

Layouts are chosen by adding a `layout_name` property to your output. They don't apply to raw/JSON data that you `context.$send({...})`. 

### The Hive-Messages frame

This is a simple alert system for alerting users to errors, state changes, etc. It is synchronous through the rendering system. It requires embedding `<%- helpers.message_alert() %>` in your layout/page markup. Displaying messages in this manner also clears them. 

### The Admin frame 

The Admin frame contains a layout and a menu and some introspection modules that allow you an overview of site resources. The pages it displays are a mixture of local admin hive actions in other modules and a few actions of its own.

### The Blog frame

The Blog frame uses a file-based repo for article storage. (articles are too important to be put into a database :) It has markdown editors and a folder system for categorizing articles. It has ACL for article creation/editing and the ability to upload/embed images.

## Client side resources

* Angular.js and Twitter Bootstrap are used in abundance on this site. 
* A host of related extensions like the Angluar Bootstrap UI and typeahead are used surgically. 
* Isotope is used for article block display in the folder view.