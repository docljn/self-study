- https://stackoverflow.com/questions/24476194/difference-between-static-and-this
- static::TEST can be used inside a static method, whereas $this::TEST requires an instance before being used (so non-usable in static methods).
- self refers to the class where the method is defined, but there's no difference between $this:: and static:: in non static methods.
- Only static can do this:
  - public static function foo() {
    static::FOO;
    }
- Only $var:: can do this:
  - $obj = new Foo;
    $obj::FOO;
- Both can do this:
  - public function foo() {
      static::FOO;
      $this::FOO;
    }

- The difference is that static:: uses "late static binding". Find more information here: http://php.net/manual/en/language.oop5.late-static-bindings.php
- "Late binding" comes from the fact that static:: will not be resolved using the class where the method is defined but it will rather be computed using runtime information. It was also called a "static binding" as it can be used for (but is not limited to) static method calls.
- Static references to the current class like self:: or __CLASS__ are resolved using the class in which the function belongs, as in where it was defined:
- self:: Isn't inheritance-aware and always refers to the class it is being executed in.
- static::
  - is inheritance-aware (from php5.3+) and references the class that was initially called at runtime
  - can only refer to static properties
  - in non-static contexts, the called class will be the class of the object instance

I still do NOT understand this!

- additional reference
  - https://www.leaseweb.com/labs/2014/04/static-versus-self-php/
- when you use “self::” you refer to the class in which the method/variable is defined, or where the 'new' keyword is written
- when you use “static::” you refer to the class from which the method/variable was called i.e. this allows for inheritance to be taken into account?
