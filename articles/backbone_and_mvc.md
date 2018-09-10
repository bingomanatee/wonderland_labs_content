I had an interesting discussion about backbone and application logic. I'd been working at a company where the logic for control flow was all embedded into the **view**. I had a discussion with someone during a code review in which the supposition was that the controlling code should be in the **model**. This wasn't an option at PersonalCapital, where the model was in many ways external to the Backbone environment. 

I have come to two  personal opinions that I have come to about data models:

1. Models should largely be as stupid as possible. External methods should be responsible for adding business intelligence to model data, not the model classes themselves. 
2. Data is quite often better modelled as update events rather than static collections.  View implementations can retain whatever fraction of the emitted data they need to maintain state. 

so in my general pattern of application design I don't generally put application logic into models. 

## Views as controllers

Using a View as a controller might seem like odd MVC, 

## Controllers as controllers

But in reality, the classic MVC pattern doesn't put logic in either place -- it traditionally belongs in the controller. Backbone notoriously lacks a controller, leaving it up to you to define one; its not that hard -- a controller can be a generic object/custom class that has reference to models and views and manages events. 

So in retrospect, if I were to work with Backbone in an ideal scenario, I'd generally prefer to work that way.