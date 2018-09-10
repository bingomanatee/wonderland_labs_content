I have decided after years of attempting to generalize repeated patterns that there is a kind of quantum vortex around some tasks wherein every attempt you make to make a general solution creates insane specific friction around all your use cases. 

One common task case is form elements. I have written "form element handlers" over and over and finally come to the conclusion that outside of larger systems like Angular/JQUI, it is better to create form elements manually and use jQuery/dom operations on them yourself than to try and create a pre-fabricated solution and apply it to all use cases.

There is an intersection of some common patterns that should make you question trying to generalize a problem set:

1. **The basic problem is not that hard.** Creating form element / markup, with/without labels, is not that tough.
2. **The customization possibilities are wildly varied.** There are tons of ways to label form elements; many (such as tables with labels on top) do not conveniently put the label markup directly before/after the input markup. Many times you want to blend multiple form elements into a "meta solution" as with a combobox, City/State/Zip fields, date widgets, et.al. And thats not even getting into the fact that many form elements are very browser-bound when it comes to basic UI. 
3. **The input and output combinations are wildly varied.** Form element input data can be all sorts of stuff - ajax, DB, model, etc. And the result of interaction is likewise pretty wildly varied -- especially with file inputs, checkbox groups, et. all. 
4. **The ratio between the assumed commonalities and varieties of instances is huge.** For instance, you can generalize a dropdown. However, looking at all the things that drop down, you basically can include any possible widget/web content known to man. (At some point, you will be able to click on an entry in a Google search result and the website will drop down.)
5. **You can easily think of design specifications that will stymie your basic assumptions** -- see more about this with the Grid example below. 

## Grids -- we don't all know each other. 

When you think about grids, the visual characteristics they have in common jump out at you. Titles, row/column UI, possible pagination, search, sorting. This makes for a fairly tempting target for generalization. However, when you start doing a grid in real life, things like switch-out edit forms, row selection, dynamic data updating, custom cell formatting and zero state use cases are what define your experience. 

I have never seen a grid component that is easier to use -- especially, easier to inherit -- than a templating loop with custom code. Angular itself makes that an extremely easy ask given that you can pipe `repeat` to `sortBy`, `order`, `search` and hey presto, you get your filtered data. But this is true in basically any framework -- if you have a function that pumps out records and listeners for headline clicks, you have all the foundation you need for a grid. So even if you have a pretty powerful grid component, it is taking the place of a very low profile manual implementation. And it is very likely the markup and CSS it generates are not going to be as easy to style as something you come up with, if you know the UI specs beforehand. 

Consider a very basic design fork: say your client wants checkboxes for rows, but only if the "edit multi" button is clicked. The functionality for doing this, and making your cells scale proportionally when the checkbox is clicked, is pretty significant. This is not the sort of functionality you would want to be available on all instances of a grid. 

Many basic features of tables -- especially true HTML tables -- like colspanning are pretty easy to implement on a specific use case but take a whole lot of horsepower and branching logic to generalize. 

Other common requirements -- categorization headers, especially multi-depth ones -- are a real headache to generalize, but almost no effort at all to implement on a task by task basis. 

So, in the end, you have a scenario where the possibility to save time is slim, and the possibilities of creating a huge, overworked component -- worse yet, one that cannot be applied to a specific specification -- are significant.