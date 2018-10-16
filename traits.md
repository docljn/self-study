# Traits
- phpdocs
  - http://php.net/manual/en/language.oop5.traits.php
- sources
  - https://www.culttt.com/2014/06/25/php-traits/
  - https://beberlei.de/2013/04/12/traits_are_static_access.html
  - https://blog.ircmaxell.com/2011/07/are-traits-new-eval.html
  - http://thinkrelevance.com/blog/2013/04/04/abstraction-or-leverage
  - http://rosstuck.com/how-i-use-traits
  - https://www.sitepoint.com/using-traits-in-php-5-4/
  - http://www.zentut.com/php-tutorial/php-traits/

## Definition
"Traits is a mechanism for code reuse in single inheritance languages such as PHP. A Trait is intended to reduce some limitations of single inheritance by enabling a developer to reuse sets of methods freely in several independent classes living in different class hierarchies."
- equivalent to a runtime copy-paste of code

## Background
- aka mixin: https://en.wikipedia.org/wiki/Mixin
- like an interface with implemented methods
- enforces dependency inversion principle, i.e. high level modules are independent of the implementation of low-level modules
  - High-level modules should not depend on low-level modules. Both should depend on abstractions.
  - Abstractions should not depend on details. Details should depend on abstractions.
- avoids multiple-inheritance confusion
  - Abstract Class:
    - inheritance involves an "is a..." relationship
  - Interface:
    - usage means a "has a..." relationship
    - states that an object should be able to do a thing (implementation not provided) i.e. defines a method which must be implemented
  - Trait:
    - usage means a "has a..." relationship
    - gives the object the ability to do the thing i.e. defines an implemented reusable method
- classically, a trait has no state and a mixin does, but PHP implementation includes the ability to read and write the state of the class they are mixed into.
- often, a trait can be thought of as similar to static methods except for the ability to access state

## Declaring Traits
- very similar to declaring a new class
- ```
  <?php
  trait Logger {
    function log($msg) {
    echo '<pre>';
    echo date('Y-m-d h:i:s') . ':' . '(' . __CLASS__ .  ') ' . $msg . '<br/>';
    echo '</pre>';
    }
  }
  ```

## How to Use Traits in a Class
- a trait cannot be instantiated alone (like an abstract class)
- include a trait in a class with a `use` statement inside the class
  - ```php
    class HelloWorld {
      use Traitname1, Traitname2, Traitname3;
    }
    ```
  - the `use` statement respects the current namespace
- trait names must be unique in the using class
- method precedence
  - current class methods > override trait methods > override inherited methods
- if two traits have methods with the same name you can use `as` or `insteadof` to distinguish between them
  - ```
  use A, B {
      A::hello as greetings
      B::hello insteadof A;
    }
    ```
- you can change the visibility of trait methods
  - ```
      class MyClass1 {
        use HelloWorld { sayHello as protected; }
      }
    ```
- traits can be composed of other traits
- traits can include
  - implemented methods
  - abstract methods
  - static variables
  - static methods
  - properties

## When to use Traits
### Reasons for using Traits
- when the identical method is needed in various unrelated classes
  - to avoid code duplication without using inheritance
  - to avoid multiple identical implementations of an interface method
- i.e. add functionality to a class without duplicating code or creating non-sensical inheritance trees

### Advantages
- reduce code duplication
- prevent complicated class inheritance

### Disadvantages
- often lead to divergence from the Single Responsibility Principle i.e. a class should have only one reason to change, aka a class should do only one thing, and do that well
  - makes code bloat easy
  - classes can have too many responsibilities
- code can be hard to reason about
  - source code doesn't include all the methods of a class in that class
  - duplication of logic
  - horizontal and vertical inheritance
- abstraction becomes impossible
  - code is close-coupled by default as change in the trait results in a change in all the uses of that trait (aka leverage)
- method conflicts can occur
- methods cannot be overridden or exchanged as in inheritance
- implementation cannot be changed as in interfaces
- methods cannot be tested in isolation, and are not mockable
- traits cannot be type checked


### Alternatives
- composition i.e. using interfaces to enforce contracts
- value objects

### Improvement: combine a trait with an interface
- the using class can either implement their own version of the interface or use the predefined trait
- allows for a change in implementation i.e. removes the default close-coupling
- allows for eventual abstraction if needed
