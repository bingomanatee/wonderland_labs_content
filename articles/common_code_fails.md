There are a lot of patterns for successful design patterns. here are a few "Antipatterns" that consistently cause problems. 

## TL;DR

Make sure your codebase 

* Handles bad and malformed data routinely
* Handles cases of process failure and interruption well
* Centralizes business processes for rapid upgrading of key algorithms across the codebase
* Always design components that do not depend on uniqueness -- test the ability for multiple versions to coexist

## Plan first

Seems a little bland - but every substantial codebase should begin with at least a little forecasting - graphs of data structures, throwaway experiments, process flows, etc.

If you can't think of at least 

1.  two plausible ways to solve the larger problems
2.  one scenario in which your current approach is not tenable

then you probably have not really thought your thing through effectively.

## Create clear boundaries of responsibility

It may sound like this is equivalent of "writing modular code" -- it's not. As much as possible write contained systems that pass events and data through them responsibly. 

* Let the view layer be its own system. 
* Have a strong REST/ data access layer that is independent
* Keep your business/logic/rules system clean and abstracted, for easy upgrading and changing. 

The more you have clear boundaries between these systems, the easier they are to build and change. More importantly, the easier they are to test and if necessary, replace. 

This also makes the process of systems documentation clearer. Your documents become api descriptions, not long winding stories of "How the user record goes from the web page to the database."

## Don't trust the existence of deep properties

When you call a deep sub-property without validating the entire chain, you make a lot of assumptions. 

``` javascript

var foo = a.bar.vey.id. 

```

presumes the existence of `a.bar` (and `a.bar.vey`) implicitly. Fine in many use cases but its a ticking time bomb. Similarly, assuming that `a[b]` exists is a measured risk that will eventually blow up in your face. 

Each time you poll data from a source, you should 

1.  evaluate the possibility it does not exist
2.  evaluate the possibility that it is of a different type than your code expects
3.  handle and log failures of 1. and 2. effectively

If you find yourself pulling the same long paths consistently create a failure-tolerant access method such as

``` javascript

function fooBarVey(ifNotFound){
  if (foo && foo.bar && foo.bar.hasOwnProperty('vey')) { return foo.bar.vey; } else 
  {
    return null; // or throw error or ifNotFound()
  }
}
```

In fact, any time you find yourself reaching across your codebase for values frequently, creating an abstracted accessor like the above is handy, so that if your API changes, you have fewer maintenance points to adjust. 

## Compress repeated patterns into single methods

Minimizing the amount of procedure you scatter in your codebase makes maintenance much easier: you have a single method to change when your algorithm or schema changes. ActiveRecord scopes 
are one example of a reduction of a repeated algorithm that has been concatenated into a single statement. 

## Splitting your view logic between the server and the client

This is an artifact of aging development patterns -- and a symptom that your dev team has not conquered client side code effectively. 

You should in general have a minimum of server side view templating. This doesn't mean every app has to be a single page app, but the less you shuttle back and forth between the server and client the faster your site is, [the happier your users are.](http://www.youtube.com/watch?v=arz3jhMfUx0). This means its better to send up a few rest blocks and let your client dynamically generate your views than to slush your entire view markup from the server to the client constantly.

Further, the less work your servers do, the more responsive they are to load. Which of course, also speeds up your web experience. 

In short just because you can munch markup on the server doesn't mean you should.

## Don't use OOP patterns for every problem

The myth of intelligent objects really obviates the nightmare that is maintaining a system that dips too deeply into the objective programming pattern. What's wrong with this? Well, if you have ten classes each of which have five methods to accomplish the same pattern you have to write fifty blocks of code. And test fifty blocks of code. And if you add a single method to your vocabulary, you have to do so ten times. 

Yes there are patterns of helper classes, decorators et. all but this puts you into the obligation of making those methods handle communicating with their parent class for context -- this is not always easy or tenable. 

By contrast if you have configurable procedures, you can pass your objects through them and once you validate the basic premise of each procedure you can pretty much trust it for an indefinite population of objects. 

For instance, if your app needs a soft delete/archive pattern, you can go one of two (at least) paths: 

1) create an `.archive()` method on every object in your codebase or 
2) create an `.archive_data(record, fields, archive_table)` procedure/library call, and pass any number of objects through this. 

the method in 2) may be then added as object methods, but the fact that the meat of your work is being done in a single solver drastically reduces maintenance cost if you end up changing your approach.

The more code I write, the dumber my objects get. At most, I have them get and set properties and sub-resources. 

Procedures are easier to put into a test framework. They are easier to add to a large existing framework. They are easier to adapt to a polymorphic set of classes. Also, they don't obligate you to add them to objects for which they are not appropriate simply to satisfy an interface. 

Most importantly, when they must be changed, you don't end up shuffling through dozens of code files to keep your system in parallel. 

## Failing to write contingency code

Code is failure prone: remote services go down, edge cases emerge, databases corrupt, and file systems get damaged. 
Every single line of your code has a non-zero chance of producing foul-ups, but code that interacts with an outside system (file, database, network, web service) are particularly chaotic. 

One variation of this is partial branches. For instance, cases without a "default" handler for "none of the above" data or "if/then" statements without an "else". *Even if you don't want to execute code* in an else clause you should have one and put comments as to why it is empty. 

Similarly, if you have iterative code (looping over a container), your code should consider the consequence of the array being empty. 

``` javascript

var denom = 0;
myArray.forEach(function(value){
++denom; 
});

var weight = 1 / denom; 

```

will fail (div/0) if myArray is empty. Similarly, any variable settings, method calls, etc. in the iterator callback won't trigger if there is no content to loop over. 

## Trusting your callbacks to call back. 

Node in particular is rife with callbacks. However there is always the chance that a remote system for whatever reason will hang your code, resulting in no response (as different from a response with an error). 

Putting timeout checks on each callback (especially those dependant on remote web services/networks) allows your app to keep functioning regardless of the state of the rest of the environment. Typically this looks like:

``` javascript

var timeout = false;
var timer = setTimeout(function(){
  timeout = true;
  handleLackOfData();
}, 5000);

getData(function(err, myData){
  if (timeout) { 
    handleData(new Error('timed out'));
  } else {
    clearTimeout(timer); // prevents the failsafe from triggering
    handleData(err, data);
  }
});

```
This might look strange if JavaScript isn't your native tongue but essentially this code does the following:

1. Sets up an error to trigger after 5000 milliseconds (5 seconds). 
2. Saves a hook (timer) that will kill process 1.
3. Calls a long running process (getData). 
4. Injects a check in the long running process that, in the case that the failsafe triggers, stops the callback from going forth. 
5. Has code (`clearTimeout`) in the process so that if the process finishes before the failsafe, the failsafe is "disarmed". 

Tedious? yes -- but its necessary to prevent your program from "running off a cliff."

## Not managing processes

Every process that either (a) takes significant time to execute, (b) depends on a remote resource (network, database, networked database) or (c) both 
has the potential to not return. This occurs when

* The network is down
* Your connection to the network is down
* The database has to loop over a huge amount of data
* The remote endpoint fails messily
* Any other non-perfect result occurs

Any call depending on remote resources has the potential to kick off this kind of resource munching catastrophe. 

### What are my options?

This varies from platform to platform.

* If your language has threads (Java, Python) then using threads for these kind of tasks is the best solution. However you should monitor these threads and cut them off if excessive time or resources are consumed by them (and move on to clean error trapping from there). 
* If you are in the web (and trust web workers) then using web workers might be the ticket. At the very least, promise based coding makes long processes less blocking.
* If you are using node, you can use callback based processes and trust the event loop to minimize resource impact. (see callbacks above). 
* Any system running on a Linux/unix platform should have access to unix process based programming. 

Overall, this is the basis for the virtues of [Service based architecture](http://en.wikipedia.org/wiki/Service-oriented_architecture) -- it allows you to export and manage resource issues in a segmented manner. But of course it requires a network of processes, any of which can fail or get locked into a long running cycle so don't expect that you can export all of the responsibility for managing processes.

## Coding everything from scratch.

Reusing code is not just about saving time. Its about minimizing the amount of debugging you have to do and maximizing the ability of multiple groups of people to provide the best solving and checking of processes. 

It's easy to look at a stock problem like templating and say, "I can make a faster/easier to read/better templating system than marked/ejs/jade/...." 

However there is a fairly constant ratio of lines of code to bugs: no matter how clever you are, the code you produce will have bugs and you will be on your own to fix them. If, on the other hand you use ejs/marked (and even better, help to make them more efficient), if you find a bug, you will likely find that others in stackOverflow have found and solved the problem already. 

The bulk of an efficient engineer's time should be spent on solving business problems. There are very few truly new low level problems, and by using public domain code/modules/gems you drastically boost efficiency and make sure that if you stumble, you will have a vast library of public resources to get you back on track.

Even fundamental procedures -- sorting, filtering, etc. -- have been executed and solved very well. As a rule, if you feel like you are solving a Computer Science textbook problem, odds are you will find resources to do so on github.

To be clear: this doesn't mean that business logic/high level solutions are always the best. Innumerable people have tried and failed to cram their companies' systems into a stock CRM (or Drupal) and found that they have hanged themselves by buying off on an overly restrictive framework. 

Any time you find yourself creating torturous data structures to conform to someone elses' pre-fab codebase or telling your boss "The codebase doesn't do that" (or knowing that you will have to say this a lot) you may be walking down a dark alley. 

The symptom of a restrictive codebase is when it has a large number of interdependent systems that you can't easily switch out for other (private or open source) frameworks. If, for instance, your framework determines your choice of data repository (mySQL, postGres, mongo, etc.,) the chance is the system is too monolithic. Similarly, if the code you are considering forces you to use a single specific templating system, you should be cautious. 

## Design for multiple instances 

It is rare that you will be forced to create singletons - usually they are the product of an intersection of limited vision and impatience. 

The last time this bit me on the ass was when I was designing a datagrid. I created a single event arbiter for tracking things like sort keys, pagination, et. al. My coworker who'd been with the company longer kept shaking his head. I found out shortly. The grid was in a Backbone view context, and the views were switched with animation -- meaning that for a short period, during transitions, two grids existed at once. Worse, the old views were not purged -- just hidden. 
So events from one view would communicate with the single event broker, and cause updates on another view. 

This nuttiness could have been avoided by tying the grid's event dependencies to the views themselves. But fundamentally it was the result of a rookie fail -- using a singleton design pattern to begin with. 

## Enjoy your code. 

If your codebase is not fun to use and maintain, your life will be unpleasant. And your hours will be long. 

If a given chunk of code makes you sad, find out why, and make it happy code. 

Most importantly, don't let bad code survive just because "We worked so hard on it." I have never seen a case where leaving large, ugly badly written legacy code in place has paid off. Every time I have went in and replaced bad, sad code with newer better code, it pays for itself almost immediately. 

Have courage, and most importantly, have a set of validation expectations in place before, during and after a refactor. 

And never wait for management to "Authorize" a refactor. People enmeshed in business concerns have zero attention span for the quality of codebases. Its an engineers onus to be the one that drives code refactor, and involving non-technical  people in this process is a copout and a waste of time.