+++
date = "2017-06-04T12:00:15+02:00"
title = "Contract testing with GraphQL"
draft = false

+++

Contract tests (or [Consumer-driven contract testing](https://www.martinfowler.com/articles/consumerDrivenContracts.html) ) 
"are an essential part of a mature microservice testing portfolio, enabling independent service deployments" 
(https://www.thoughtworks.com/de/radar/techniques/consumer-driven-contract-testing)

The basic idea is that a Service X (the Consumer) using Service Y (the Provider) can only be deployed if Service Y upholds the contract
between X and Y (the same is true for the Provider: it can only be deployed if it is still compatible with all Consumers). 
This means before X (or Y) is taken live it is ensured that the Provider still satisfies the demands of the Consumer. 

The important aspect here is that Consumer and Provider can evolve independently as long as the contract is still valid: it is
a very pragmatic approach because it only focus on what actually really matters at runtime.


[GraphQL](http://graphql.org) is "a query language for your API" which was originally created to improve the data exchange between a 
mobile client app and the server. But as a generic solution it is in noway limited to that and can be used to communicate between two
microservices in the backend.  

Because GraphQL is statically typed it simplifies contract testing: the Provider exposes the Schema via Introspection Query
and this can be used to check if the Consumer requests are valid. This check is done without actually calling the Provider (except for the Introspection query).


## graphql-contract-checker

To demonstrate this approach I created the project [graphql-contract-checker](https://github.com/andimarek/graphql-contract-checker).

It is basically one method which takes three inputs: the query the Consumer wants to check, the "old" Introspection result 
(from a time where the Contract was valid) and the current Introspection result (the schema currently in usage).

It then returns a boolean to indicate if the Provider still satisfies the expectations of the Consumer.

A full working example is [graphql-contract-checker-example](https://github.com/andimarek/graphql-contract-checker-example).
It emulates a Consumer microservice which checks via Gradle task if the contract is valid. Details can be found in the README.


Because the Provider is never called the check is very fast and it can easily be used to check dozens of different queries.
Also because every GraphQL query specifies exactly what should be returned it is assured that the Consumer receives exactly what it needs. 


## Limits

It is only checked if the Provider satisfies the request: if the request is valid and if the types haven't changed. But because the 
Provider is never actually called there is still room things that can go wrong. Depending on how much test coverage you want this means
you want addtional integration tests. 


## Feedback

You can reach out to me via twitter [@andimarek](https://github.com/andimarek/graphql-contract-checker-example) if you have any kind of 
feedback.
