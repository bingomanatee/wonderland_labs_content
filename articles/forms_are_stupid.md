I have been using forms for over 15 years and I hate them nearly as much as the first day I laid eyes on them. There is just so much wrong there its almost impossible to describe. 

Almost. 

## The form tag should be structural

Why force people to create layouts for their forms? Just create a structural construct in the form tag definition and lay out all form elements automatically. (I'm using concepts from the below stuff here too.)

```` html

<form layout="vertical"
 title="Join our site as a member"
 style="form-head: 5em; width: 100%; form-divider: 1px solid grey" 
submit="Register" 
action="/member/register" 
cancel="Cancel" onCancel="hideForm()">

<formemail name="username" label="Email" placeholder="Your Primary Email">Email</email>
<formpassword name="userpassword" label="Password" min=8></password>

</form>

````

should give me:

![form1.png](/blog_image/form1.png)

## Labels should be attributes

Every input element needs a label; why make it a separate markup element? Just add an attribute: ` label="Email Address" ` to the input.

## The primary submit and cancel buttons should be implicit to the form 

Why keep knocking out markdown for the elements of the form that are mandatory in nearly every use case? Tag them into the form

## Each input type should be its own tag

Why keep overloading input? I'm sure whoever thought up the input tag had somewhere to get to. 

Textarea for some reason escaped the pen they put everyone else into. Free the rest of the tags. I'd like to see

* formtext
* formemail
* formpassword
* formchoice
* formrange
* formbutton
* formcaptcha

and the others I'm not remembering off the top of my head as first class tags. 

## Bring captcha in as a formal form definition

Why keep forcing people to slap captcha boilerplate and third party vendors to humanize the process of form submission? Make the browser do the lifting. 

## Make radio and checkbox elements full buttons

The idea of a 16 pixel target area is absurd. Make the button and label a formal singular element as in

```` html

<formchoice style="buttons" label="Shirt Size" layout="vertical" cols="2" prefix="size-" choicemode="single">
<option value="1">Small</option>
<option value="2">Medium</option>
<option value="3">Large</option>
<option value="4">Extra Large</option>
</formchoice>

````

![form2.png](/blog_image/form2.png)

### Eliminate the false duality between lists, checkboxes, and radio buttons 

All three controls are basically the same thing: a control that lets you choose between options. In the case of checkboxes, the choice is yes and no. Radio buttons are the choice of one among many items, as are lists. Checkbox groups give you the choice between many items and you can choose more than one-- as you can in the (badly wrought) multichoice lists we have now. 

The need for several divergent markups for what is the same basic system baffles me. Just give us a formchoice tag with options that we can style as a dropdown, vertical or horizontal button list and be done with it. 

As it is designers have to create separate artifacts for the item and its label with annotative metadata; why?

# Do we need to make every form element look alike? Why can't I invent my own UX?

Suck it up. There are lots of places for art majors to shine. Forms need to be clear, uniform and standard. You can choose your palette and font but leave the rest of the form system to the pros. 

There are only about three real choices for form layout: inline blocks, vertical lists, and stacked. If you want to invent your own bad ux, leave off the label tag and the layout property from the form and go to town. 

## Add a menu tag system in HMTL

Stop making us reinvent the wheel; give us formal menu definitions in HTML:

```` html
<menu name="site navigation">
<item url="/home">Home</item>
<item url="/about">About</item>...
</menu>
````

Angular has been doing this with directives and there's a host of JS components for this but this is so fundamental to web design it really should be a formal part of the markup, not a DIY project. 

Its appearance should mimic the built in UX of the windowing system the browser resides in. (or the mobile UX.) 

The great advantage form elements have to date is that even in Mobile, they mutate to meet the appearance and interface of the device naturally, by default. This should be true of menuing as well.