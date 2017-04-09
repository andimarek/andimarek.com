+++
date = "2015-05-21T14:32:00+02:00"
title = "Flux Architecture distilled (and a bit about AngularJS)"
draft = false

+++
After watching the Flux [Intro Video](https://facebook.github.io/flux/docs/overview.html), reading about Flux and additionally reading some blog posts about AngularJS and Flux (e.g. [#1](http://victorsavkin.com/post/99998937651/building-angular-apps-using-flux-architecture) and [#2](http://christianalfoni.github.io/javascript/2014/09/26/using-flux-with-angular.html)) I had the feeling that the fundemantal ideas behind Flux are not very well presented and as a consequence not very well understood. (See also this [infoq article](http://www.infoq.com/news/2014/05/facebook-mvc-flux).)

So I will try to grap the essence of Flux (as I understand it):
(I will focus on the Facebook Flux implementation https://github.com/facebook/flux)

(This is not a introduction into Flux, React or Angular)

## The Problem Flux solves: Model (or Component) dependencies

When you watch the Intro Video closely, you will see that Flux is motivated with an example of an app:

![](http://cdn.infoq.com/statics_s2_20150519-0054u2/resource/news/2014/05/facebook-mvc-flux/en/resources/flux-react-mvc.png)

The Model upates the View and View changes are reflected in the Model. This is could be a typical Angular app with two-way binding.

The problem here is that you have a complicated dependency graph, perhaps even with cyles. And as a consequence the changes in the View can trigger other changes in other Views and so on. The same goes for changes in the Model. Ok ... the real problem are not the dependencies itself (propaply most or all of them are needed): The real problem is that they are totally implicit and not really introduced with the bigger picture in mind. The result is a unwanted and not very well understand behaviour.

Flux solves this problem of implicit dependencies with the  [Dispatcher](https://facebook.github.io/flux/docs/dispatcher.html#content) and the `waitFor` method. This one of the two most important aspects of Flux! Actions and Stores are just abstractions needed to implement the Dispatcher.
Actions represent a state modification and a Store encapsulates a State (it's the Model).

The other important aspect is: The Dispatcher doesn't allow Actions triggering other Actions. When you call `dispatch` while handling an Action, the Dispatcher will throw an Error. This enforces a simpler app and a better understanding of your Actions. The consequences of an Action are: Some Store changes and propaply some changes in the View. But that's it: No cascading Actions (Action `A` will trigger Action `B`, will trigger Action `C` ...)
And this is essential: When the goal is to clean up the complicated Model/View dependencies you can't just introduce something called "Actions" and that's it. You have to add some real value. In this case: Actions are not allowed to trigger other Actions.


The Dispatcher is the central element to coordinate the Model (Store) changes. To achieve this, there must be only one Dispatcher. This single Dispatcher is the gate keeper: It handles AND coordinates all upcoming changes. This is different to a Store: There are plenty of different Stores in every real life app. 

And that's why this picture is not very good:

![](http://cdn.infoq.com/statics_s2_20150519-0054u2/resource/news/2014/05/facebook-mvc-flux/en/resources/flux-react.png)

There should be more than one Store and even more Views.

Getting back to the `waitFor` method: To coordinate an upcoming change and to clean up the implicit dependency mess you call now `waitFor` and make sure that every information a Store `A` from a Store `B` needs is available. 

This is a much more better than before: You are stating in a declarative way that Store `A` depends on Store `B`. You don't rely on any other hack or implicit flow in some other parts of your app. You are certain that you can access Store `B`.
Also: Because the Dispatcher knows the complete picture you can't have any circular dependency. `waitFor` will throw an Error and you are forced to resolve the cycle. 
This brings the whole dependency topic into your source code instead only in the heads of the developers or on some white-board.


In the [Dispatcher documentation](https://facebook.github.io/flux/docs/dispatcher.html#content) there is an example where the Store dealing with a city selection needs to make sure that the Store containing the selected Country is up-to-date:

```
CityStore.dispatchToken = flightDispatcher.register(function(payload) {
  if (payload.actionType === 'country-update') {
    // `CountryStore.country` may not be updated.
    flightDispatcher.waitFor([CountryStore.dispatchToken]);
    // `CountryStore.country` is now guaranteed to be updated.

    // Select the default city for the new country
    CityStore.city = getDefaultCityForCountry(CountryStore.country);
  }
});
```

## Dispatcher summarized

* The Dispatcher is the single entry point for handling Actions
* Dependencies between Stores (while handling Actions) are handled with the `waitFor` method
* Circular Store dependencies are forbidden and will result in an Error
* Actions triggering other Actions are not allowed: A `dispatch` call while handling an Action will result in an Error

## What is with the unidirectional data flow?

Yes, it's true: There is only one way the Store/Model is updated: Via Actions. But it's just a one-way databinding: The view changes are not immediately reflected in the app Model. Of course the result of a user interaction is often a Model change, but it's coordinated and executed with the help of the Dispatcher. 

The so called "unidirectional data flow" is just a consequence of the Dispatcher: When you need to coordinate all changes to the Model you will always end up with a gate keeper and only one way to change the model. So I think this naming is a little misleading and confuses more than it helps.

## How does React fits into this?
React is a very good extension to the Flux Idea, but solves a different problem: When you have changed your Model and the View needs to be updated, how do you apply the DOM changes? React uses a virtual dom to apply the changes in a efficient way. This allows the developer not to think anymore about the diff between now and later, but just apply the current State to generate the View. 

When you think about the "unidirectional data flow" from Flux again: When you want to emphasize this kind of functional thinking, that you just have a function with input and output,  than React fits very well with Flux. Because now you can really think of the flow of your app as "unidirectional". And that's propably the reason why Flux is so strongly associated with this wording.

## How to use AngularJS with Flux
Instead of React you can use any technology to update the View with the new Store values and to generate new Actions. The main idea of Flux is the Dispatcher, which is very abstract and has nothing todo with any View technology.

But when you use Angular you will propaply break the functional unidirectional idea at one point sooner or later. Of course you can just reset a Directive after every Store change, but because Angular doesn't have a virtual dom you will have to deal with performance problems.  

That doesn't mean you shouldn't use Flux with Angular. I just wanted to make clear what Angular has to do with Flux.


## What if I don't need or don't want a Dispatcher?
Of course you can just implement a Store and decouple the View from the Model. You can even discard the Actions and just call the Store directly. And depending on the app and the use case it might be fine. 
But nearly every real life app while have more than one Store (and several Actions) and will have dependencies between the Stores. And then it gets interesting: At least now you should get a Dispatcher and Actions and call `waitFor` for the dependencies and you should make sure that Actions will not trigger other Actions. 

If you don't do it, it's also fine, but then you just implemented something else and I would argue it has nothing to do with Flux. 
 



## This is just my Point of view
The official facebook flux repo contains only one real piece of code: The Dispatcher. No Views, no Stores ... just the Dispatcher. After reading this text I hope it's clear why: It's the essence of Flux. The rest is just a concept how to use the Dispatcher. 

So my Point of view is: The Dispatcher is the core idea and its the part which contains a concrete solution for a problem. And of course facebook always talks about Flux in the context of React. And granted: It's a very good fit with React, but you don't really need React to use the Dispatcher.

And as long as you stick with the core idea of a Dispatcher you can implement Flux differently or better. See [here](https://reactjsnews.com/the-state-of-flux/) for alternative implementations. But I would not call it Flux if you just have a Store and Actions or just a Store.

I would appreciate any feedback via  [@andimarek](https://twitter.com/andimarek) or just leave a comment.

**Update 22.05.2015:** Added "Dispatcher summarized" and infos about how the Dispatcher prevents cascading Actions.

