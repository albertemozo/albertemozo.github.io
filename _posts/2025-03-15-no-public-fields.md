---
layout: post
title: No Public Fields
tags: oop software-design 
---

Exposing an object's fields publicly is a practice that I prefer to avoid at all costs. I'll try to explain why.

In a nutshell, public fields...

- Break [encapsulation](https://en.wikipedia.org/wiki/Encapsulation_(computer_programming)).
- Violate the [information hiding](https://en.wikipedia.org/wiki/Information_hiding) principle.
- Reduce [cohesion](https://en.wikipedia.org/wiki/Cohesion_(computer_science)) and increase [coupling](https://en.wikipedia.org/wiki/Coupling_(computer_programming)).
- Go against the [Tell, Don't Ask](https://martinfowler.com/bliki/TellDontAsk.html) principle.
- Foster [inappropriate intimacy](https://refactoring.guru/smells/inappropriate-intimacy).

## Encapsulation

Encapsulation is a property of objects that allows us to isolate their internal behavior from others. Thanks to this property, we can use objects as black boxes that accept messages and return responses without worrying about what happens inside.

Furthermore, when one object interacts with others, encapsulation allows us to modify its internal implementation without affecting its clients.

Encapsulation also helps putting together the things that change together, following the principle of high cohesion and low coupling.

Public fields undermine encapsulation by exposing internal details and opening the door to third parties messing with the data and behavior of objects.


## Information Hiding

The information hiding principle tells us to keep the implementation details hidden behind well-defined interfaces.

Speaking about objects, this involves making use of encapsulation as a general rule, but this principle can be applied to other kinds of modules or even whole applications or systems.

Public fields are an obvious violation of this principle, even if not the only possible one.

## Cohesion and Coupling

The principle of high cohesion and low coupling has been around since the birth of OOP, and it is one of the most important ones we should follow to keep the cost of changing our software under control.

High cohesion means that the elements within a module (an object in our case) must be closely related and work together to achieve a single, well-defined purpose. This helps clarifying the role of the object, and makes it easier to test and extend. The popular Single Responsibility Principle (the S in SOLID) is just a rephrasing of this one.

Low coupling means that modules (objects) must have minimal dependencies on each other. They interact through well-defined interfaces, so changes in one module have little to no impact on others. This reduces the risk of ripple effects when code is modified, and simplifies maintaining behaviors that are modeled in autonomous modules.

Public fields allow implementing logic related to the data belonging to an object inside a different object, reducing the cohesion of the latter. It also allows third parties depending on the internals of the object, increasing coupling.

## Tell, Don't Ask

*Tell, Don't Ask* is another principle of Object Oriented Programming that tells us to communicate with objects by telling them what to do, instead of asking the object for data and then working with it. This leads to keeping data and behavior together within the boundaries of the object.

Ultimately, applying this principle can have a positive effect on cohesion and coupling.

Public fields expose the internal object data, opening the door to asking instead of telling. But it's worth noting that even getters would go against this principle, so you may not want to follow *Tell, Don't Ask* in every situation.

## Inappropriate Intimacy

The violation of the previous principles, and especially the breakage of encapsulation by accessing the object's internals, is considered by many a [code smell](https://en.wikipedia.org/wiki/Code_smell).

A code smell is a pattern that can indicate the existence of an underlying problem in the code. In our case, the inappropriate intimacy between objects can be a marker of the cohesion, coupling, maintainability, and reusability problems we've seen before.

## Example

Let’s consider two alternative implementations of a bank account, along with two interfaces to interact with it: an ATM and OnlineBanking. One uses public fields, while the other uses private fields.

The following example exposes a public field so any client, such as the ATM, can access and manipulate it.

```typescript
class BankAccount
{
    public balance: number;
}

class ATM
{
    private bankAccount: BankAccount;

    withdraw(amount: number): void
    {
        this.bankAccount.balance -= amount
    }
}

class OnlineBanking
{
    private bankAccount: BankAccount;
    
    withdraw(amount: number): void
    {
        this.bankAccount.balance -= amount
    }
}
```

In this other approach, the balance is private and clients are forced to interact with the account through its public methods.

```typescript
class BankAccount
{
    private balance: number;

    withdraw(amount: number): void
    {
        this.balance -= amount
    }
}

class ATM
{
    private bankAccount: BankAccount;

    withdraw(amount: number): void
    {
        this.bankAccount.withdraw(amount)
    }
}

class OnlineBanking
{
    private bankAccount: BankAccount;
    
    withdraw(amount: number): void
    {
        this.bankAccount.withdraw(amount)
    }
}
```

As you can see, the second implementation encapsulates data and behavior within the limits of the `BankAccount`, making it more cohesive and less coupled to the `ATM` and `OnlineBanking`.

Now the definition of what withdrawing means is in the `BankAccount` and any client can benefit from that behavior, instead of defining it twice. Currently it's just a simple subtraction, but in software simple things don't last too much.

Imagine we want to introduce a new feature to prevent overdraft.

```typescript
class BankAccount
{
    private balance: number;

    withdraw(amount: number): void
    {
        if (amount > this.balance)
        {
            throw new Error("Insufficient funds.")
        }

        this.balance -= amount
    }
}
```

If we didn't have a cohesive definition of the bank account, we would have to replicate this logic all over the clients.

But things can go even further, imagine we want to allow some authorized operations, and we need to differentiate the total balance from the available one.

```typescript
class BankAccount
{
    private ledgerBalance: number;
    private heldFunds: number;

    get availableBalance(): number
    {
        return this.ledgerBalance - this.heldFunds
    }

    withdraw(amount: number): void
    {
        if (amount > this.availableBalance)
        {
            throw new Error("Insufficient funds.")
        }

        this.ledgerBalance -= amount
    }
}
```

Can you tell what would you need to do to achieve this if `ATM` and `OnlineBanking` were depending directly on the removed `balance` field? Try and tell me.

## Side notes

### Data Transfer Objects

DTOs are a particular kind of object that are intended to move data around the application. They never contain behavior, so they can be considered as data structures instead of real objects.

Some people relax the design requirements for this kind of object, which makes complete sense.

However, that is not the case of objects that don't contain logic merely by coincidence, such as poorly designed entities or value objects. Those are not DTOs, those should hold actual behavior and if they don't yet, that's not an excuse to make the situation worse by leaking their internal data.

### Read-only properties

Many programming languages offer some way of marking a field as read-only, so they cannot be modified after set.

Some people will argue that it's perfectly valid to make read-only fields public, since nobody else will be able to modify them. Well, it is not.

It should be clear at this point, but the problem with public fields is not only the possibility that any other party messes with the property values, but how this exposure generates coupling and breaks cohesion.