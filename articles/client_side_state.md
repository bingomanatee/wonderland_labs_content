Establishing client side state through url routing is a fairly important chore. [This SFJS video ](http://youtu.be/uOj-AwcVbjA) is one of many great analyses in this arena. However, creating a singular bookmark for your app state is not as straightforward as it might seem.

## The Simple Solution

Assuming that your page has a single simple focus -- as in, navigating an online contact list -- its pretty simple to bookmark your page by the identity of that resource, as in `http://www.mysite.com/contacts#22` when viewing a record for user ID 22. This is true whether it is a single page app or you do a page reload for each client. 

## Things start to get interesting

However most pages are more dynamic than that. Say, for instance, your contact app has three tabs -- "history", "address", "notes", and that is a state that you want to bookmark. Now your URL gets longer: `http://www.mysite.com/contacts/#22/notes`. 

This is all well and good; you can even use querystring data to allow for arbitrary appendation of state for any number of widgets as in `http://www.mysite.com/contacts/#/user/22/tabs/notes` as described in [this blog article](http://tomdale.net/2012/05/ember-routing/). basically a key value flattened list. 

## Its all about context

Then, you have to consider, who "owns" the job of the URL rewrite. If for instance your components are individually responsible for reading and updating the URL, you have to consider that components can get reused and embedded and don't necessarily know their context. For instance, the tab system you use for your contacts might be used in a view for tickets. So, you might have a page with a list of all your contacts in a particular city, each user having their own tabset. 

If you put the tab-widget in each contact's ticket, then changing one contact's state will change the tabs property of the url, and when copied to another page, will display the same tab state for each user, because they will be reading off the same global URL property. 

The quick fix for this is to allow the parent context that uses the tab widget to set which URL variable each tab uses -- so for a three contact view, the result might be `http://www.mysite.com/contacts/#state/CA/tabs22/notes/tabs33/address/tabs16/history`. However this only pushes the problem back one level, as that context may itself be nested, etc. 

The fact is, in a dynamic page, state is likely hierarchical, and individual state only relevant when in the context of its place in the hierarchy. A url is not a great place for hierarchical structures; generally you are better off allowing a single or at lest, relatively flat and limited state snapshot, centrally managed, than to try and distribute the role of URL updating or covering all client side states in the URL, unless creating some sort of fantastic deep-pathed url scheme is a desirable activity.