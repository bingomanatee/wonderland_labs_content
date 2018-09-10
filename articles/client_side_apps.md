I had a discussion with a company recently who still was on the north side of client side
apps. For those who have been sold the question might seem fatuous, but for those with a 
heavy investment in the rich environment of server-heavy development patterns it is very real.

I'd like to state up front: I'm not trying to imply a backend app is not a workable 
solution. But I think the case can be made for a very strong frontend for most use cases 
for which backend-oriented applications are presently employed. 

Here are a few things I'd like to start out with:

## Backend work is expensive

Every redundant database query, served file, and calculated request is a cost to both 
the business(in money and long term, purchased servers or server resources) and the user
(in time). 

## Slow sites degrade the user experience

Exhaustive work as been done to demonstrate the correlation between a slow app
and a bad user experience. In many cases the connection itself provides the inertia, so
regardless of the server situation, a user in a poorly served internet market gets a 
slow experience the more files and transactions go over the web. https://stevesouders.com/
details these costs better than I ever could so Im taking this as a fact moving forward. 

So with that in mind lets look at ways that the frontend application provides a faster
experience with fewer file requests and faster return of information

## Transferring templates to the client

The first optimization for client side applications is that template work -- the act
of combining data and templates to viewable HTML -- happens in the browser, not the server. 
This is especially useful for saving time when the page has no real data demands; 
hits on static pages like FAQs completely obviate any request for server activity.

This is especially true of the repeated boilerplate such as navigation, headers/footers,
etc. 

It also means that when you do mutation of already loaded data such as pagination or sorting
of data passed in through a previous request take no extra action to the server. 

Similarly persistant data such as the logged-in user's metadata aren't part of any further
computations on the server. 

## Reducing the impact of data requests. 

Quite often, servers send the same data out to many, many users, requiring the investment
of calculations that always produce the same result, many times a second. While there
are ways to optimize this, nothing beats the ability to crystalize this data into a
static JSON file that can be served straight to the client, and in some cases, cached 
in the browser for a while, obviating the need to involve servers at all. 

Even in cases where the data is custom to a user or behind a permission system, caching
systems can employ a basic security check system before transmitting the data, or 
a very limited filter to ensure the user gets only what they are allowed to see. 

Also, the data custom to a particular user can be prepared for transmission on a schedule,
reducing load spikes on the server. 

The net effect is not only fewer hits on the server, but the ability to maximize the 
user's request-response time by putting the files in a CDN closest to their place on the network
and a much more controllable data preparation experience, especially for shared, repeatable
or non-rapidly changing data. 

## combining multiple resources to a single transmission 

One of the magics of WebPack (and other similar systems) is that the contents of the 
sites JavaScript, CSS, Templates and templating Images are all bundled into a single file. 
That is, the logo, background images, bullets, etc. are all included in a single burst
transmission. That alone ensures that there are fewer HTTP transactions; fewer transactions
means fewer demands on the server and fewer chances that an odd image takes an extraordinarily
long time to make the leap creating weird "flashes". 

## The Testing Equation

Client side applications allow you to create a division between testing the user experience
and testing the backend APIs. User Experience testing can be executed against snapshot
data and is much faster than end to end tests. The fact that the tests are faster
allows for more testing and more scenarios, providing a stronger coverage and more confidence
on your ability to refactor and revise the user experience and the code that drives it.

The tests on the server side are also more focused, tying directly to data bursts,
API validation and data modelling. Once the transitive property has gained confidence
inside the company (that is, trust in the fact that if you prove that the backend
delivers what the API requires and that the frontend interacts properly with the API,
you have _de facto_ end to end coverage) your overall procedures are better energized. 

## Speed of development, diagnosis, and experimentation

Client side applications are faster to mutate and evolve. When, for instance, you need to
get client side code in shape for a different API, you can create quick post filters in
client side code to mutate the incoming data, or mocks with the new API, and develop 
tangible artifacts in front of the server side evolution; this means that the visual and
frontend developers aren't depending on server side code being developed before they 
update the site in anticipation of it. 

Also, having split front and backend processes help focus the effort of finding issue
root causes, because the front and backend can be easily changed in isolation, making it
easier to find the source of bugs. 

## The SEO question

The one area that seems to be the last argument for server-side investment is SEO data;
that is, the fact that the raw HTML bundled for the benefit of the search engines 
that a server side app provides is composed of the information search engines need.

The concept of an Isomorphic app is the answer to that. Isomorphic apps let you pre-bake
javascript templating in the server for a given request before sending the HTML file
to the browsers. That is, the same data-templating composite that the browser provides 
(and is executed by javascript in concert with data) can be done on the server,
providing a complete templated content on page load suitable for search engines. 
http://isomorphic.net/ provides some insight into the underlying technologies and
systems behind this but it is a fully answered question for best-practice client side apps.

## Conversion Costs

The greatest resistance to client side apps come from those who assume the conversion costs
are prohibitive. They are not, and they are controllable.

### Most of what you need you already have

The design and apis you need to make a site frontend-friendly are mostly already within your codebase. 
Interpreting them into a frontend paradigm is a very straightforward exercise.

### Conversion can take place at whatever pace is required

Frontend services don't need to own the entire experience. They can be applied as needed, where wanted.
Leaving some processes -- especially internal dashboards and other low-value pages -- as is doesn't 
hamper your ability to work in a more frontend-centric to your application. 

You can embed frontend systems within backend-served pages as you wish, to get a feel for what the 
client side application delivers. However, many of the benefits of performance and reduced backend 
traffic won't be felt if your investment in client side technology is too patchy or insincere.

### "But this adds a whole layer of technical debt to my site!"

Well, yes. But past a point when templating is no longer part of the server side equation,
it also removes a significant layer of complexity and technology to the debt you already have. 
Further, by pushing the user experience into the client, it allows for debugging and testing
wholly within the browser, which provides a much more immediate, testable and introspectible
experience than server site heavy applications, which removes a whole host of inertia to the development process.