(Note- this is ripped from an email at work -- formatting to come)

One of Webpack’s bonuses is that it makes third party modules easier to install and inject into your code. However it does deserve a little discussion.
## Method 1: Node/NPM
If the third party module you want is in node, you can install it using NPM (the Node Package Manager) .
1.       Go to your webpack root directory. (hint: it should have a package.json in it) 
2.       Type npm install my-module-name –D * 
3.       Inside your project, type var myModule = require(‘my-module-name’). 
Interesting trivia note: that will translate to getting a reference to a hash; that is, every time you pull my-module-name down you are referring to the same object, so no matter how many times you require that module, you won’t change the memory/efficiency of your project. Similarly as singletons they will have the usual notorious side effects:

``` javascript
(my-module.js)
module.exports = {
foo: 1
}

(my-code1.js)

var mm = require (‘my-module’);
console.log(‘foo:’, mm.foo); // “foo: 1”
++mm.foo;

(my-code2.js)
var mm = require (‘my-module’);
console.log(‘foo:’, mm.foo); // “foo: 2”

```
### Advantages to npm 
1.       We “freeze” the version of our dependencies to a specific project, eliminating the need to synchronize dependencies across projects
2.       We don’t have to check in huge numbers of third-party files into GIT; only the package.json changes
3.       The projects stay self contained and are more testable.
4.       We don’t have to add multiple insertion points to our index.html every time we want to add a module
5.       Bower actually prunes unreachable/unreferenced code from your dependencies during bundling, further reducing the size and memory requirements of your bundle and increasing the speed of your app. 
6.       Modules you install but never require won’t add to your compiled project. 
A node on technical debt
Even though we are installing modules using node, webpack’s build files are not dependent on node to work properly. A webpack build module is purely client-side javascript and should run in all environments from ie9+ and above. 

## Method 2: Bower

Bower is kind of the “Client side npm”. You should use Bower If

1.       Something you want isn’t in NPM.
2.       For some reason the NPM version of the module doesn’t work in Bower.
3.       You actually want to hack the source files of the vendor module.

Using bower has advantages over the “download and unzip” school of vendor package installation for one, It will do any post-download compilation necessary (usually). For another, its (in general) a one-step operation, once your environment is set up. 

### Setting Up Bower

Bower is itself a node module.** to install it you must enter npm install –g bower. You may run into file conflicts that Howie has to resolve for you, to unlock your global node-modules folders. 
Once you have done this go into your working directory and type bower install my-module –D. 

This will install your vendor files in the directory specified in the .bowerrc folder. 

After you have installed these files, you must put css and javascript links into the index.html files; you can’t use webpack require to pull in bower modules, as there may be closure side effects to doing so. Remember in the ngPOA branch there are several copies of index.html, each of which must be kept up to date. 

### Advantages to Bower

1.       It is in general faster than downloading, messing with the file system, unzipping, etc. 
2.       If your bower directory ever becomes corrupted/altered you can delete it and type bower install to rebuild all your dependencies.
3.       If you ever want to bump to the next version of a dependency, you just have to run 

```
bower uninstall my-module –D` (clears the files, and the bower.json record
bower install my-module –D`
```
or 
``` 
bower install my-module#1.2.3 -D
```
 to target a specific release

### Disadvantages to Bower

1.       You won’t have the ability to munge several dependencies (css, js, images) into your webpack build file; the number of HTTP requests will be larger on a bower based project than one using npm + webpack
2.       The GIT repo of your project will have a lot more files. 
3.       Bower isn’t quite as elegant as node in that you have to sync up your index.html file after installation
4.       The webpack bundler actually prunes unreachable code during compilation, but it won’t be pruning your bower files. 
5.       Once you add a bower utility to your index.html file it stays there even if you never use it, adding to project size until you manually prune it.

Despite all these issues its still better than the manual method. 

The only hitch you may run into is if you don’t have a .bowerrc file (shouldn’t happen in ngPOA/oopis as we already have set up configuration). Review http://bower.io/docs/config/ for notes on the .boweerrc file. Your .bowerrc should NOT have http://localhost:8000 in its search path – just the bower.herokuapp.com path. 

___________________________________________________________

*  there are three ways to install npm modules: 
1. without changing your package.json file: npm install my-module-name will install your module in the place you are working in but will NOT change your package.json file. This is the WORST option; because the files you install into node_modules are NOT tracked in git; only the package.json file is. So your code will work fine, but after you push and someone else pulls the file, your my-module-name module will NOT be installed for them and they may even install a slightly different version of that module; all bad things. 
2. as a “Dev Dependency”: npm install my-module-name -D (identical to npm install my-module-name --save-dev) will record the module as a “devDependency” in your package.json file. The basic upshot of dev-dependencies are that they are needed to build the project, but once the project is built, they are not needed for it to run in production. Since that is exactly what happens when you build the webpack module; the required files are pulled directly into your build files and are no longer directly referenced. (proof of which being, when the webpack build runs, there are no url calls made to /…/node_modules/…/ anywhere) 
3. as a dependency: npm install my-module-name -S  (identical to npm install my-module-name --save) records the module as a “dependency”. This will (for our purposes have exactly the same effect for our project, but the semantics aren’t quite as proper as option 2. 
4. (the worst of all) npm install –g my-module-name will install the module in your operating system’s global directory. (where exactly that is varies by OS). This can have side effects to other projects on your environment and has the same problems as 1. Above. 
** I’m sure if they could they’d make it a bower module but there’s obvious issues with that approach…