I am picking up some things in the Unity sphere I thought I'd share; hopefully they will save some time for those stepping in for the first time. I'm definitely not an expert here but picking things up as I go....

## Use C&#35;

As a JS advocate I am as surprised as anyone to find that C# is not only a good language -- it is a really good language. And much of the good work going on in Unity is in C# so really it is best to stay with the herd here. 

Yes there are plenty of other compilers for Unity including a bastardized version of JavaScript and even a Python interpreter; but I have had great success with C#; Given the density of Unity's codebase, the type-checking really works in your favor here. 

## Event Programming in Unity 

While there are native listeners for mouse action and such, if you want to listen for your own events, you have to use a lightly documented and circuitous method of delegates in which the event type is defined as a delegate, and a property of type event is exposed for you to add or remove listeners to. 

the documentation is [Here](http://answers.unity3d.com/questions/593012/how-to-pass-on-information-through-c-events.html); I won't repeat it in its entirity except to note a few facts that don't jump out of some of the documentation:

* You can pass an argument to a delegate callback. (perhaps even more than one - haven't tried it)
* You can't subscribe to a delegate callback outside of the class it is defined in -- however you CAN create a method which will do this for you that you then expose for remote subscription. 
* It is recommended that you subscribe and unsubscribe to delegates during the `OnEnabled` and `OnDisabled` handlers. However, These handlers won't kick in on initial instantiation so you will still have to listen in for the first time in the `Start()` handler. 

Why would you want to do this? My use case at leap is that I have a game construct of a Cursor that I have to manually code rollover and click behavior in response to the Leap Motion Controller. To do so I have every button (that is active) "Listen" for an update to the cursor position and toggle their displays to give you feedback when you "hand over" a button's target region. 

I do this by emitting the cursor position from a delegate to any number of listeners. 

There are a host of great reasons to use events in unity for other purposes -- for instance, you could emit a "OnExplosion" event, passing the strength and center of the explosion as a Struct, and resolve its effect on any game object that is "explodable". 

## Sprites and Non-sprite interaction

2D Sprites are new in Unity 4.3 and like all new features have some quirks. Unity has non-sprite things like TextMeshes and 3D objects you might want to mix in with your 2D sprites and layering these things together is not a slam dunk. 

Standard Unity objects are organized in `Layers`. Sprites sort themselves in `Sorting Layers`. (I can't for the life of me see the wisdom of a dual layering system but there it is.) 

Ordinarily the only way to assign a sorting layer to a non-sprite is by code; however Nick Gravelyn has made [a clever drop-in suite of tools](https://github.com/nickgravelyn/UnityToolbag) that let you assign sorting layers using the editor. 

Using his UnityToolbag you can assign a sorting layer (and an ordering number within said layer) to any Unity object you feel like.

## Multi-camera scenes

You may often find it easier to compose a scene using multiple cameras; for instance you may want to keep your HUD (score,titles) layer always on top of the rest of the action and managed with an ortho/2D camera while your action is in 3D. Also you might want a Sky Dome / Space Dome to always be in back of the rest of the action, rotating but not moving with the player's point of view. 

While layers can help its sometimes cleaner to have two back-to-back cameras one of which handles part of the scene, one of which handles another part. Each camera has two vital properties for multi-camera work: Depth, which is the sort order of multi-cameras, and Clear Flags, which allow you to expose one camera's work behind anothers'. 

## In Close

Unity, like a modern film (Modern being post Walt Disney, inclusive) uses layered visuals to accomplish stunning composition. There are multiple mechanics to manage this: (in order of precedence) camera depth, sprite sort layers, and (regular) layers. Learning to work with these is part of making your Unity project well managed and well behaved.