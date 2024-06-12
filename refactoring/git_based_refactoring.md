# Ashley Ellis Pierce: Git Driven Refactoring

<https://brightonruby.com/2018/git-driven-refactoring-ashley-ellis-pierce/>

- every design problem has a refactoring technique
  - sandy metz:
    - get a whiff of this talk
    - <https://www.youtube.com/watch?v=PJjHfa5yxlU>

- how to find the design problem so you know how to refactor it?
- you need context to recognise design patterns
- git provides context to help you see where the patterns are being violated, and hence places to refactor

## Working Example: SOLID

- if you follow the principles you should have code which is easy to understand and change
- git can help you spot where the principles are being violated

### Single Responsibility Principle: only one reason to change

- why has this class changed recently?
  - `git log filename`
  - can you group the commit titles in any way?
  - these are probably the 'reasons for change' that should be their own classes
- means you need to have good commit messages!

### Open/Closed Principle: open for extension, closed for modification

- you shouldn't have to change existing code to add new code
  - if a diff adding a feature is full of red AND green, you're in violation of the Open/Closed Principle
- consistent style is essential

### Liskov Substitution Principle: child should be able to substitute for parent

- github hooks
  - write a stylebot to check for principle violations
  - let your bots do the nagging

### Interface Segregation Principle: no client should be forced to depend on methods it doesn't use

- does your class assume that a parameter will be a particular type of thing?
- only methods in the parameter's class should need to know what kind of thing it is
  - ```git log --grep "ClassName"```
  - lots and lots of new methods added to a class to try and make it imitate the interface expected are clue

### Dependency Inversion Principle: depend on abstractions not completions

- does your class create ```new Object()``` inside itself?
  - use dependency Injection to fix
  - another stylebot to the rescue here
