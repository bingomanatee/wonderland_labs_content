The "Framework Wars" are heating up. Having worked side by side in Backbone and Angular I have to say, it is very difficult to defend the concept that the differences between frameworks are academic or superficial. The choice of the right framework is extremely meaningful in downstream maintenance and technical debt. 

I'd like to highlight a few points of contention between Backbone and Angular specifically and the differences it has made to me in production. 

## &quot;All Angular does is get rid of a little boilerplate&quot;

Nobody denies that Angular allows you to do more with less code. However I think its worth highlighting the kinds of order of magnitude we are talking about here. We are not talking about "A little code." We are talking about a LOT of code, and some of the messiest, highest-maintenance code in the site. 

### The Model

This is one area where Angular stands out against not just Backbone, but Ember. Ember requires you to actually design formal classes for your model data to interact properly with your page; and I will note there are several situations where that is actually a good thing, the fact remains that being able to dump a JSON object into a controllers' property list and use it right away does lift a huge set of development cycles off of the frontend developers' shoulders. 

In Backbone, a common practice would be to [use a model as a controller](/blog/javascript/backbone_mvc) because there is not a formal controller component in the Backbone lexicon.&ast; 

There is another side effect of this. If (in Angular) you don't have a prototype-instanced model, then any calculation you want to do with fields is embedded in the view and accomplished with filters, which by my reckoning is a much cleaner design pattern.

### Dashboard Hell

Many Backbone applications' interaction networks are simple enough to manage that it becomes easy to fail to appreciate how much dynamic interaction is truly managed by Angular. However if you ever have to code a filtered Dashboard, you really come to value the dynamic binding of Angular and what it does to save you time and effort as a coder. 

Sometimes people don't calculate the interaction debt their code undertakes until its too late. 

Let's play "Fantasy eBay" and design a simple -- that is, not so simple -- investment dashboard. 

Our dashboard will have the following elements:

1. A stock ticker search term
1.a an autocomplete dropdown for the search term based on your accounts' stocks
2. A date range filter which is actually two independent fields:
2.a From date
2.b To date
3. A checklist of brokerage accounts
4. A graph of stock performance
5. A table of holdings
6. A breadcrumb reflecting the current filters
7. Interactivity with the URL to allow bookmarking of the current state. 

## Computing interaction

An interaction is when a change in one UI elements makes a change in the model, which in turn triggers a change in another UI element. 

The amount of cascading effects are staggering. 

State change can happen directly, because both UI elements are observing the same model, as with the breadcrumb and the URL both observing the date range. that is:
* view changes model
* other views that observe the model are updated

It can happen indirectly; for instance, the ticker filter changes the list of accounts reducing the list to those accounts that contain holdings that match the stock filter.
* view changes model
* model change triggers refiltering of other models
* other views that observe the refiltered model are updated

The worst case interactive scenario is that the number of DIRECT interactions you have is (View elements ^ 2) - view elements. That is, every view element has the potential to trigger an update in every other view element besides itself. 

This means our nine-element dashboard can have up to SEVENTY TWO interactions you have to allow for. 

In reality, I counted twenty one obvious interactions, shown on the colored lines of this diagram. 

![brokerage_dashboard.png](/blog_image/brokerage_dashboard.png)

The number of interactions is reduced significantly because two of the elements (the graph and the table) don't have interactions that change the models in ways that the other components need to observe. (though that could change with product enhancement...) Also, the autocomplete widget serves to directly update a single element, the search term. 

## How this plays out in Backbone and Angular

In Backbone, the level of complexity depends on how you implement interaction. 
* if you have a view heavy codebase, you end up broadcasting state between the views and each other directly. This is a manual process which is extremely time consuming. 
* if you have a model heavy codebase, you end up sending a lot of update messages to the model, which in turn send update messages to other views. This is probably a better way to code, but still means a fair amount of work. 

In Angular, most of these messages are **implicit* due to data binding. The process of binding a model value to a UI control will create interactivity between the other widgets that observes the same model. 

Where RPC patterns are in place, you may have to use manual `$scope.$watch` patterns to change the remote model, but this sort of pattern is still single-order complexity. 

## In Closing

No matter what framework you are using to design a UI, it is worth your time to graph and count the number of interaction patterns that exist between its components. This kind of estimate can vastly impact the number of tests a system requires, and your estimate for how long a given project will take to complete. 

It should also increase your respect for the degree to which data binding can reduce your technical debt; this is true whether Angular or its binding peers (Ember and Knockout) are used to effect this pattern.

--------
&ast; There is nothing preventing you from creating your own custom controller, as [the above article](/blog/javascript/backbone_mvc) documents; its just that between putting business logic in the view or in the model, the latter seems a more natural choice.