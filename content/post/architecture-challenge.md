+++
date = "2015-06-12T14:18:47+02:00"
title = "Software architecture: Real challenge, bad term"
draft = false

+++

Software architecture is probably the strangest term in software development. 
No term is so widely used and the same time so vague defined. 
No term is so widely abused and at the same time so important to software development.

## A short history

Since around 1968 people tried to fix software development through trying a more engineering like process for development. To make it short: It was a failure.

One major influence for this "Software engineering" discipline was "Civil engineering". And why not: Both disciplines are about building or producing something. And civil engineering was sufficiently successful to be a role model. So the term architecture found its way into software development.

Over the last 40 years, the engineering approach turned out to be not a very useful model. Some parts of the industry realized it and moved on to something better: Agile development.

But the term architecture survived while all other parts of this software engineering idea are vanished or highly disputed.

## The term in daily usage

It's impossible to find a perfect definition of software architecture. Many smart people tried and I will not add a new one here. But still everybody does use it. Why is this so? It seems like the term evolved into an abstract, shared umbrella expression among developers. It describes a collection of loosely connected aspects while developing software.

I don't think you can give someone a definition and they can successfully use the word. You have to read a lot about software and architecture, you have to listen to other people and most important you have to build software on your own. And over time architecture will become something real inside your head: It's about understanding and looking at the whole. It's about dependencies. It's not about UML or line indentation. It's often about things, which don't change over years (because they don't need to or because they are too hard to change). Sometimes its about a specific technology or language, but often it's not. It's about design, but not everything design related is architecture. It's about how to organize and communicate fundamental structures and ideas. It shouldn't be about the persistence technology, but often it is. It's about understanding the overall purpose of the software and how to deal with change.

## The challenge is real, but the analogy is wrong and harmful

Everything described above is a complex problem. How do you prepare for a change without knowing how the change will look? How do you add useful abstractions without adding accidental complexity? How do you organize you source code in alignment with the organization? And so on. These are all real challenges.

But the word architecture is simply a bad choice for describing these challenges. It's bad because it implies a wrong analogy to traditional architecture. Software architecture has nothing to do with traditional architecture. It's not useful to compare them, it even turned out to be harmful and should be avoided.

Traditional architecture is about building something like a house or a bridge. It deals with physical constraints, it has clearly separated steps, it doesn't deal with any real changes after finishing. Building a house is a project while software is about building a product.

And granted: We got rid of most of the false analogies, but one is clearly visible until today and still hurts us: That architecture is something different than coding.

The analogy goes like this: The architect (or a group of architects) designs a house and then it is build by completely different people with completely different skills. The architect is better paid and more important than a single worker on the construction site. Because he/she does all the creative design and is responsible for the overall outcome, while the worker just executes something.

Mapped onto software development it means that the architect does fancy UML diagrams and produces a lot of documents but he/she is not coding. Somebody else does it. The one who codes just executes, like the worker on the construction site.

This may sound strange, but a lot of organizations tried to build software in such way and probably do it till today.

Most people realized that it doesn't work well and so they started to bring architecture and coding closer together. The joke about the architect in the ivory tower came up while the real joke is that there is even the role of an architect.

## Every developer is an architect

Architecture is also one of the most abused terms because the industry does use it as some kind of badge. Too often the title "Software architect" is a promotion from a "normal" developer. 
Especially in the enterprise world it's still common practice. Even companies, who try to be agile, have this superior rank. And I don't mind the promotion aspect or that some people need another title than Developer.

The first problem with this title is the implication you can separate and distinguish development from architecture. It also implies that architecture is more important than development. 
Both is wrong: Architecture is an aspect of development: Every line of code, every abstraction, every module contributes to the overall architecture. And you can't change the architecture without coding.

The second problem is that senior people are not automatically the best ones for making decisions regarding architecture. But when the only promotion path your organization offers for software developers is to become an architect, everybody wants to become an architect. And again you have conflated authority with architecture.

Every developer has the responsibility to care about architecture. There shouldn't be a specific role for that. Of course, some care more than others, some developers are more interested in UX or DevOps or whatever. And it might also be true that experience is an important factor for a good architecture (so is talent). But all this doesn't justify a special role.

This is similar to QA: Over time people realized that creating a specific role for quality assurance is a bad idea. 
But getting rid of QA was easier, because nobody was promoted to QA and you can't impress someone with this title. "Software architect" still sounds better than "Software developer" for most of the people. Largely because of the wrong analogy to the traditional architect: The architect gets all the fame for the new building.

If you really need a special title for developers, I prefer for example "Lead Developer" or "Senior Hipster Developer" or whatever you may think of without introducing a false analogy.

## Conclusion

I would be perfectly happy if we could find another accepted word for architecture, like "Structural design" or a fantasy word without prior meaning.. But this is wishful thinking. In reality we are stuck with this historical burden and I just hope that in the future architecture will be seen as just one aspect (albeit very important) of software and we will stop promoting people to architects.




