# C++ Questions

0. [Topicless](#notopic)
1. [Polymorphism](#poly)
2. [Smart Pointers](#smartptr)
3. [Exception Handling](#exceptions)
4. [Lambda Expressions](#lambda)
5. [Enumerations](#enums)


### <a name="notopic">Topicless

  1. What is the difference between an argument and a parameter?
      ```c++
      void foo(iny my_param){}
      ...
      ...
      int my_arg =3;
      foo(my_arg);
      ```

  2. What is the iterator invalidation?
      - It's like pointer invalidations which means that your iterator points and invalid location.
      - When you made insert/delete operations on an contaier while iterating, your iterator might be invalidated.
      - Sample:
          ```c++
          // Creating a sample vector
          vector<int> v = { 1, 5, 10, 15, 20 };

          // Changing vector while iterating over it
          // (This causes iterator invalidation)
          for (auto it = v.begin(); it != v.end(); it++)
              if ((*it) == 5)
                  v.push_back(-1);

          for (auto it = v.begin(); it != v.end(); it++)
              cout << (*it) << " ";
          // 1 5 10 15 20 -1 -1
          ```

### <a name="poly">Polymorphism

  1. Explain polymorphism in C++?
      - it describes the concept that you can access objects of different types through the same interface
  2. What is the difference between static and dynamic polymorphism?
      - Static binding/Compile-Time binding/Early binding/Method overloading.(in same class)
      - Dynamic binding/Run-Time binding/Late binding/Method overriding.(in different classes)
  3. What is function overriding? How it works?
      - redefinition of the base class function in its derived class with the same signature i.e. return type and parameters.
      - Function has to be defined as virtual in base class
      - override keyword(not strict) Tells the compiler: "make sure there is an EXACT virtual function I am overriding"
      - sample:
        ```c++
        class A{
          virtual void foo();
          virtual void zoo() const;
        }

        class B: public A{
          virtual void foo() override;
          virtual void zoo() override; // Compiler error. Zoo was const
        }
        ```
  4. What is the 'final' keyword?
      - It forbids next level inheritance when used in class signature.

        ```c++
          class B: final A{};  // No one can derive class B
          class C: public B{}; // forbidden
        ```

     - It forbids function override in next level
        ```c++
          class A{
            virtual void foo() final;
          }

          class B: public A{
            virtual void foo() override; // Compiler error. foo func. is guarded
          }
        ```
  5. How much is the size of empty class?
      ```c++
      class A{};
      sizeof(A); // ?
      ```
      - It's not zero. Each class has a internal/secret unique identifier that used for unique address identification and equality checking. For example these classes has no variable however they are not equal and minumum 1 byte is allocated on memory to store them.
        ```c++
        class A{};
        class B{};
        A a; // sizeof(A) is 1
        B b; // sizeof(B) is 1
        a == b; // it's false
        ```
      - If we define a variable, we lose unique variable
        ```c++
        class C{
          uint8_t c;
        }; // sizeof(C) is still 1 byte. Unique identifier discarded because of internal member

        class D{
          uint16_t d;
        }; // sizeof(d) is 2 bytes
        ```
  6. What is abstract class and how we use them in C++?
      - If you're familiar with java/c++ you know interfaces. They are same.
      - Abstract class is a class type that you can't instansiate it. In order to make a class abstract, you need at least 1 pure virtual function. Which means zeroed function.
        ```c++
        class Abs{
          virtual void foo() = 0;
        };
        Abs abs; // compiler error
        ```
      - You can derive abstract functions. If you don't override pure virtual function, derived class is also abstract.
      - Concrete class = instansiatable class

### <a name="smartptr">Smart Pointers
  1. What is the RAII(Resource Acquisition Is Initialization)?
      - *Resource Acquitision*: Open file, allocate memory, acquire a lock
      - *Is Initialization*: resource acquired in constructer
      - Resource relinquishing: Close file, deallocate memory, release a lockAcquired in the destructor.
      - For example smartpointers help for resource lifecycle.
        ```c++
        {
          #include <memory>
          ...
          std::unique_ptr<int> b = std::unique_ptr<int>(new int{3});// c++11
          std::unique_ptr<int> a = std::make_unique<int>(3); // c++14
          std::shared_ptr<int> c = std::make_shared<int>(4); // c++11
          std::cout<<*a<<std::endl;
          *b = 2;
          *c = 11;
          // no delete required when scope end
        }
        ```

### <a name="exceptions">Exception Handling

1. TBA

### <a name="lambda">Lambda Expressions

1. What is the functor?
    - Functor is a class/struct/object that you implement `operator()`, which means it's callable object.
    - Sample:
      ```c++
      struct ACall{
        int operator(){
          return 3;
        }

        int operator(int x){
          return x+3;
        }
      };

      ACall a;
      a();
      a(5);
      ```
    - You can pass functors to STL functions. They might have members and that's their difference from functions.

2. What is the lambda?
    - It's a different/easy syntax to create functors.
    - Sample:
      ```c++
      //[](){};
      //|  | \--> Function body
      //|  \----> Function arguments
      //\-------> Capture area

      [](){ std::cout<<"I'm a lambda"};
      [](int x){std::cout<<"I'm also a lambda and I take argument:"<<x};
      [](){ return "I can return anything"};


      int a;
      string b;
      [a, &b](){ b="Hello"; a=3; return "I can capture variable from outside and use it"; }; // stateful lamda
      ```

    - Stateles Lambda: Don't capture anything
    - Stateful: Captures data from outside

3. How lambda captures values?
    - Captures variables by value or by referencens.
      ```c++
      int x{100};
      /*
      [x](){
        x+=100; // Compile error. It's read-only since handled by value. Not mutable.
      }();
      */

      [x]() mutable{
        x+=200; // It only changes value of local X.
        std::cout<<x; // print 200
      };
      std::cout<<x; // print 100


      int y=20;
      [x, &y](){}; // capture x by value, y by reference
      ```
    - Default captures:
      ```c++
      [=]     // Default capture by value
      [&]     // Default capture by reference
      [this]  // Default capture this object by reference

      [=, &x]     // Default capture by value but capture x by reference
      [&, y]      // Default capture by reference but capture y by value
      [this, z]   // Default capture this object by reference but capture z by value
      ```


### <a name="enums">Enumerations

1. Describe unscoped and scoped enums?
    - Unscoped
      ```c++
      /*
      enum-key enum-name: enumerator-type {};
         |
         \----> Enum Key, defines the scope of the enumeration
      */

      // Unscoped
      enum State {OK=1, NOK=2};

      int x=1; // or std::underlying_type_t<State> x =  std::underlying_type_t<State>(OK);
      if(x == OK){/**/} //Unscoped

      ```
    - Scoped
      ```c++
      enum class State2 {OK, NOK};
      // can not compare state2 with integer without casting
      if(x == static_cast<int>(State2::OK)){}
      // You need to compare enum class with enum class and enum-name as scope identifier

      State2 s1=State2::OK;
      State2 s2=State2::NOK;

      if(s1 == s2){}
      ```
