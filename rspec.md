# RSpec

## DevDev notes

- RSpec shared examples are a way of DRY-ing up specs. A shared example describes some behaviour that is common between specs
  - the shared examples are pulled into a context using either `include_examples` or `it_behaves_like`
  - `include_examples` simply pastes the shared examples into the current scope "blindly"
  - `it_behaves_like` will pull in the shared examples into a new context block

- `subject` defaults to an instance of the described class under test

- SPIES: you can ensure that an object has received a method, and with certain arguments.
  Some examples:
  - expect(MyObject).to have_received(:a_method).(3).times
  - expect(MyObject).to have_received(:a_method).with("an_argument")

- Check out this article by Martin Fowler:
  https://martinfowler.com/articles/mocksArentStubs.html#TheDifferenceBetweenMocksAndStubs

- Shane recommends checking out Haskell, which is a functional programming language

- "Domain Driven Design Quickly" - check out this book

- If you want to try DDD, think about it carefully and also be familiar with SOLID principles