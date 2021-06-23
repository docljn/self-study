# Comparing Things: Equality in Ruby

## Types of Comparison

There are four equality methods in ruby: `equal?`, `==`, `eql?`, and `===`

Reminder: Everything in Ruby is an object, including numbers, variables and methods.

As operators like `==` are methods they can be overridden.

### Object Identity

The method `equal?` returns true only if the objects being compared are the same object in memory, i.e. they have the same object_id

This method should *never* be overridden

```ruby
object1 = Object.new
object2 = object1

object2.equal?(object1) # => true
object2.object_id == object1.object_id # => true
object2 == object1 # => true
object2.eql?(object1) # => true
```

As defined on the `Object` class `==` and `eql?` both have the same functionality as `.equal?` i.e. object identity, but are commonly overridden to provide a comparison based on *object equivalence*

### Object Equivalence / Value Equality

In most cases, standard Ruby classes define `==` to be true if every property of object2 is the same as the equivalent property in object1 - this is **object equivalence**

Generally, `eql?` is defined as an alias of `==`, but not always

There is no check for object identity in any of these comparisons

sidenote: Struct defines `==` to compare via attributes - which makes sense when you think of a struct as a value object

#### eql? is an alias of ==

two strings are equal if they have the same content and length

```ruby
"cat".equal? "cat" # => false
"cat" == "cat" # => true
"cat".eql? "cat" # => true
```

in arrays the order of elements is also taken into account

```ruby
[1, 2, 3] == [1, 2, 3] # => true
[1, 2, 3] == [1, 3, 2] # => false
```

in hashes content is compared, but not order

```ruby
{one: 1, two: 2, three: 3}.eql?({two: 2, one: 1, three: 3}) # => true
```

#### eql? is different to ==

numeric types compare using type conversion with `==` but not with `eql?`

```ruby
1 == 1.0 # => true
1.eql? 1.0 # => false
```



It is expected that you will override `==` and/or `eql?` in order to allow for meaningful comparisons between instances of a class you define.

#### Custom Class Example

```ruby
class Account
  .
  .
  .
  def ==(other_account)
    matching_sort_code?(other_account) && matching_account_number?(other_account)
  end
end
```

### Case Equality aka Threequals `===`

On the `Object` class, `===` calls `==` by default, but in many other classes it has been overridden to allow for meaningful case statements:

`Y === X` translates to "Does X belong in the set/box/group Y?"

the receiver of the `===` operator is the expression in the `when` clause, not in the case clause.

As a class method, it’ll normally be equivalent to asking the receiver, ‘is this argument an instance of yourself?’. As an instance method, it’ll normally be equivalent to ==, value equality.

#### Standard Ruby Examples

```ruby
Range === (1..10) # => true
String === 'foo' # => true
'foo' === String # => false
(1..5) === 3 # => true
/java/ === 'javascript' # => true
```

#### Custom Class Example

```ruby
```

## Comparing objects of different types

Ruby is dynamically typed: objects play roles

We need to be able to compare objects of different classes where a normal value comparison would fail because the properties of the two objects are different in at least some respects.

Without any modification, comparing two objects of different types using `==` will call the `super` method, and hence compare on the basis of identity - which will always return false

### The Comparable Module (Mixin)

Comparable is used by classes whose object can be ordered (sorted)

`<=>` is defined in the implementing class, and is the basis for the methods `<`, `<=`, `>`, `>=`, `==`, `between?` and `clamp`, included from Comparable

`object <=> other_object` returns -1, 0, or +1 depending on whether object is less than, equal to, or greater than other_object

if the objects cannot be compared using the defined criteria, the method returns nil

```ruby
```

## Overriding the hash method?

I have an Item class which stores the item name, quantity and price. You have to implement the equality methods for this object. Remember, you have to:

Define a == method that compares the state of your object with that of the other one and returns a boolean value.
Define a eql? method that simply calls the == to do the actual comparison.
and
Define a hash method that returns the result of XORing (using the ^ operator) the hash of all that instance variables which together determine the state of the object.

https://commandercoriander.net/blog/2013/05/27/four-types-of-equality-in-ruby/
http://rubymonk.com/learning/books/4-ruby-primer-ascent/chapters/45-more-classes/lessons/105-equality_of_objects
https://medium.com/rubycademy/object-equality-in-ruby-667f00c40194
https://medium.com/@khalidh64/difference-between-eql-equal-in-ruby-2ffa7f073532

## References

<https://apidock.com/ruby/Object/eql%3F>
<https://ruby-doc.org/core-2.6.5/String.html#method-i-3D-3D-3D>
<https://ruby-doc.org/core-2.6.5/Comparable.html>
<https://batsov.com/articles/2011/11/28/ruby-tip-number-1-demystifying-the-difference-between-equals-equals-and-eql/>
<https://tasdikrahman.me/2019/03/21/object-equality-object-oriented-programming/>
<https://truffles.me.uk/rubys-equality-operator>