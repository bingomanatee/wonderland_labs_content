When I was studying programming last Millenium, the backward system we had was terminal driven. We would code on a c-prompt, "submit" our job to the task processor, and go to the printout room and wait for our job to come back on tiger stripe paper. 

Up to a half hour. 

For jobs like calculate the Fibonacci sequence to 100 numbers". 

I don't know any programmer who would put up with this level of feedback but so often we work on interfaces that delay feedback to an amazing degree

## The Backender's dilemma

The classic circumstance where this occurs is when you submit a form and it comes back with errors. This is generally when a backend contractor is asked to make a site. From his point of view you're "conserving energy" because you have to write validators for input data, so why duplicate your work on the client when you're obligated to perform it on the server as well. 

But as a user experience this is awful. When the failure comes back they have to hunt and pick out the error within their form which can be extensive, and some fields (uploads/passwords/credit card numbers) will ALWAYS have to be re-entered even if they aren't erroneous. 

As a counterpoint, Angular + Bootstrap + HTML5 allows (relatively) easy validation hooks in bootstrap forms so that when a user enters a field, you can display an error adjacent (and immediately after) a field value has been entered. This process is even intelligent about dirty fields; i.e., a required field is not an error until you try and submit the form. 

Even HTML5 + Chrome tags like "required", "min", "max" will provide the user with a better level of instant feedback. 

## Delayed feedback, Leap style

One area of delayed feedback we are wrestling with in Leap Motion Controller is that with the new Skeleton interface, we are asking for the user to accept a bit of delay when they first stick their hands into the detection cone. 

![Leap Image](http://www.blogcdn.com/www.engadget.com/media/2013/03/leapnasa01.jpg)

This makes the Leap Motion Controllers' job of recognizing a hand a bit easier but makes it tough to design interfaces where quickly swiping the hand into and out of the Controller's cone of vision.

I'm sure that that after we identified the problem we will resolve this but it indicates how often an engineers definition of a "solution" is the user/developer's definition of a problem. 

As soon as you stick your hand into a leep you really want some sort of feedback that the leap is working and your hand is still a hand -- as well, you want the user to know where there hand is relative to the controller -- where it is in the "interaction box" space.

![interaction box](https://developer.leapmotion.com/documentation/images/Leap_InteractionBox.png)

## Mini Mouse

This is similar to the "where's my mouse" problem I still have. When I move my mouse after a bit of idle time especially on a big and/or multimonitor environment, I wish it got huge so I could find it. Its especially distressing when I'm doing a spoken presentation where I shift in and out of personal/computer mode. 

## Windows and Forms

When you have several windows in a tile arrangement, generally the first click into a window activates it but doesn't do what mouse clicking would do if it were already active. I wish there were a much more prominent feedback when a window goes active (and by definition, another window goes inactive). To let me know that THAT was the result of my mouse click, so I know to click again.

For instance say you are hopping between two windows -- one a google search about a product, the other a shopping cart checkout form. 

Experienced users know that clicking submit on a form twice is a bad and often expensive mistake. As well, clicking on submit often has little or know "confirmation" feedback other than a subtle message in the lower right corner of the window. 

So if I am on the google search page and decide to buy I might click on the submit button. However all I am really doing is activating the page. I haven't submitted my form at all. So I sit there looking stupid, afraid to click on the submit form button again. 

This is an intersection of two bad interfaces: the bad interface of the window (bad feedback to page activation) and bad interface of the submit button (bad feedback mechanic for browser submissions.)

I think two things should happen when you click a submit button:

1. The form should "Lock up" preventing accidental double submission
2. The page should go away since you are now in a "dead space" between an HTTP submission and the response. (at the very least -- **FORMS** **BUTTONS** and **LINKS** should grey out or something.)

I believe the second case because whatever other things you might start doing may at any moment be pre-empted nastily -- you might even trigger another ajax action or some other activity that creates an interrupted transaction scenario. 

## In Summary

Whenever you do something you should have immediate feedback as to your current state. Even a second or two of delay creates doubt and anxiety as to whether the system has received your input signal, or its broken. It forces the user to decide  whether or not to "hammer the button" til it works or wait for further feedback. The user should never be put in this position. And in fact code allowance should be made to insulate systems against "hammered input"

Part of receiving feedback is to make changes of state absolutely graphic and unambiguous; when you submit a form, activate a window, click on a link, your feedback should be immediate, large and loud. 

When you enter into usage states -- like moving the mouse for a while or putting your hand into an interaction box for Leap Motion Controllers -- you should have a large and loud graphic feedback as to where your cursor is in the interaction space. Autocad programs used to have large noisy x and y axes that covered the entire panel. This is extreme but is also quite informative as compared to a tiny rat turd of a cursor that can get lost on a large and/or involved panel/window/screen. 

## Appendix: No Country for Old Code

Handling the "submitted page event" in the Javascript context is not well explored.

Every time a page submits, a guillotine chops off any pending processes. And actually, it chops off not when the user submits a form or clicks on a link -- it chops off when the response comes back. As I mentioned before, in the special case of forms, the form should be neutralized when the user does anything which initializes a HTTP request -- whether that is clicking on a link, submitting that form, or submitting another form;  allowing activity in this nebulous window between request and response means that you can't guarantee that scripts that might respond to clicks or forms have time to execute. Also, some activities and clicks change the page and provide confirmation messages to the user that their action was completed (or failed) and those messages may never be seen if the responding page loads in quickly. 

So coders are left in the dark for two reasons: there is no universal event broadcast to stripts to let them know that a request has been initiated and they need to complete any pending activity, and there is no safe code bubble in which javascript can "Clean up" any pending activities. 

All of this is true not only because of the HTTP request/response event -- there is no safe code bubble to respond to a window being closed either. 

I think pages should have a limited "Phantom zone" of time to exist in order for the page to clean up any states shared between the page and server -- say, if the user is in a chat context, to send a message out that the user has left the chat room, or to tell the server that the user has left the page so it can clean up any other state logging records in the database that are no longer needed. 

In this phantom zone, the dom should be "read only", but still there to be polled etc.