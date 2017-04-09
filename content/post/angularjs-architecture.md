+++
date = "2014-10-18T10:51:54+02:00"
title = "Three rules for a good AngularJS architecture"
draft = false

+++
*tl;dr* Use Directives and apply the [SRP](http://en.wikipedia.org/wiki/Single_responsibility_principle).

For the last 1,5 years I've been involved in developing two AngularJS applications. I was a member of a team of six, and over the time we developed different best practices. 

The following three rules I think are the most important and the most general. 

__1. Rule: Use Directives to structure your app.__

[Directives](https://docs.angularjs.org/guide/directive) are probably the most complex feature in Angular, but also the most important in regards to your architecture. Always create a directive to encapsulate some specific responsibility (in the best case only one). 

A good indication that it's time for a new directive is your markup: When your HTML is too large (perhaps more than 100 lines) create a new directive which contains only a subset of the original HTML.

To make it clear: Creating a new Directive doesn't always imply reuse. I would even claim that it's not the main feature of Directives: It's structuring your app through meaningful components, encapsulating a single domain specific (and not technical) responsibility.

**2. Rule: Your Controllers should only deal with $scope specific issues.**

This might be obvious but it's very important and very powerful. 
Only take care of things in your Controller you really must do in your Controller: $scope specific tasks like receiving a click event for example. 

When your Controller is doing too much non UI stuff, refactor it out in a service. (And of course when your Service is too big, make two Services out of it.)


**3. Rule: When your tests are too big or too complicated: Apply Rule 1 and 2.**

Our tests have always been a very good indicator of how clean our design is. Nearly always when the tests for a Controller/Service/Directive have been too hard to understand or too big, getting over the code and applying Rule 1 and Rule 2 resulted in a much better design and much better tests. 

We even observed a metric: The Lines of Code (LOC) of the test vs LOC of the production code should not be more than 4:1. When it's higher, something is wrong. I'm very sceptical about such metrics, so please don't take it too seriously. It's just something we observed and it's not a real rule.  

**Conclusion**

Of course this three rules are not enough. Other tricky areas exist where we didn't come up with good general rules. E.g. should you use a custom event to make an action explicit or just use a watch and react to changes (implicit).

Also these rules may seem simple, yet they proved to be very powerful and always worth considering.

Any Feedback is appreciated: Leave a comment or contact me via Twitter: [@andimarek](https://www.twitter.com/andimarek).