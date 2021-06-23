# Nothing is Something
  -- Sandi Metz
https://www.youtube.com/watch?v=OMPfEXIlTVE

## Part 1: Nulls break code, and Conditionals are Evil
### Problem: Fatal Error
- e.g.
  ```php
  class MyClass
  {
    $ids = ['cow', 'tree', 'pig']
    $animals = []
    foreach ($ids as $id) {
      $animals[] = Animal::find($id);
    }
    //result: $animals = [Cow, null, Pig];

    foreach ($animals as $animal) {
      echo $animal->getName();
    }

    //result:
    // Cow
    // PHP Fatal error: Call to a member function getName() on a non-object
  }
  ```

### Solutions
#### Ignore it
- if you can afford to ignore the nulls, if they really don't matter
- use array_map or similar to remove any null returned
- continue with what is left

#### instanceof checks
- e.g.
  ```php
  class MyClass
  {
    $ids = ['cow', 'tree', 'pig']
    $animals = []
    foreach ($ids as $id) {
      $animals[] = Animal::find($id);
    }
    //result: $animals = [Cow, null, Pig];

    foreach ($animals as $animal) {
      if !($animal instanceof Animal) {
        echo 'no animal found'      
      } else {
        echo $animal->getName();
      }
    }

    //result:
    // Cow
    // no animal found
    // Pig
  }
  ```

- this means your code has two options depending on the type of object returned
  - option 1: supply default behaviour
  - option 2: send a message
- this really isn't Object Oriented Programming
- behaviour shouldn't be set by the querying code, but by the object being queried
- OOP is meant to be all about sending messages to objects

#### better solution: Null Object
- define a null class with a descriptive name which will respond to the same methods as the original class with useful defaults
  -> The Null Object Pattern (Active Nothing)
- when a method could return null, return a new NullObject instead
- your code can then send the same message and expect a response
- e.g.
  ```php
  class MyClass
  {
    $ids = ['cow', 'tree', 'pig']
    $animals = []
    foreach ($ids as $id) {
      $animals[] = Animal::find($id) || new MissingAnimal();
    }
    //result: $animals = [Cow, MissingAnimal, Pig];

    foreach ($animals as $animal) {
      echo $animal->getName();
    }

    //result:
    // Cow
    // no animal found
    // Pig
  }
  ```
- we no longer need to duplicate behaviour everywhere we could return null
- we still have a problem, in that we explicitly have to remember to return an instance of the NullObjectClass (we've added a dependency)
- any class using methods on Animal class also needs to know about MissingAnimal class

#### optimal solution:
- wrap the untrustworthy external API (Animal.find()) in your own API which is guaranteed not to return null
- e.g.
  ```php
  class GuaranteedAnimal
  {
    public static function find($id)
    {
      return Animal::find($id) || new MissingAnimal();
    }
  }

  class MyClass
  {
    $ids = ['cow', 'tree', 'pig']
    $animals = []
    foreach ($ids as $id) {
      $animals[] = GuaranteedAnimal::find($id)
    }
    //result: $animals = [Cow, MissingAnimal, Pig];

    foreach ($animals as $animal) {
      echo $animal->getName();
    }
  }

  ```
- you no longer need to change what you do in your code depending on whether there is the possibility of null being returned rather than an object.

## Part 2: The House That Jack Built (Inheritance vs Composition)
- inheritance is for specialisation NOT for sharing code
- naming can be really misleading
- inheritance means A _is a_ B if A inherits from B
- composition means A _uses_ B which _can do_ C

Composition is often the hidden pattern when you get stuck with a situation that looks like it needs multiple inheritance
- we can use dependency injection to achieve composition
- reveal how things are different by making them more alike
- work out what you are changing and give it a name
- once you've done that, you have the possibility of a Class which implements the single change you need
- that will then carry out the required role in the classes you were trying to subclass in the first place
