I was asked recently how I would version things -- specifically, what I would lock down; and it occurred to me that versioning covers two things that aren't always easy to sum up in a single number:

1. the **interface/API** -- that is what functions and classes are exposed, their parameters, and their output formats.
2. the **code that satisfies the API**. 

If you change the first, the code that depends on a system will probably break (depending on which and how much of the API you use and how you use it.)

However, if you change the code, whether or not you see changes to your use of the module is up in the air. It might perform better, worse or the same. 

In a perfect world, a given depth of a semantic version would indicate whether the code or the API has changed. I haven't seen this in practice, and in reality, I would almost rather see the API and the code to each have their own version number. 

Then, in the requesting specification, you could decide how deep you wanted to lock your code. By definition, your dependency would lock to a specific API. Whether to lock to the code version would be up to you; at the minimum, you'd want to lock to at or after the current code number. 

Also, in fairness, when the API number changed, the code number would have to change as well -- you can't change the API without changing the code.