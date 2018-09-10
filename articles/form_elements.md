Form elements have been with the browser since the beginning. With HTML5 they are even more semantic with URL / email validation (which is much more tough than you may think), select groups, required fields and other things. 

And yet people continue to screw with them. 

Partly its because the browsers have made restyling Form elements very difficult. I would REALLY LIKE to be able to size and style my checkboxes and radio buttons but no, I am fucked. (For that matter, I wish they were contained, so that I could put the label inside the input: 

``` html

<input type="checkbox" name="subscribe">Clicking on this text should check/uncheck the radio button</input>

```

And styling grouped lists is pretty awful too. 

That being said -- the temptation to resist the urge to replace HTML form elements with DOM/JS based widgets is a bad one for many many reasons. Like these:

## Form elements are parseable on all platforms 

Mobile, tablets, even lynx readers; you are guaranteed that your entire audience has full access to your entry mechanism. Most mobile devices bend over backwards to make form elements easily manipulated, despite the small screen

## By hiding form elements you lose the ability to leverage the HTML5 validation mechanics

Tags like `required`, `min`, `max`, and the filters for `url` and other specialized entry types will fail silently -- well nearly silently, as they will throw console errors. But they **WILL** block your users' attempt to submit their form. This is definitely worst case scenario. 

## You are adding technical debt to one of the most mission critical areas of a site

Every widget you make for a form element is more burden to the engineers, and has a nonzero chance of going sideways. ANd when it does you lose user input. User input is in todays interactive world the most valuable part of the web experience -- for them and certainly for you. Because of this, you want the most robust tools available taking in and processing that input. 

Using form elements, and most especially the built-in validations inherent in HTML5 compliant browsers, ensures you are relying on proven, shared code to retrieve feedback from your users. 

Even a javascript adverse user, surrounded by jars of their own urine and TV Guides from the 70's, can use a form. And trust me, you want to hear from him -- those TV Guides are vintage now.