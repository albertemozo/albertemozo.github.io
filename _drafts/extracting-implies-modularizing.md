---
layout: post
title: Extracting → Modularizing
tags: microservices monolith-decomposition
---

This cute arrow → is a logic symbol called [material implication or material conditional](https://en.wikipedia.org/wiki/Material_conditional). It can be read as *"implies"*, so if the left side of the statement is true, the right side must be necessarily true as well.

Some time ago my team was discussing the benefits of two different approaches to dealing with a certain part of the system that needed major improvements but was so legacy that the perspective of messing with it was scary. The functionality was scattered around a monolithic application, with no boundaries at all, sharing resources, communicating, and depending on an unknown number of other components.

One side stood up for refactoring the legacy implementation until adding new features was safe and somehow comfortable; while the other side argued that working in such a mess would be a waste of time, so it would be better to reimplement everything outside the legacy solution.

After some time, one of my colleagues said something that left us shocked.

— *How come do you expect extracting to be easier when it also implies modularizing?*

Single eyebrows raised all over the place, but the explanation made their partners follow them to draw surprised expressions.

You could reimplement a feature as a standalone microservice, but how will you manage to get all the clients of the old implementation to use this new one? The process of tracking the usages of the old code and making its clients point to the new microservice is just the same as it would be to point them to a facade or whatever other module interface. The difference is that, while IDEs give you plenty of support for refactoring logic inside the same code base, things get much more complex when you have to deal with external interfaces through HTTP calls, messaging, or other network-based communication.

So in the end, microservices are nothing else but independently deployable modules that communicate over networks. In that sense, extracting functionality to a microservice always **implies** modularizing plus other tasks; by definition, it can never be less work.

Does this mean that the only possible approach to monolith decomposition is refactoring until the target module is ready to cut & paste into a new microservice? Nope. This is only true for situations where the amount of crossed dependencies is the big issue. There are other situations, for instance, when a feature is not connected to many other components but it has a big internal complexity. In that kind of scenario, a simpler reimplementation with only a bit of plumbing work to be done can be the better approach.