# Mob Programming and the Power of Flow
  -- Woody Zuill

Tech Suggestion
- 80" 4k monitor for a team of 4-5

Software devs are at the pointy end. We get told what to do.
Consider how to avoid activities that are contrary to your wellbeing.

"The value of another's experience is to give us hope, not to tell us how to whether to proceed."
  -- Peter Block

MontyBoy.net
- we all tend to take random data and find patterns

Almost nobody at the meet up had tried mob programming, although most had heard of it.

## Definition
All the minds working
- on the same thing
- at the same time
- in the same space
- at the same computer

## Why Pair / Mob
Working alone, you get the best and the worst of yourself in the work.
Working together, you get the best of each of you, and avoid the worst completely.

The feedback loop gets much much shorter - you often avoid the fix - bug - fix cycle

### Specific Benefits
- knowledge sharing
- continuous code and design review
- many perspectives
- work on the right things
- rapid feedback
- better solutions
- flow of work items
- higher quality

### Why does it work
- together you get more done than the combination of your solo efforts
- the things which do get done are more useful
- code was easier to read, cleaner, and better quality


## How
Gather people who _together_ have all the needed skills and knowledge
e.g. tester, product owner, facilitator, db administrator, experienced dev i.e. knowledge of tools, old hand i.e. knowledge of system/logic, new eyes i.e. notice assumptions

Share the cognitive load
- you don't need to understand everything and pay attention to everything
- understand enough to play your part

### Effectiveness, not efficiency
- build the right things, not lots of things most of which are the wrong things
- efficiency: busywork
- productivity: delivering the wrong things
- if you can, avoid building the wrong things altogether rather than having to learn by doing it wrong first

### Ask the right questions
- Transformation comes more from pursuing profound questions than seeking practical answers
- If you don't know how to ask the right question you'll discover nothing
- Take a question, and ask it in exactly the opposite way

### So, why mob?
- How can we be effective with 5 people at one computer? Upended...
  - Does it make more sense to have 5 things being worked on, or one?
  - How can we be effective if we separate the people who should be working together?
    - coordination and communication costs will go up if you split people up

- What are the things that prevent effectiveness?
  - communication problems
  - decision making problems
  - doing more than is barely sufficient
  - technical debt
  - thrashing
  - politics
  - meetings

- How long does it take you to get an answer to a question that's blocking you?
  - 1 hour - 2 days is the average span
  - imagine if you were only allowed to steer your car every 30 secs? how slowly would you have to go?
- Bad Solution: while you're waiting, just work on something else...  
  - Working on more than one thing means that you are busy all the time, but also blocked all the time, and having to context switch, and hence being unproductive
  - Inventory: work started on, but not yet delivering value
    - aka waste
- Good Solution: get answers and continue working on the original task...
  - Ensure all the skills, knowledge and experience needed is in the room at the same time
  - Ensure your product owner is available to answer questions immediately / synchronously
    - They need to prioritise the delivery team, not just the users


### Driver / Navigators Method
- _not_ one person working and the rest watching & trying to figure it out (that's mob theatre)
- _not_ only one person navigating the whole time (each navigates when their expertise is required)
- _not_ one person insisting on doing things their way
- _not_ anyone saying "that's not going to work"
- **driver**
  - at the keyboard
  - a smart input device
  - enables the navigators to 'talk' to the computer
- **navigator**
  - everyone who isn't at the keyboard can be (should be) a navigator
  - speak your idea, don't encode it directly so that the watchers have to decode it

"For an idea to go from brain to computer, it must pass through _someone else's_ hands"
  -- Llewellyn Falco

### Flow
**psychological flow**
- you and the thing you are doing cannot be separated
- requires the right balance between challenge and skills
  - Mihaly Csikszentmihalyi

- do we prevent flow when we work as a team?
  - no
  - you all focus on one task
  - you each bring your own speciality/skill
  - you have everyone you need in the same place at the same time to get the work done
- c.f 'Team Flow, from concept to application', Jef van den Hout  

**lean flow**
- complete production of one piece, from start to finish
- with
  - as little work in process (inventory)
  - as little waiting between operations (queueing)
- as possible
- c.f. Flow, The principles of product development, Donald Reinertsen

- queues are the root cause of the majority of economic waste in product development
  - increase
    - time from inbox to delivery (cycle time)
    - risk
    - variability (inconsistency in operations/support particularly if there's a long delay between a person doing something and doing it again)
    - overhead
  - decrease
    - quality
    - motivation


**software development flow**
- each story flows directly from idea to delivered, working software directly
- idea -> story -> design & code -> deploy to real use -> feedback -> evaluation (what did we learn) -> idea (something new to learn)
- each iteration should be small, inexpensive, and aim to deliver value

**mobbing optimises for the flow of the work, rather than the output of the individual**
- not the most of each person
- the best of each person

_Mobbing = Flow++_
- individual flow:
  - each person has the safety and space to think in their own way
  - we can take time out from the mob if needed
- team flow:
  - we all work well together
  - progress is much quicker
- lean flow:
  - eliminate queues
  - eliminate inventory
  - we pause to think, rather than to work on something else

### What is 'the right work'
- getting the right work done includes exploring, experimenting, discovery
- the wrong things are waiting, merging, arguing, discussing rather than trying things, work that doesn't need to be done, bug fixing, meetings, coordination, prioritising, building the wrong thing

"The object isn't to make art, it's to be in that wonderful state which makes art inevitable"
  -- Robert Henri

## Managing Mobbing Devs
- if each team is working on one thing at a time, it becomes much easier/simpler to keep track of what is being done
- virtual mobbing is possible
- preconditions for successful mobbing
  - practice how to hear other people's ideas and being willing to try them
  - it's not the kind of work you're doing, it's your skill at teamwork
  - musician practices solo but also as a team: devs seldom do
  - be the best at getting the best out of others
- if there is a dependency between teams
  - consider working together on the interface
  - consider splitting/decoupling the project
- you cannot move gradually from coupled to decoupled: each system knows how to perpetuate itself!
  - instead, create a bubble where it's safe to try something new
  - move fast, if you can
  - the more teams in an organisation, the more difficult it is to work efficiently
- if a team tries something that doesn't work for more than a few hours, there's something wrong with the team dynamic

## When do you mob
- you can mob all day every day
- some do a few hours a day together in a mob, a few hours pairing
- you don't need to all be in the mob at all times
- lunchtime is break time for everyone
  - no-one gets to carry on coding alone
  - no-one talks about code over lunch
  - give your brain a proper break
- if you are working on a one-person problem, you should probably automate it

Email Woody directly for the slides
woody@woodyzuill.com

```
Mob Programming - A Whole Team Approach
www.leanpub.com/mobprogramming
```
