# Making our application RESTful

Here is a quote from Roy Fielding, the author of REST:

*I am getting frustrated by the number of people calling any HTTP-based interface a REST API. Today’s example is the SocialSite REST API. That is RPC. It screams RPC. There is so much coupling on display that it should be given an X rating.

What needs to be done to make the REST architectural style clear on the notion that hypertext is a constraint? In other words, if the engine of application state (and hence the API) is not being driven by hypertext, then it cannot be RESTful and cannot be a REST API. Period. Is there some broken manual somewhere that needs to be fixed?*

— Roy Fielding
https://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven

## So what does that mean?

Essentially, the responses we return from our services need to contain information about what the client can do next. For example, if the first call to our service is to get all Books, then we need to provide information about what can be done next with those books. Maybe one can be deleted but another can't, or one can be updated but another is read only. This information will be sent in the form of hypertext in the json response(s).

This principle is called HATEOAS (Hypermedia as the Engine of Application State) and is what we will be implementing in the following sections.

The example given on the [HATEOAS Wikipedia page](https://en.wikipedia.org/wiki/HATEOAS) gives a nice example of this principle in action.

## Adding in HATEOAS to our application
