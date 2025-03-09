---
layout: post
title: Generic Subdomains
tags: ddd
---

Generic subdomain is a concept from Domain-Driven Design I find especially useful for the modern software engineer.

DDD splits the domain — the problem space of the business — into specific areas called subdomains. The boundaries between these subdomains are often aligned with the boundaries between models, understanding models as structured abstractions of real-world concepts. DDD asserts that different parts of the domain are represented by different models, which co-exist but may be partially or entirely incompatible. As a result, they require separate implementations and solutions, ideally encapsulated within their respective bounded contexts.

But not all subdomains are equal.

- We can categorise subdomains according to different criteria, but three main types are usually identified.
- Core subdomains are the parts of the domain over which we can build our competitive advantage.
- Supporting subdomains are the ones that not being directly a source of competitive advantage, are needed for the business function and specific to it.

Generic subdomains, finally, are the ones common to many businesses, for which there are standardized solutions.

This specific division is useful because it enables us to prescribe a rule of thumb on how to deal with each type of subdomain:

- Invest the most effort on developing for your core subdomain(s). Optimize for continuous improvement. The future of the business depends on this.
- Don't worry too much about supporting subdomains, just make things work, you won't build your success upon these.
- Don't get distracted by generic subdomains, use off-the-shelf solutions and avoid reinventing the wheel.

I've seen too many teams building homemade authentication platforms, deployment toolkits, and CRMs — classic examples of generic subdomains where leveraging existing solutions would save time and effort. I think a bit of DDD won't hurt from time to time.