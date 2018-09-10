If removing Lodash from your codebase feels like a good economy you are not doing it right. 

## Going Lodash

There are three reasons to use lodash:

1. Using methods that are in flux in EcmaScript without caring about where in the flux your codebase is
2. Not writing basic mapping boilerplate over and over again
3. Chaining functional technique in an elegant way. 

Also, 
4. Lodash can be reduced to ONLY the functionality you use with custom builds, so you should rarely have to be concrened about its size as a library

I'm not going to talk about the first because that is the way most people use it and then trim it when they get better control over their environment

so starting with #2:

## #2: Eliminating boilerplate

Here are a few of the methods that reduce boilerplate on a consistent basis in Javascript: 

* *groupBy*
* *sortBy* 
* *min*/*max*
* *flatten*
* *compact*
* *uniq*

Honestly: how many times do you sort things in your code? or squeeze blankies out of arrays? or find the largest or smallest value in a series? or eliminate duplicates? all the f**king time. And the systems you write to do those are not spectacular or often, thoroughly test covered. 

There are a mess of utility methods in Lodash for strings, arrays, objects and collections; *groupBy* is one I keep going back to to find things in buckets. Say you have numbers you want to group by their integral value. Its a one liner: `_.groupBy(values, _.floor)`. 