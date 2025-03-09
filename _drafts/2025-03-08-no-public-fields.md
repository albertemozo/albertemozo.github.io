---
layout: post
title: No Public Fields
tags: oop software-design 
excerpt_separator: <!--more-->
---

It is not uncommon to find code that exposes an object's fields publicly. This is a practice we often associate with junior engineers, but it also has supporters among more experienced ones. After all, there should be a reason why most object-oriented programming languages allow this practice.

<!--more-->

For some of us, however, this practice is absolutely undesirable. The reality is that sometimes this restriction is so deeply ingrained that when we have to explain the reasons behind it, it’s not always easy.

## Encapsulation

[Encapsulation](https://en.wikipedia.org/wiki/Encapsulation_(computer_programming)) is a property of objects that allows us to isolate their internal behavior from others. Thanks to this property, we can use objects as black boxes that accept messages and return responses without worrying about what happens inside. Furthermore, when one object interacts with others, encapsulation allows us to modify its internal implementation without affecting its clients. Encapsulation also helps putting together the things that change together, following the principle of [high cohesion](https://en.wikipedia.org/wiki/Cohesion_(computer_science)).

Following the principle of [information hiding](https://en.wikipedia.org/wiki/Information_hiding), ultimately helps us make our code easier to maintain. Publicly exposing an object's properties breaks this encapsulation and prevents us from benefiting from it.

Let’s look at some examples.

Imagine an object representing a bank account, and let's consider two alternative implementations, with private and public fields respectively.

In this implementation, which respects the principle of information hiding, it has a private field to store the balance and public methods to operate on it.

```typescript
class EncapsulatedAccount
{
    private balance: number;

    constructor(initialBalance: number)
    {
        this.balance = initialBalance;
    }

    deposit(amount: number)
    {
        this.balance += amount;
    }

    withdraw(amount: number)
    {
        this.balance -= amount;
    }
}
```

```typescript
const account = new EncapsulatedAccount(100);
account.deposit(50);
account.withdraw(25);
```

On the other hand, this alternative implementation publicly exposes the balance so any client can manipulate it.

```typescript
class ExposedAccount
{
    public balance: number;

    constructor(initialBalance: number)
    {
        this.balance = initialBalance;
    }
}
```

```typescript
const account = new ExposedAccount(100);
account.balance += 50;
account.balance -= 25;
```

At this point, we can already spot a difference between the two approaches. While `EncapsulatedAccount` contains all the definitions of the possible behaviors of the object, the `ExposedAccount` defines nothing. The meaning of "depositing" and "withdrawing" is implicit in the client implementation. This leads to low cohesion: behavior and data are in different places.

But what happens if we want to introduce a new rule to the domain? For example, disallowing overdrafts.

In the `EncapsulatedAccount`, this is trivial — we only need to modify the `withdraw` method.

```typescript
withdraw(amount: number)
{
    if (amount > this.balance)
    {
        throw new Error("Insufficient funds");
    }

    this.balance -= amount;
}
```

However, in the implementation with a public field, it is impossible to introduce this change directly, forcing us to visit all the class’s clients and introduce the same logic... or refactor the class to encapsulate data and behavior.

In this simple example, it might just be a small inconvenience, but in a real-world case, the number of dependencies and their complexity could be significant. The result is a higher cost of change — in other words, lower maintainability.

## Tell, Don’t Ask

Maintainability

Variants and special cases

DTOs & POJOs

The readonly public in PHP.