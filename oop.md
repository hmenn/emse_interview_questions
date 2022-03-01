## OOP specific questions that I've met

1. Please explain SOLID Principles.

    - **S**ingle responsibility
      - A class should have only one reason to change
    - **O**pen/closed
      - Software entities should be open for extension, but closed for modification
    - **L**iskov substitution
      - If S is a subtype of T, then objects of type T in a program may be replaced with objects of type S without altering any of the desirable properties of that program (e.g. correctness)
    - **I**nterface segregation
      - No client should be forced to depend on methods it does not use
      - Don't put lots of methods in a single interface. If it's required, create new IF for each method/functionality
    - **D**ependency inversion
      - High-level modules should not depend on low-level modules. Both should depend on abstractions
      - Abstractions should not depend on details. Details (concrete implementations) should depend on abstractions.