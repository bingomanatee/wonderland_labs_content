In working with a backend app at Sony I found a host of UI things that didn't feel "right". Given that design/rightness is subjective I thought I'd list the principles I believe build confidence in a user and are a good foundation for stronger UI. 

## List data should not be overwhelmed with controls

In general I prefer lists to link to manipulation pages / modals rather than having them be overly congested with controls themselves. For one thing, "list vertigo" often makes people click on action buttons on the wrong row (one row up or down from their actual target).

By contrast if your click views a detail item, the target of your activity is more prominent, reducing inadvertent manipulation. This can be accomplished through:

* a modal view/edit window
* a separate page
* A split screen detail area

On that subject it is rarely a good idea to have "thick" rows -- keep row content on a single line to minimize scrolling. 

## Lists should have a sensible sort order

For most data sorting descending by date is the most sensible order; but this depends on context -- sometime its by name, other times, by creator, etc. In no case should data be presented in random order, or sorted by a field that is not displayed. (though, in many cases, reverse created sort is obvious and natural enough to break this rule.)

## Every action should have clear consequences, both before and after asynchronous resolution

When you click on something, the screen should change in a visible manner. (more visible than a corner-of-the-page alert for instance. At the least, this should obviate/diminish the chance of a double click. 

Rather than waiting for a response from the server, the buttons and screen should change; for instance, if you submit a form, the fields should be disabled (or rendered as text) and some title should appear somewhere saying "submitting data to server" or something less generic. 

A more formal implementation of this is the "review screen" -- i.e., once you hit the save button you should get a confirm screen; I am not in favor of a whole lot of "are you sure?" confirm screens; if the action doesn't cost the user time and there is a clear path to revising/deleting the results of an activity I say let them send with a single click.

But anything is better than submitting the form then leaving it on screen to edit (and potentially re-submit?) while one submission is out. Better to close the form and presume things went well than to leave them in an ambiguous state after submitting, waiting for the server to confirm your action. 

## The user shouldn't be able to submit improperly formatted data

Rather than send information to the server that fails the most basic validation, the client side app should have proper filtering and coaching to allow the user to fix these kind of problems before the submit. The submit button should be disabled while obvious client side problems exist, and the issues and steps to correct should be on the screen.

## Pagination artifacts are clumsy and should be replaced with scrollbars and in-context loading

Since the Dom/HTML has a built in navigation device for lists -- the scrollbar -- use row virtualization over pagination wherever possible. 

## Icons without text are bad and lead to confusion as to their ultimate effect.

Icons should always have prominent text declaring their purpose. I don't think that icons have ever been so clear and unambiguous that they can stand on their own, made worse by the fact that app designers often have a very local and context specific use of icons that aren't what their audience is necessarily comfortable with. 

## The user should easily be able to find records and data they created.