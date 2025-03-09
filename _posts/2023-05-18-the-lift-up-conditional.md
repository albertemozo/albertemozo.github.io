---
layout: post
title: The Lift-Up Conditional
tags: refactoring
---

The *Lift-Up Conditional* is a refactoring technique useful in those situations when a conditional logic consisting of several cases is mixed in the shape of a mess of nested conditions.

I came across it recently thanks to the local Coding Dojo organized by [CraftersVigo](https://twitter.com/CraftersVigo), and I must confess I was left with my mouth open.

This technique is better shown than explained, but I'll try to summarize the process and then include a couple of videos demonstrating it.

1. First, you need **full test coverage** of your code and a way to visualize it in your IDE.
2. Second, you need to **isolate the messy code block** in a separate function or method.
3. Third, you make a **hypothesis about a condition** that could be isolated from the mess.
4. Fourth, you **create a conditional** (the *Lift-Up* one) with that exact condition and you copy the whole messy code in both the `if` and the `else` branches, using a temporal extracted method if you want.
5. Fifth, you **run your tests again with coverage**, the dead code will be marked in red. You delete all the dead code and clean up empty or never-changing conditionals.
6. Sixth, this is when you get **astonished** with the result.

As promised here you have a couple of videos demonstrating the technique:

* The [Gilded Rose kata solved by Emily Bache](https://youtu.be/OJmg9aMxPDI) using this technique (this is the second of a three-part screencast).
* The same kata [solved by Gregor Riegler](https://youtu.be/0bhfWtZocF8), also using the *Lift-Up Conditional* refactoring.
