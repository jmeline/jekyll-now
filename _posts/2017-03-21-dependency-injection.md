---
tile: Dependency Injection
---

Part of the SOLID principles, D stands for Dependency Injection. It's purpose is to decouple code by implementing the Dependency Inversion Principle. It's a design pattern that helps a class separate the logic of creating dependent objects. The goal is a loosely coupled system.

According to Martin Fowler, Dependency Injection is to have a "separate object, an assembler, that populates a field in a class" that determines the implementation of an interface.

Dependency Injection Principles (DIP):
  * High-level modules should **not** depend on low-level modules. Both should depend on abstractions. Another way of saying this is a dependency should never be made upon a concretion; only upon an abstraction.

Three types of Dependency Injection

 * Type 1 IoC (interface injection)
   - Drawback is that it requires a lot of interfaces to get your application functioning
 * Type 2 IoC (setter injection)

 * Type 3 IoC (constructor injection)
   - Preferred because it makes testing easier
 A service locator is another way to break dependencies. Is an object that knows how to collect and implement all the services that maybe needed in an application. Though it is highly discouraged because it hides dependencies and convolutes code.

When to use setter or constructor injection.
  A General issue with OOP is: Should you fill fields in a constructor or with setters? Marten Fowler says: Create valid objects at construction time. It is based on Kent Beck's Smalltalk best practices.
  Constructor initialization allows you to hide fields that are immutable by simply not providing a setter.

  Constructors suffer from parameters. If you have a lot of strings as parameters, you're relying on position to understand intent. Setter injection can clarify each name to indicate what the string is supposed to do. This is sometimes easier to read.


Problems with IoC
  * Harder to understand and leads to problems during debugging
