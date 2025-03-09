---
layout: post
title: The Kali Ma Decomposition Anti-pattern
tags: microservices monolith-decomposition anti-patterns
---

![Kali Ma](/assets/images/kali-ma.gif){:style="display:block; margin-left:auto; margin-right:auto"}

Do you remember the scene from Indiana Jones and the Temple of Doom? The evil Mola Ram sticks his hand inside a poor guy's chest and pulls out his heart while he elevates a prayer to Kali Ma. For some reason, the heart keeps beating and the guy survives such a trauma.

Pretty disgusting, to be honest. But you know what? Many people in the software industry seem to like the idea, so they try the same approach in their own system when it comes to dealing with monolithic applications that need to be decomposed into smaller pieces.

This sophisticated technique consists in selecting a core part of the system, buried deep in the monolith, connected to a thousand other components by direct invocations, shared data models, and who knows what more; and trying to extract it directly to a shiny new microservice.

How? Just reimplement the features you think are the most important in a brand new repository, turn around, and show the bloody beating thing to the audience. Bravo! 👏

The discerning reader could ask: but how will you replace the old solution with the new one? How can the existing dependencies be fulfilled by the extracted service?

Well, that's the point of this whole sarcastic nonsense. That won't happen. Unless you have Mola Ram in the development team, nobody would be able to reconnect the veins and arteries to the external heart.

Perhaps I'll write something about how to face this problem properly at some point.
