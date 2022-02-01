# C++ Questions

1.[Polymorphism](#poly)

### <a name="poly">Polymorphism
  
  * Explain polymorphism in C++?
    - it describes the concept that you can access objects of different types through the same interface
  * What is the difference between static and dynamic polymorphism?
    - Static binding/Compile-Time binding/Early binding/Method overloading.(in same class)
    - Dynamic binding/Run-Time binding/Late binding/Method overriding.(in different classes)
  * What is function overriding? How it works?
    - redefinition of the base class function in its derived class with the same signature i.e. return type and parameters.
    - Function has to be defined as virtual in base class
    - override keyword(not strict) Tells the compiler: "make sure there is an EXACT virtual function I am overriding"
