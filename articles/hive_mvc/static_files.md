Unlike conventional Express websites that use a single folder for all client side resources (static files), Express allows you to have innumerable static folders, allowing you to store javascript and CSS specific to a particular action inside the action folder. 

?[Frames](frames), ?[Hives](hives), ?[Actions](actions) and ?[Layouts](hive_layout) can all have static folders. 

## Defining a Static Folder

Static folders store all their resources in one or more subfolders. Typical subfolders include `js`, `css`, `img`, etc.; however you can name your static folders anything you like. 

Static folders are designated in the ?[configuration](config_files) of the host via a `static` JSON node. For instance, if you have an action "/foo", you might have the following definition in `foo_config.json`: 

``` json

{
  "routes": {
    "get": "/foo/foo"
  },
  "static": {
    "js": "/js/foo",
    "img": "/js/img"
  }
}

```

this would map the file `[site]/frames/foo_frame/hives/foo/static/img/foo_fighter.jpg` to the url `http://www.mysite.com/foo/img/foo_fighter.jpg`.

Note, this allows you to create the illusion of centralizing client side resources in virtual folders that may or may not exist. 

Again - you can define the "url prefix" of your static resources in any way you like. 

To be clear: just putting folders in the static folder will not automatically publish them, and each folder in the static directory must be specifically mapped to an alias, or its contents will not be published. 

* * * 

While you CAN store static files in this fashion, you do not HAVE to. You can still store all your static files in a `public` folder, and/or you can store all the static files for a ?[frame](frames) in a single static folder. 

You can also use an Express `public` folder for single source static file distribution.

## Static Folder contents

There is no limit to what you can put into static folders, including subfolders/subfiles, etc. That is, just because a folder is called "js/" or "javascript/" doesn't mean it can't hold CSS files, images, etc. 

It is quite okay to put whole vender folders of JS content inside a JS file and still serve both javascript, CSS files, images, JSON, etc. from that folder. 

## Static Files via Actions

You can very easily serve static files via an action:

#### configuration file

``` json
{
  "routes": {"get": "/myfiles/*"}
}
```

#### action script

``` javascript
var path = require('path');
var MY_FILE_ROOT = '/users/bob/MyStaticFiles';
var fs = require('fs');

module.exports = {

  on_input: function(context, done){
    var url = context.$req.url;
    context._path = url.replace(/.*myfiles\/, '');  
    done();
  },

  on_process: function(context, done){
    var file_path = path.resolve(MY_FILE_ROOT, context._path);
    fs.exists(thumb_path, function (ex) {
       if (ex){
         context.$sendfile(file_path, done);
       } else {
         done (new Error('cannot find file ' + file_path);
       }
    });
  }

}

```

This action will be pretty responsive, as no view_helpers are triggered and you skip the whole templating/ejs process. 

The missing file handling is pretty brutal here; possible alternatives is to have a "404 file" or a custom JSON blob with error data.

## Choosing aliases for static file folders

There is no "Law" for how to choose the prefix of a given folder. Also, the name of the folder is not added to the path of a folder. That is, if "js" and "javascript" have the same contents, then 

``` json 
{
   "static": {
         "js": "/js/foo"
      }
}
```
and

``` json 
{
   "static": {
         "javascript": "/js/foo"
      }
}
```

will result in the same effect, from the browsers' point of view. 

You can give the same alias to more than one static collection. As long as you're sure that the actual FILES in each static folder is unique, you can for instance map all the javascript in each one of your actions as `{"js": "/js"}`.