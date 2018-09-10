I had an interesting discussion about backbone and application logic. I'd been working at a company where the logic for control flow was all embedded into the **view**. I had a discussion with someone during a code review in which the supposition was that the controlling code should be in the **model**. This wasn't an option at PersonalCapital, where the model was in many ways external to the Backbone environment. 

## Models as Controllers

I have come to two  personal opinions that I have come to about data models:

1. Models should largely be as stupid as possible. External methods should be responsible for adding business intelligence to model data, not the model classes themselves. 
2. Data is quite often better modelled as update events rather than static collections.  View implementations can retain whatever fraction of the emitted data they need to maintain state. 

so in my general pattern of application design I don't generally put application logic into models. 

Backbone kind of follows the [MVVM](http://en.wikipedia.org/wiki/Model_View_ViewModel) pattern by default -- though it is not completely fair to say that Backbone *has* an application pattern. That pattern does advocate driving the view through Model centric intelligence.

## Views as controllers

Using a View as a controller might seem like odd architecture, but in reality, most of the complexity of a frontend application is in view-based interaction. In part this is due to the fact that there is rarely a clean separation of concerns between the mechanics of view animation/rendering and the manipulation of underlying business elements. 

Creating and reacting to multiple nested views is kind of the "Backbone way" and this kind of supports the premise that heavy code in the view is a viable way to construct an application. 

## Controllers as controllers

But in reality, the classic MVC pattern doesn't put logic in either place -- it traditionally belongs in the controller. Backbone notoriously lacks a controller, leaving it up to you to define one; its not that hard -- a controller can be a generic object/custom class that has reference to models and views and manages events. 

So in retrospect, if I were to work with Backbone in an ideal scenario, I'd generally prefer to work that way.

This kind of supports my general opinion on Backbone, that it is more of a utility library like jQuery than a full framework. It is this lack of aggressive structure that Backbone puts forth as an asset, and in many scenarios, it may be. 

## Complexity implications

Assuming that you create view components for various elements on a page that map directly to model elements, there are ?(significant implications)[how_the_other_half_lives] to where you put your business logic and how you balkanize your models.

* If you share models between your views, interaction becomes significantly less elaborate. All you have to do is send a change to the model, and it broadcasts change to all the views that observe that model. 
* If you use private/local models for each view component, then every view has to broadcast change manually to each component that observes the model being changed. This is also true if you use a remote data source as the system of record. 

## the RPC pattern in HTML5 client development

One alternative pattern to direct client-client messaging is client observation messaging. This allows for a range of components to use a remote service to manage interaction. This is detailed in a video taken at Personal Capital. 

<iframe width="560" height="315" src="//www.youtube.com/embed/4rY0e95LgF4" frameborder="0" allowfullscreen></iframe>

In short the patterns of interaction is: 

1. A view component changes a model.
2. The change is sent to the remote server via a web service call.
3. The change is made and business logic (validation etc.) applied. 
4. A pub-sub change notice is made to all pages observing the model(s) that have updates. 
5. All views which are observing the model in question refresh their view, either through incremental update of their local cache of the model or through re-polling their models from the remote web service. 

There are a few advantages to this method:

1. You only have to code logic for business constraints in one place, the remote server
2. You can enforce consistency of behavior between the web and mobile versions of your service
3. You don't have to manually allow for the implications of a change between all the elements on the page. 

One of the potential downfalls is that you may be missing the opportunity to save on HTTP requests by keeping all the interaction modelling on the client side. 

Also, if you have a scenario where you want to model a potential change, then have the user choose whether or not to "save their changes", you are obligating yourself to BOTH observe server changes AND model local interaction (and interleave the messaging -- modelling transactional locking on the client side). 

# Angular by way of contrast

Angular is an interesting counterpoint in architecture. In Angular, the "Model" is expressed as fields in the controller, which directly echo into the view through binding. Model/backend interaction is handled through discrete HTTP/request libraries whose input is directly used to update controller fields, triggering changes in the view through binding.  

Where Backbone has highly developed View classes and formal Model instances, Angular doesn't have a "View" class *as such*; view logic is embedded in the template or extracted directly from the controller.