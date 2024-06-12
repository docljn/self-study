# Refactoring: an opinionated introduction

This is written from the perspective of a developer working with Ruby and Javascript - dynamically typed languages which have limited automated refactoring support in most modern IDEs.

## Definitions

Noun: a specific, reversible code transformation which solves a particular code smell. It may or may not improve overall code quality.

Verb: the process of applying a series of refactorings

"A change made to the internal structure of software to make it easier to understand and cheaper to modify without changing its observable behavior… It is a disciplined way to clean up code that minimizes the chances of introducing bugs."
-- Martin Fowler, Kent Beck

Improving the design of existing code
  … in a sequence of small transformations
  … that preserve the important behavior of the system
  … which you can complete relatively quickly
  … and which give you inexpensive options to change direction.

## Disambiguation

Refactoring is not the same as rewriting, restructuring, re-architecting, or replatforming.
Each of these is a valuable exercise, but it is not refactoring.

Other things that are commonly confused with refactoring:
Fixing any bugs that you find while working on a piece of code.
Optimization.
Tightening up error handling and adding defensive code.
Making the code more testable – although this may happen as the _result_ of refactoring.

Once you start using Branch by Abstraction, or the Strangler Fig Pattern, and you are working with old code and new code, with detours and scaffolding to support the interim structure, then although it's technically refactoring, it is neither quick nor safe, and should not be treated as a routine coding practice.

## When (and when not) to refactor: Make the change easy, then make the easy change

Writing code so that the team can keep a sustainable pace is your job. It's not something you should have to ask permission to do.

"Any fool can write code that a computer can understand. Good programmers write code that humans can understand."
--Martin Fowler, Refactoring

* use good names
* remove duplication
* get rid of code smells
* keep methods small
* keep complexity low
* optimise code for reading (not writing)

You don’t decide to refactor, you refactor because you want to do something else, and refactoring helps you do that other thing.

The only viable reason for refactoring is an economic one: in order to make it possible to deliver more features more rapidly, to be more responsive to the changes that people need. This is _why_ it's the right thing to do professionally, because otherwise we are deliberately slowing down development and negatively affecting our customers and the company bottom line.

The idea that new code is better than old is absurd - old code has been tested (if not in a test suite, then certainly in production). When you throw away old code and rewrite from scratch you lose (potentially) years of bug fixes and knowledge about how real users interact with the system. You have absolutely no guarantees (beyond hubris) that you will do a better job than the original coders.

Be very careful before starting a Refactoring Project: you are likely trying to fix issues that were caused by creating the wrong abstractions, anticipating a future that never arrived - by attempting to predict the future again.

Ideally, you want to refactor just before starting work on a new area of code, or just after releasing a new feature when you are receiving feedback from users to confirm your understanding of both the problem and solution. The scope of your refactoring work should be driven by the change or fix that you need to make – what do you need to do to make the change safer and cleaner? Don’t refactor for the sake of refactoring. Don’t refactor code that you aren’t changing or preparing to change. And stop refactoring as soon as you are no longer certain that you are improving the code!

Spend ten minutes a day doing cleanups that are too small to matter, and over time you see results which really do matter. If you haven't learned to do the daily tidying, then any big cleanup will be unsustainable (and frankly pointless).

"One reason why developers are so keen to rewrite code is that it is much harder to read code than it is to write it.
For most developers, it is easier and more fun to write a new function than to work out how an existing one should be used."
-- Joel Spolsky

“It’s like I want to go 100 miles east but instead of just traipsing through the woods, I’m going to drive 20 miles north to the highway and then I’m going to go 100 miles east at three times the speed I could have if I just went straight there. When people are pushing you to just go straight there, sometimes you need to say, 'Wait, I need to check the map and find the quickest route.' The preparatory refactoring does that for me.”
-- Preparatory Refactoring (Jessica Kerr)

### When to rewrite rather than refactor

If you are using a language or a platform which cannot move forward (no longer supported, obsolete, etc) and thus prevents future change, your only option may be to rewrite. This is, however, a risky approach as it essentially prevents any development of the product for the duration of the rewrite.

Before you start a Big Rewrite, you need to understand and document all the business logic in the application you intend to rewrite.
Generally, with a legacy application, you have neither understanding nor documentation.

Depending on how testable and maintainable the existing code is, it may be safer to test drive replacement code than attempt to cover existing code with tests.
After replacing a few sections of code with test-driven replacements, however, it may become safer and quicker to refactor rather than continue the rewrite

### Caveat: refactoring for understanding

If you are struggling to understand a particular piece of code, scratch refactoring (refactoring to understand) may be justifiable.
Try renaming methods and variables once you understand their purpose, or splitting complex conditional statements, or inlining methods which make the code hard for you to reason about.

Don’t bother reviewing and testing all of these changes. The point is to move fast – this is a quick and dirty prototype to give you a view into the code and how it works. Learn from it and throw it away.

## Requirements for safe refactoring

An automated test suite which you trust to detect regressions, and which is currently green and in version control.
Familiarity with code smells and the refactorings required to resolve them.
The discipline to follow the prescribed refactoring steps, including running the test suite after each small step, especially if you work in a language or environment which does not have good automated refactoring support.

## Pitfalls to avoid when refactoring

Combining refactoring with behaviour change - this includes automated linting and reformatting!
Refactoring more tha you need to achieve the current task.
Committing changed code just because you changed it - if it's not an obvious improvement then all you're doing is replacing familiar code with unfamiliar code.
Engaging in a refactoring tug of war: you apply a refactoring, then someone else applies the inverse refactoring, and so on.
Attempting to predict the future.
You might find others perceive you as careless because you refactor, rather than get it “right” the first time.

## Refactoring in a Legacy Codebase: a special case

Read the book - Working Effectively with Legacy Code (Michael C. Feathers)

* Identify pain points
* Break dependencies
* Cover the desired piece of code that’s about to change with tests
* Make your changes
* Refactor all you want afterwards when it's safe to do so

## Refactoring Workflow

Why do you want to refactor the code?

* it's ugly and makes me sad to look at
* it doesn't use the most modern paradigms
* it's inefficient
* it doesn't make sense
* it's difficult to test
* every time we change it, we introduce new bugs
* everybody is too scared to touch it

Are you working on, or about to work on, this area of the codebase?

* no _step away from this code - you'll do more good focussing on code which is about to change, and there's always the risk of an undetected regression_
* yes

What does your test suite look like in this area of the codebase?

* what test suite? _write some tests, and if that's not possible consider the "replace algorithm" refactoring_
* I've got unit tests for every method: private and public! _write some higher level tests so you can change the implementation safely_
* I've got feature- and/or integration-level tests for the most important functionality

Are your tests all green?

* no, I need to refactor to fix things _back out of your changes to the last point where tests were green, and commit_
* no, the tests apply to the old behaviour before I made changes _it is not safe to combine refactoring with behaviour change_
* yes, I changed the tests to make them pass after I made code changes _keep behaviour change and refactoring in separate commits_
* yes

Do you understand the code you are going to be refactoring?

* no _apply the simplest, least-risky refactorings: renaming variables, classes and methods_
* yes

Do you know which code smell you are going to start by fixing?

* sorry, what? _code smells are your pointer to the correct, safe, refactoring_
* yes

Do you know how to carry out the safe refactoring steps?

* steps? _safe refactorings have been documented: don't try to reinvent the wheel_
* yes, my IDE has a refactoring menu _excellent, but be aware that even the best tooling can make mistakes_
* yes, I have a reference book/website/cribsheet _excellent, take it carefully_

OK, you've completed the refactorings to fix this code smell - are you tests green?

* no, I'll need to fix them _something has gone wrong, stop and work out if your tests were at the wrong level, or the refactoring was not actually safe_
* yes _commit_

Any more code smells that you can see, or that this refactoring has exposed?

* yes _will fixing them make the change you intend easier? if not, stop and bask in the glow of 'good enough' code_
* no _stop and bask in the flow of 'good enough' code_

Are you completely satisfied with your code?

* yes _mistake, you've probably overengineered it and likely taken too long to do so, too_
* no _good, aim to design and code just well enough to avoid nasty surprises when implementing the next feature_

Assuming that after refactoring you have quite understandable code that works in an obvious manner, that doesn’t contain kludges and that is quite easy-to-use. Stop! There is an infinite list of potential improvements between good code and ideal code - and attempting to achieve the latter will prevent you from actually shipping things that customers will use. Progress, rather than perfection.

Once you are done, consider reviewing your own PR before opening it up for formal review - the overview may help you see where you have made local improvements and missed the bigger potential gains. You need to switch your mindset from creator to publisher in order to do this, and a formal review can help with this switch.
