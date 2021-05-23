# Composition not Inheritance
  -- Sandi Metz
  https://www.youtube.com/watch?v=OMPfEXIlTVE

## Part 2: The House That Jack Built (Inheritance vs Composition)
### What's the difference?
- inheritance is for specialisation NOT for sharing code
- naming can be really misleading
- inheritance means A _is a_ B if A inherits from B
- composition means A _uses_ B which _does_ C

### Composition is often the hidden pattern when you get stuck with a situation that looks like it needs multiple inheritance
- we can use dependency injection to achieve composition
- reveal how things are different by making them more alike
- work out what you are changing and give it a name
- once you've done that, you have the possibility of defining (and injecting) the required **role** in the classes you were trying to subclass in the first place
- the result is a single class which can provide different behaviour depending on the roles injected
  - e.g. pdfBuilder(podDataLayer), pdfBuilder(jobDataLayer) instead of pdfBuilder(arrayOfJobIds = null, arrayOfPodIds = null) with conditional branching to create the correct datalayer depending on which array is populated

- do watch the video, as the example is excellent (and funny)


## My thoughts based on additional reading
- mixins in ruby appear to be a way of shoehorning multiple inheritance into a single inheritance setup
- traits in php have the advantage of letting you know if you've defined the same method in two different places
- traits make it obvious that you never intend to instantiate the thing described/modelled by the trait
- traits can make it hard to develop the right abstraction, because the duplicated code is hidden
- it would appear that defining the right abstraction needs at least 3 instances of reuse
