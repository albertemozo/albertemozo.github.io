---
layout: post
title: The Pocket Guide to Test Doubles
tags: testing test-doubles
---

Test doubles are objects used to replace a dependency of the unit under test that we prefer not to execute, often because it involves external services.

There are several types of test doubles, each with specific characteristics. The choice depends on the unit under test.

**Dummy**: Does nothing; its methods return empty results or fail if invoked.

```python
class DummyProductCatalog(ProductCatalog):
    def add(self, product: Product) -> None:
        pass

    def all(self) -> List[Product]:
        raise NotImplementedError("I'm too dummy to be invoked.")
```

**Stub**: Returns a known response, either hardcoded or configurable, which can be a valid response or an error.

```python
class SingleProductCatalog(ProductCatalog):
    def add(self, product: Product) -> None:
        pass

    def all(self) -> List[Product]:
        return [Product("some_id", "Some Product", 1.0)]
```

**Fake**: Implements alternative logic to the original, usually simpler and without external dependencies.

```python
class InMemoryProductCatalog(ProductCatalog):
    def __init__(self):
        self._products: List[Product] = []  # Made private

    def add(self, product: Product) -> None:
        self._products.append(product)

    def all(self) -> List[Product]:
        return self._products
```

**Spy**: Allows us to verify whether it has been used and how.

```python
class CountableProductCatalog(ProductCatalog):
    def __init__(self):
        self._count: int = 0  # Already private

    def add(self, product: Product) -> None:
        self._count += 1

    def all(self) -> List[Product]:
        raise NotImplementedError("I don't need this.")
    
    def product_count(self) -> int:
        return self._count
```

**Mock**: Defines how it should be invoked and fails if expectations are not met.

```python
mock_catalog = MagicMock(spec=ProductCatalog)
mock_catalog.add.assert_called_once()
```

## Which type of double should you choose?

![Test Doubles Flowchart](/assets/img/test-doubles-flowchart-light.svg){: width="30" .light .left }
![Test Doubles Flowchart](/assets/img/test-doubles-flowchart-dark.svg){: width="390" .dark .left }

A given unit may require multiple test doubles at the same time to function, and the criteria for choosing each may vary. It is important to distinguish between those that affect the behavior we are validating and those that do not.

As a general rule, it is recommended to use the simplest and most limited double that allows us to execute the unit under test without errors. In that sense, the dummy is the perfect double, although it is rarely useful because if it is invoked, it will not work. For doubles that affect the behavior we are testing, we must also consider how they help us perform our assertions.

If we are testing a query that retrieves information, at some point, we will need a double that provides the required information. A stub or a fake will be the most suitable. Between the two types, the stub is the simplest, so in general, it will be our choice.

If we are testing a command that alters the system's state, we need to ensure that the dependencies executing the action have been invoked, even if they do not return data. A spy or a mock allows us to achieve this. The spy is the simplest of both, so it will be our first option.

### The problem with spies and mocks

Spies and mocks share the ability to check if they have been invoked and how—some via query methods and others by defining expectations. To fulfill their function, the test double must be invoked in a predetermined way, meaning that using them couples our test to the specific implementation of the unit under test. This means that if we change the structure of the unit's code, even if we do not change its behavior, our doubles may no longer respond correctly, and the test may break.

This is known as a **fragile test**.

Fragile tests are harmful because they discourage refactoring and continuous improvement, which are essential practices for maintaining a healthy codebase.

### What happens if we don't use the right double?

It is not possible to use a double with fewer capabilities than we need because the test would fail. But it can (and often does) happen that we use an overqualified double, with more functionality than necessary.

For example, we could use a stub instead of a simple dummy to replace a collaborator that is never invoked. This unnecessarily increases the complexity of our test suite.

It can also happen that we not only use an excessively complex double but also abuse its capabilities.

For instance, we might use a mock and define expectations on its methods to replace a dependency from which we only need a specific return value. This not only increases complexity but also makes the test unnecessarily fragile.

## Good practices

**Less is more**. The less your tests know about the domain logic, the less you configure them, the fewer expectations you put on them, the better.

**Write your own doubles** instead of using mocking libraries. Libraries encourage the use of overqualified doubles and excessive mock usage, which make our tests more fragile.

**Don't double what you don't own**. Doubling third-party libraries will usually force you to use mocking libraries and will couple your tests to concrete vendors and library versions. Instead, hide those dependencies behind an abstraction you control and double that abstraction.

**Avoid reusing doubles**. We're used to avoiding duplicated code, but tests are different. We want them to be simple and decoupled from each other.

## Credits

This article is heavily inspired by [La guía definitiva de los dobles de test](https://franiglesias.github.io/test-doubles-guide-1/) and its [second part](https://franiglesias.github.io/test-doubles-guide-2/), recently published by my friend Fran Iglesias. I simply wanted to have an accessible resource at hand to share with my colleagues on the many, many occasions when I am forced to suggest an alternative to the infamous mock. But if you really want to understand the topic, don't waste more time here and go read the original, even if translated by your browser.
