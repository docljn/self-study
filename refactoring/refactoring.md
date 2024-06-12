# Refactoring

"A change made to the internal structure of software to make it easier to understand and cheaper to modify without changing its observable behavior… It is a disciplined way to clean up code that minimizes the chances of introducing bugs."
-- Martin Fowler, Kent Beck

Make the change easy. This may be hard.
Then, make the easy change.
-- Kent Beck?

Changing the structure of a thing without changing its functionality.

Mostly in very small steps, while coding, and not in big batches.

Mostly for making future changes easier.

The way I’ve seen it used is too often, “I didn’t write this code, and I don’t like it, so I’m going to completely rewrite it”

When most developers talk about “refactoring the code”, they really mean “restructuring the code”. Rewriting it. Certainly to make it better and fix some bugs. That will be a mix of re-organizing the code, changing the modeling, and modifying some behavior along the way — for the best of course!

One reason why developers are so keen to rewrite code is that it is much harder to read code than it is to write it. For most developers, it is easier and more fun to write a new function than to work out how an existing one should be used.

The idea that new code is better than old is absurd - old code has been tested (if not in a test suite, then certainly in production)

When you throw away old code and rewrite from scratch you lose (potentially) years of bug fixes and knowledge about how real users interact with the system. You have absolutely no guarantees (beyond hubris) that you will do a better job than the original coders.

The only viable reason for refactoring is an economic one: in order to make it possible to deliver more features more rapidly, to be more responsive to the changes that people need. This is _why_ it's the right thing to do professionally, because otherwise we are deliberately slowing down development and negatively affecting our customers and the company bottom line.

Rather than try to make one big change, work out how to split it into many changes which are so small as to be almost insignificant alone. It's the _combination_ of refactorings, and the sequence in which they are done, which is really powerful. Spotting a code smell, recognising which refactorings might be useful to remove it, and how to combine a series of refactorings, is a key practice. To help with that, consider a 'code smell of the sprint' where you agree to look out for a particular code smell and consider which refactorings would be suitable to remove it.

When would you be pushed to a rewrite over a refactor:

* when you are using a language or a platform which cannot move forward (no longer supported, obsolete, etc)

People tend towards rewriting rather than refactoring because, again, code is easier to write than to read.

## Working through Articles, one by one

### What it is and what it isn't

<https://www.javacodegeeks.com/2012/04/what-refactoring-is-and-what-it-isnt.html>

Fixing any bugs that you find along the way is not refactoring.
Optimization is not refactoring.
Tightening up error handling and adding defensive code is not refactoring.
Making the code more testable is not refactoring – although this may happen as the result of refactoring.
All of these are good things to do. But they aren’t refactoring.

Write tests where you can
Make structural changes in small, independent, safe steps
Test the code after each small step

Refactoring Patterns make it easier if you do not have an IDE or a language which makes automatic refactoring simple and safe
<https://refactoring.com/catalog/>

You don’t decide to refactor, you refactor because you want to do something else, and refactoring helps you do that other thing.
Don’t refactor code that you aren’t changing or preparing to change.

#### Scratch Refactoring - refactoring to understand

Clean up code you don't understand or can't stand.
Rename variables and methods once you understand what they really mean
Delete code you suspect doesn't wok
Break up complex conditionals
Break up long methods, or inline methods which make code hard for you to reason about

Don’t bother reviewing and testing all of these changes. The point is to move fast – this is a quick and dirty prototype to give you a view into the code and how it works. Learn from it and throw it away.

#### Large Scale Refactoring - aka rewriting, re-platforming, re-engineering

Refactoring that takes a few minutes or an hour or two as part of a change is just part of the job. Refactoring that can take several days or longer is not refactoring; it is rewriting or redesigning.

Once you start using Branch by Abstraction, or the Strangler Fig Pattern, and you are working with old code and new code, with detours and scaffolding to support the interim structure, then although it's technically refactoring, it is neither quick nor safe, and should not be treated as a routine coding practice.

### To Refactor or not to refactor

"I want to have not just code, but a masterpiece that is pleasant to look at, that is pleasant to work with at any stage of the project."

That's an admirable desire, but a dangerous goal - refactoring all the things all the time is a bad idea.

#### STOP

Don't start refactoring before the feature has been released / before you get feedback.
You might end up rewriting the whole thing several times as your understanding improves - and then find on release that your assumptions were incorrect. Better to release a prototype and get feedback before it's perfect.

If the code is adjacent to your current task, is working, and is not actually connected to your task - leave it alone!

Assume that after refactoring you have quite understandable code that works in an obvious manner, that doesn’t contain kludges and that is quite easy-to-use. Stop! There is an infinite list of improvements between good code and ideal code - and attempting to achieve the latter will prevent you from actually shipping things that customers will use.

It's 2 days before the release date. Stop! You should be stress testing, regression testing, fixing critical bugs - not refactoring. There is always the risk that a simple refactoring will hit some hidden coupling and you'll end up with a user-facing critical bug on release day.

The code is old, and nobody is left who know why it was designed that way, and you think it looks ugly. STOP! This is usually step one on the journey of The Big Rewrite, and the more obvious you think the refactoring is, the more likely you are to trip over something that the original coder explicitly had to work around (think bugs in third party code, weird race conditions, processor-specific optimisations)

#### START

Your aim should always be code which is readable, understandable, convenient to use, and easy to maintain and develop.

* tidy first: if you are in between projects, consider renaming variables or method names to make them more explicit
* redesign: that's not refactoring, so if you're going to do that, be honest about it
* here be dragons: also not refactoring
* time box: if the code makes you sad, allow yourself time (deadlines permitting) to improve it in small ways, but don't cause deadlines to slip

### Code Refactoring Best Practices: When (and When Not) to Do It

The basic purpose of code refactoring is to make the code more readable, more efficient and more easily maintainable

Good times to refactor:

* before you start work on a new piece of functionality
* just after launching a product

Refactoring is part of the TDD Cycle

* red
* green
* refactor

“It’s like I want to go 100 miles east but instead of just traipsing through the woods, I’m going to drive 20 miles north to the highway and then I’m going to go 100 miles east at three times the speed I could have if I just went straight there. When people are pushing you to just go straight there, sometimes you need to say, 'Wait, I need to check the map and find the quickest route.' The preparatory refactoring does that for me.”
-- Preparatory Refactoring (Jessica Kerr)

Scope your refactoring before you start - small, safe changes, testing after each change

Focus on progress, not perfection

### Too Many Developers Get Refactoring Wrong

"Refactoring is a controlled technique for improving the design of an existing code base. Its essence is applying a series of small behavior-preserving transformations, each of which is “too small to be worth doing.”
-- Martin Fowler

Be very careful before starting a Refactoring Project: you are likely trying to fix issues that were caused by creating the wrong abstractions, anticipating a future that never arrived - by attempting to predict the future again....

Be careful with your abstractions. Better to wait until the shape of the abstraction is clear than try to predict the future.

"It’s the CTOs responsibility to help developers skill up. It’s the CTOs responsibility to create a professional engineering culture without shortcuts and bad coding. Don’t give in to pressure, keep your professional engineering up. It’s not easy for the CTO to give pushback on pressure, but it can be done—you’re not going to be fired."

### Refactoring: Where Do I Start?

"refactoring is a key skill to develop as rewriting costs more and is tolerated less"

#### Refactoring vs Rewriting

When refactoring:

* we maintain the behavior of the current system
* we begin with the existing code and change it over time as needed
* we can steadily improve existing code over time without disrupting the release schedule. We can release code, even in the middle of a refactoring.
* we can start with less up-front work, change direction when needed and stop part-way through if that’s appropriate.
* we often support parts of the old and new system at the same time

When rewriting:

* we might decide to change its behavior deliberately; or worse, we might accidentally change its behavior, introducing either incompatible changes or defects, both of which annoy users.
* we sometimes start from scratch, replacing large sections of code as needed.
* we often replace large sections of code at once, and we usually cannot release any part of a rewrite, because it has to replace an entire section of existing code at once.
* we usually have to plan the entire rewrite in advance, we need it to be proceed as expected or return from coding to planning, and we can’t stop until it’s complete.
* we usually ﬂip a switch to move from the old system to the new. This makes us less aggressive in putting the new system in place, so that we must live with the old system (and its problems) in its entirety for longer.

Before you start a rewrite, you need to understand and document all the business logic in the application you intend to rewrite.
Generally, with a legacy application, you have neither understanding nor documentation.

#### Slightly different definition

The act of identifying a specific problem with code, envisioning how to improve it, then improving it in small, reversible steps
Refactoring is also a noun: a particular code transformation which solves a specific code smell is "a refactoring"
A refactoring is reversible - and may or may not improve the quality of the code

#### Getting good at it

To refactor effectively, you must learn three things:

* How to identify code smells
* How to apply refactorings
* Which refactorings eliminate which code smells

Often it can be helpful to take a class and refactor it to within an inch of its life.
Don't do this with the intention of shipping your code, but with the intention of learning how one refactoring can result in another code smell, or how a particular sequence of refactorings can solve a particular problem

Refactor systems to make room for new features, rather than let changing needs slowly consign systems to the scrap-heap

#### Refactoring code you don't know

* apply the simplest, least-risky refactoring: renaming variables, classes and methods
* any other refactoring carries the risk of changing behaviour in unpredictable ways
* if you can't see the impact of a change, and it's not obvious where to add more tests, look for ways to make smaller, less-risky changes to give yourself a safe place to work
* identify boundaries, and introduce interfaces at the boundary of code you want to change, for example

#### Design Patterns vs Refactorings

* it is possible to use refactorings to evolve most of the popular Gang of Four patterns
* they need not be designed in up front, but can be evolved to as a system grows
* see "Refactoring to Patterns" - Kerievsky
* starting with a design pattern, rather than evolving into one, runs the same risk as early abstraction - you are trying to predict the future

#### How to know you are improving things by refactoring

Good code:

* passes all the tests
* minimizes duplication in all its forms
* expresses its intent clearly to the reader
* removes unnecessary elements

#### What if the code you want to refactor is untested

* if adding tests to the untested code is simple, do so
* alternatively, consider the 'replace algorithm' refactoring
* use TDD to rewrite / reimplement an untested method - anything bigger and you are straying into Big Rewrite territory

#### What can go wrong (technical)

You might:

* not have enough tests, in which case you could introduce defects
* apply narrow-minded refactorings that you need to undo later
* try to refactor before you understand what the code needs to do
* see unrelated problems while refactoring, then start fixing those, rather than dealing with the problem at hand
* stumble upon a defect and fix it
* stumble upon a small, missing feature and add it
* crawl down a rat-hole and not see how to back out of it
* refactor more than needed for the current task

Writing new code requires a completely different approach than refactoring.
If you notice a defect while refactoring, and can fix the defect in only a few minutes, reach a stopping point in the refactoring, check in the changes, then test-drive a fix for the defect, check in those changes, then return to refactoring.

#### What can go wrong (people)

* You and another person might engage in a refactoring tug of war: you apply a refactoring, then someone else applies the inverse refactoring, and so on
* You might find yourself the only person refactoring
* You might find others perceive you as going more slowly because you refactor
* You might make others look bad because refactoring helps you complete tasks more quickly than they do
* Another person might destroy a design you have painstakingly refactored
* You might find others perceive you as a careless designer because you refactor, rather than get it “right” the first time
* If your boss believes that refactoring slows you down, measure your progress as you refactor. Measure such things has defect rates, cycle time (the time from when a feature request arrives to when you are ready to ship it) and feature implement time (the time from when you start a feature to when you are ready to move it along the conveyor belt, such as to the testing team). If these measures improve, then you can provide compelling evidence to your boss that refactoring is a good idea and should continue. If these measure don’t improve, then you have identified areas you need to improve. In either case, you receive valuable feedback.

#### How to reduce the chances of things going wrong

* generally only rewrite when you are certain that you can rewrite quickly, safely, with an acceptable level of correctness, and with less effort than refactoring
* depending on how testable and maintainable the existing code is, it may be safer to test drive replacement code than attempt to cover existing code with tests
* after replacing a few sections of code with test-drive replacements, it may become safer and quicker to refactor rather than continue the rewrite

#### When to back away carefully and make no changes at all

* refactoring without a purpose means you have no way of knowing if the code changes you make improve the situation or not
* if you don't need to fix a defect or add a feature in a particular area, leave the code alone
* If you need to fix a defect, sometimes refactoring the code in question makes the offending lines of code obvious. Sometimes, refactoring can remove the place where a defect exists!
* If you need to add a feature, you can refactor code in the neighborhood to make room for the new feature. Done well, the new feature simply clicks into place.

### Profitable vs Unprofitable Effort

Consider the balance between code readability, developer competence, and the specific code you're looking at.

### You don't need permission to refactor if it's done in small enough steps

Writing code so that the team can keep a sustainable pace is your job. It's not something you should have to ask permission to do.

    "Any fool can write code that a computer can understand. Good programmers write code that humans can understand."
    Martin Fowler, Refactoring

Don't try to predict the future - speculative generality is a snare and a delusion.
In this context, making it right means making the code good in the current context.

* use good names
* remove duplication
* get rid of code smells
* keep methods small
* keep complexity low
* optimise code for reading (not writing)

When you find you have to add a feature to a program, and the program's code is not structured in a convenient way to add the feature, first refactor the program to make it easy to add the feature, then add the feature.

Refactoring should happen to facilitate a specific current change, not potential future change.

### You should never be completely satisfied with your code

If you are, then either you've over-engineered, the system is so small the design really doesn't matter, or you've been working on it so long that you can spend any and all the time you like on tinkering without jeopardising profitability

Instead, aim to design and code just well enough to avoid nasty surprises when implementing the next feature. Part of that is continuously challenging your assumptions about what the code and design actually need. If you are allowed to be wrong, with refactoring as way out of your mistakes, you can allow yourself to make decisions without agonising over every single detail. You must, however, be comfortable changing your decision in light of new information - even if your original decision still fits 80% of the proposed use case!

Any code change carries risk, and refactoring is no different. In one sense you could say it increases risk, because you change code more often than you might otherwise do.

Learning to do refactoring safely and quickly is a necessary skill. Just like TDD, though, it's not necessary to refactor your way to a good design when you already believe that, for example, MVC is a good fit. Do allow for the fact that you might be wrong, and be prepared to refactor away from that decision if necessary.

### Refactoring, the activity

Involves improving the design of existing code
    … in a sequence of small transformations
    … that preserve the important behavior of the system
    … which you can complete relatively quickly
    … and which gives you inexpensive options to change direction.

Learning how to refactor effectively involves learning how to combine the smallest of steps into a sequence that improves the code.
Learning to recognise which combination of steps will lead to steady progress in improving the design of the code takes practice (and often a mentor / apprenticeship)
In the same way that you learn to find your way around your code editor of choice, with plugins and shortcuts and keybindings, you can learn to perform the small refactorings without thinking and free up your brain to think about the bigger change you're aiming for.
Drill refactoring until you can do the nano steps without effort - write code to throw away: some example drills in here <https://blog.thecodewhisperer.com/permalink/breaking-through-your-refactoring-rut>

### Refactoring is a development technique, not a project (that's rewriting)

    Decomposition in computer science, also known as factoring, is breaking a complex problem or system into parts that are easier to conceive, understand, program, and maintain.

    Refactoring is intended to improve nonfunctional attributes of the software. Advantages include improved code readability and reduced complexity; these can improve source-code maintainability and create a more expressive internal architecture or object model to improve extensibility.

    A rewrite in computer programming is the act or result of re-implementing a large portion of existing functionality without re-use of its source code or writing inscription. When the rewrite is not using existing code at all, it is common to speak of a rewrite from scratch.

Refactoring, thus, is changing the factoring - changing the way in which the system has been composed or problem deconstructed

If done well, code refactoring may help software developers discover and fix hidden or dormant bugs or vulnerabilities in the system by simplifying the underlying logic and eliminating unnecessary levels of complexity. If done poorly it may fail the requirement that external functionality not be changed, introduce new bugs, or both.

Avoid the idea that refactoring === massive cleanup, and that cleaning up a codebase happens in massive batches or not at all.

Spend ten minutes a day doing cleanups that are too small to matter, and over time you see results which really do matter. If you haven't learned to do the daily tidying, then any big cleanup will be unsustainable (and frankly pointless).

### How to know if you're refactoring

These are all good things, but they are not refactoring.

* fixing bugs you find as you refactor
* optimisation
* adding error handling
* making code more testable
* redesigning, replatforming, or re-engineering a system (significant changes, with significan risk)

Refactor, then change, then refactor, and repeat....

You refactor code before making changes, so that you can confirm your understanding of the code and make it easier and safer to put your change in.

The scope of your refactoring work should be driven by the change or fix that you need to make – what do you need to do to make the change safer and cleaner? In other words: Don’t refactor for the sake of refactoring. Don’t refactor code that you aren’t changing or preparing to change.

### Manual vs Automated Refactoring

Modern IDEs have automated a lot of the tedium of refactoring.
However, using these tools can mean you lose context: you don’t get to see the wider effects of the refactoring you’re attempting.
At a minimum, consider reviewing your own PR before opening it up for formal review - the overview may help you see where you have made local improvements and missed the bigger potential gains. You need to switch your mindset from creator to publisher in order to do this, and a formal review can help with this switch.

### Don't mix refactorings with behaviour change

That includes reformatting and automated linting!

* it adds risk as it's harder to see the effect of any single change in isolation
* it makes bug attribution harder and means you likely have to roll back both changes
* it makes code review _much_ harder given it's impossible to tell which changes were for which reason

At an absolute minimum, separate commits in which you refactor from commits in which you change behaviour

### Avoid wasteful refactoring

Use heuristics - rules of thumb - for "good enough" code (clear names, short methods, small classes etc)
Stop refactoring as soon as you're unsure whether you are improving the code or not
Don't commit changed code just because you changed it - if it's not an obvious improvement then all you're doing is replacing familiar code with unfamiliar code
Refactoring in support of an immediate change means you are much more likely to be aware of the ways in which the existing structure does not support the new requirements - and that's a great time to refactor.

### Refactoring in a Legacy Codebase is a different animal

Read the book - Working Effectively with Legacy Code (Michael C. Feathers)

* Identify pain points
* Break dependencies
* Cover the desired piece of code that’s about to change with tests
* Make your changes
* Refactor all you want afterwards.

### Don't comment your code: refactor it

Replacing a commented section of code with a well-named method has several advantages

* the method name won't go out of date the way a comment might
* the parent method becomes shorter and easier to read

Learning to refactor by replacing commented code with named methods is a gentle way to start

If you feel the need to comment your code, consider what you are apologising for and whether you can fix it rather than apologising

### Good habits have a price. Bad habits have a cost. Either way, you pay

Refactoring is a good habit...

We either refactor continuously as we build features – paying the price. Or we slowly accumulate cruft until our codebase is impossible to change – paying the cost.

### Refactoring stories are a bad idea

* they will always be deprioritised in favour of new (revenue-generating) features
* they encourage corner-cutting with the promise of some future 'refactoring' to make it all good
* validation and QA of refactoring stories is either prohibitively expensive or skimped
* if you're improving non-functional parameters, be honest and treat that as a technical story, not a refactoring

### Refactoring is inevitable

As we code we discover unknown things. This is expected, and desireable.
Unfortunately, what often happens is that the developer is blamed for not being proactive and foreseeing the changes during the original refinement.
Instead, what should happen is that these newly discovered requirements should be discussed and explored to see whether they will fit within the existing design, or whether changes need to be made to the design in order to support delivery.
If the deadline allows, refactoring should be done before implementing the feature. If not, then refactoring should be done immediately following release.

### Don't put refactoring on the backlog

The metaphor is a codebase as a field through which you have to make a path.
Over time, brush, scrub, thorns, rubbish etc builds up if you don't make an effort to keep the field orderly - it's not bad code, or technical debt, it's the gradually accumulation of 'not very good code'.
Don't try and clean up the whole field: you won't have enough time to do a good job, the result will be disappointing to everyone who thought you were going to fix all the things, and you'll be less likely to get the time again.

Instead take the next feature that we are asked to build, and instead of detouring around all the weeds and bushes, we take the time to clear a path through some of them. Maybe we detour around others. We improve the code where we work, and ignore the code where we don't have to work. We get a nice clean path for some of our work. Odds are, we'll visit this place again: that's how software development works.

With each new feature, we clean the code needed by that feature. We invest a little time more than we would if we continued to trash the field, but not much more -- and often less

### Don't refactor yourself out of business

Stopping work on new features in order to refactor is, firstly, not refactoring - that's a rewrite/restructure
Secondly, it suspends the company learning feedback loop for the duration, meaning you might be investing time rewriting features which turn out to be irrelevant once the company starts learning again.
Assume that you will never know less than you do now - and thus that even if you write the best code you can, using the current best practices, it will still be less good than code you write in the future.

To ensure you aren't wasting your time and money

* insist on incremental improvements, no matter how big the legacy code problem seems to be
* only work on code you are going to change anyway: don't invest time on systems where there will be no benefit to customers from your work
* only make durable changes where you can be certain that the improvement will persist and will not break something else in the process i.e. with good unit test coverage and continuous integration, with appropriate production monitoring and alerting if the fix comes undone in future, according to team-agreed coding guidelines
* share what you learn so the whole team benefits from your hard won experience
* as a manager, ensure your team is aware of the financial and other constraints under which the company is operating so they can make appropriate trade-off decisions
* take the time to really explore what isn't working: e.g. refactorings that never complete, making incremental improvements but still feeling stuck in the mud, making the same mistakes over and over again, schedules slipping by increasing amounts, and month-long refactoring projects when you only have three months of cash burn left.

### The road to refactoring (how to make success more likely, and get stakeholder buy-in)

#### Don't mix refactorings with hotfixes

Your priority here is to fix the actual problem, with as few high-quality code changes as possible, to make for a green test reproducing the bug, quick code review, passing CI, and QA. Do _not_ start tidying up as you go!

#### Always provide customer value

The smallest refactorings often provide the greatest value over time - renaming a variable, extracting a function, inlining an abstraction. These are often things that will be suggested during the code review process: don't discount them because they are small. They combine to make for code which is more readable and more easily human-understandable.

If you cannot do the refactoring on the fly - it's too big to do in tens-of-minutes - consider whether this is a refactoring or a rearchitecting/re-engineering/replatforming. These can still be justified by e.g. the need to rearchitect code before a new feature can be implemented, but they should not be discussed in the same way as refactorings. This kind of change is technical requirement - a pre-requisite for implementing the new feature.

#### Use urgency to justify big rewrites

Selling a rewrite "because it's going to cause problems" is incredibly difficult. You need to take the opportunity provided by a production incident, a security issue, or a key customer complaint, to justify putting in the work. The window of opportunity to convince your stakeholders that the effort is justified may be very small - so be prepared with at least a basic idea of how you would fix issues you know about

#### Refactor impactfully

Cosmetic refactorings add risk without benefit. Replacing axios with a native 'fetch' would be nice, in the sense that it's more modern, but it would also end up touching pretty much every page in an application and require an enormous regression testing effort without adding any value to customers or developers. Axios has a perfectly nice API, it's known to the team, and while modern libraries are smaller, the difference isn't meaningful.

## Code Smells

Each code smell has a corresponding refactoring, or sequence of refactorings.

Code smells can be categorised into the following categories :

* Bloaters
* Object Oriented Abusers
* Dispensables
* Couplers
* Change Preventers

<https://refactoring.guru/refactoring/catalog>

## Refactoring Workflow

Why do you want to refactor?

Are your tests green?

Are you currently changing functionality?

What's your code smell?

What is the refactoring sequence to solve the code smell?

## Books

<https://refactoring.com/catalog/>
<https://www.industriallogic.com/refactoring-to-patterns/>

## Articles

<https://aakinshin.net/posts/refactoring/>
<https://agileotter.blogspot.com/2024/05/profitable-struggle-and-unprofitable.html>
<https://www.altexsoft.com/blog/code-refactoring-best-practices-when-and-when-not-to-do-it/>
<https://www.amazingcto.com/too-many-developers-get-refactoring-wrong/>
<https://blog.jbrains.ca/permalink/refactoring-where-do-i-start>
<https://blog.ploeh.dk/2022/09/19/when-to-refactor/>
<https://blog.thecodewhisperer.com/permalink/why-refactoring-is-not-always-a-code-smell>
<https://blog.thecodewhisperer.com/permalink/breaking-through-your-refactoring-rut>
<https://daedtech.com/refactoring-development-technique-not-project/>
<https://dzone.com/articles/what-refactoring-and-what-it-0>
<https://chrisoldwood.blogspot.com/2017/05/are-refactoring-tools-less-effective.html>
<https://www.codewithjason.com/dont-mix-refactorings-behavior-changes/>
<https://www.codewithjason.com/avoid-wasteful-refactoring/>
<https://docondev.com/blog/2020/1/4/refactor-you-keep-using>
<https://geekingwithmauri.com/work/refactoring.html>
<https://critter.blog/2020/09/15/dont-comment-your-code-refactor-it/>
<https://www.codesimplicity.com/post/refactoring-is-about-features/>
<https://www.germanvelasco.com/blog/refactoring-is-a-habit>
<https://hrishikeshkarekar.medium.com/refactoring-is-good-refactoring-stories-are-not-9ae71fcca96c>
<https://www.javacodegeeks.com/2012/04/what-refactoring-is-and-what-it-isnt.html>
<https://www.joelonsoftware.com/2000/04/06/things-you-should-never-do-part-i/>
<https://martinfowler.com/articles/extract-data-rich-service.html>
<https://martinfowler.com/bliki/RefactoringMalapropism.html>
<https://martinfowler.com/bliki/DefinitionOfRefactoring.html>
<https://martinfowler.com/bliki/RefactoringCringely.html>
<https://medium.com/codex/why-refactoring-is-inevitable-22df14dbd543>
<https://refactoring.guru/>
<https://ronjeffries.com/xprog/articles/refactoring-not-on-the-backlog/>
<http://www.startuplessonslearned.com/2009/01/refactoring-yourself-out-of-business.html>
<https://thoughtbot.com/blog/lets-not-misuse-refactoring>
<https://tkdodo.eu/blog/road-to-refactoring>
<https://understandlegacycode.com/blog/refactoring-and-defactoring/>
<https://wiki.c2.com/?WhyNotEnoughRefactoringHappens>
<https://zaidesanton.substack.com/p/how-refactoring-almost-ruined-my>

## Videos

<https://www.youtube.com/watch?v=PGjrn9Ci2aY>
<https://www.youtube.com/watch?v=gcSh-yXaXVs>
<https://www.youtube.com/watch?v=K7xSsNpeM8I>
<https://www.youtube.com/watch?v=CJI5p4n6ElI>
<https://gotopia.tech/articles/195/expert-talk-code-refactoring>
<https://www.youtube.com/watch?v=NeXlQX2E5iw>
