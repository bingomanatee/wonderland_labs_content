As an older programmer, my first whiff of Testing/TDD was "Oh, this is what N00b coders do when they lack confidence in their own work." 

Eventually I began to realize that there were a lot of ways that TDD really countered natural development psychology. 

## The fundamental economy of development is developing reusable components

If we had to rewrite every function for every use case nothing would ever get done. The fact that we reuse code also means that we expect the way we used a given block yesterday should function even as we expand its functionality to meet todays' needs. 

Especially as developers start to use other peoples code, this becomes relevant. If you use someone else's code, you will eventually find bugs in other people's code, and fix bugs in other peoples code. However, if dev A writes a function, dev B uses the function and notices the bug but codes around it, and dev C fixes the bug, dev C's fix will screw dev B's code because its using the bug as a feature. 

Without testing all around, its very easy to create side effects in other peoples' work and only see the result when it breaks in live use. 

## Its often the only way to test the boundaries of use

This is about more than "Code Coverage." I still think code coverage is far less important than use case coverage. 

Using fake data is frequently the only way to test certain conditions. For example I was working on a data grid in which some transactions were in "Pending" state -- that is, a check posted to a bank has not been fully accepted by the bank but still appears in your list of transactions. 

Manual testing based on real data, you might go quite a long time before you run across a real pending transaction. And even if you did, it might get fulfilled before the dev got a chance to look at it. (or while.) 

So testing for known conditions is often far easier in a test scenario than a real one. 

Similarly, when you have a known set of values -- or even an intersection of two known state sets -- you can produce full tests for all intersections of known values and instantly see fall throughs in odd states. In real life, manually or existentially testing for all intersections of conditions is a crap shoot.

## A program is a very long chain of trust. Finding the weakest link without tests is very tiresome. 

Real world code goes in and out of envelopes and pipes a lot. When code breaks, it is good to have tests along the pipes so that you can isolate and affirm where the pipe is breaking. Manually isolating goodness is very rough trade, and without tests in place, tiresome and stressful.

## Refactoring Production Code 

Refactoring open source/personal projects is easy and fun. Refactoring code that real time customers rely on is difficult and risky, especially without coverage. QA teams are usually full-time devoted to the latest releases. Convincing a manager that you need to fix or upgrade a package, especially one that is not creating a bug on the hit list, is a very difficult argument.

That is one reason that production code tends to be crap. 

Most things in life require a second or even third pass to be good. Some time off, some use and some reflection generally improves the perspective and quality of your work. Its tough to find another industry where product revision and refinement are given so little energy. 

Tests can reduce the fear factor for refactoring, and give you more leverage to go back and fix things by minimizing the blowback.