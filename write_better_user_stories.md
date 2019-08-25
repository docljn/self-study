# Write better user stories

-- a Mountain Goat Software Webinar with Mike Cohn

<https://www.mountaingoatsoftware.com>

## Background

- user stories mean you can kick a project off in a lightweight way
- use-cases are NOT user stories
- getting user stories right is critical to their (and your) success
- not everything is a user story
  - job stories
    - <https://jtbd.info/replacing-the-user-story-with-the-job-story-af7cdee10c27>
    - "When _____ , I want to _____ , so I can _____ ."
  - feature driven development
    - <https://www.mountaingoatsoftware.com/blog/not-everything-needs-to-be-a-user-story-using-fdd-features>
    - "[action] the [result] [by|for|of|to] a(n) [object]"

## Techniques for success

### Get clarity on the overall business goal

- you need a vision
- you need to know where you're headed
- you need something bigger than a sprint!

### conduct a quarterly story-writing workshop

- what
  - don't write a few stories each sprint
  - write 3 months' worth instead
- why
  - you can focus on the bigger picture
- how
  - set aside an afternoon (probably two for the first few attempts!)
  - pick **one** (at most two) significant objective for the quarter to focus on: your Wildly Important Goal
    - involve all stakeholders in selecting the right objective
    - aim for maximum learning for minimum effort/input
  - involve the whole team in writing the stories to achieve that goal in one brainstorming session (12 - 15 max)
    - more clarity around what is actually needed
    - increased creativity
  - visualise user stories with a story map (Jeff Patton)
    - a grid of cards
    - each card is one user story
    - between columns, insert the word 'then' to create a logical sequence
    - between cards in a column, insert the word 'or' to show alternative actions/behaviours in descending priority order
    - the 'header' cards are usually not specific stories which will be implemented: the options below will be implemented
- benefit
  - the old way means you will often be working on what is most urgent, not what is most important
  - writing the stories for the quarter up front means you all have a clear idea of what is important, and can choose to focus on that instead of what appears urgent at the start of each sprint
  - overall progress towards the goal will be much greater

### Split stories to demonstrate progress even if the result is not truly shippable

- why is splitting so hard?
  - how do you know if the story is small enough
  - every story is different
  - you're trying to split a story when you don't fully understand what is needed
  - you're busy on the current iteration while trying to split stories for the next one
- what goes wrong when you don't get splitting right?
  - stories take more than one iteration
  - inconsistent & unpredictable velocity
  - planning a new iteration is hard
  - disappointing sprint reviews because work isn't really done: "software is 90% done for 90% of the time"
  - the more you do, the more you learn about what needs to be done, so the more there is to do
- remember
  - potentially shippable doesn't mean truly shippable
    - it means high quality, tested, and what it does, it does well
    - it does **not** mean a cohesive set of functionality
- how to split a story
  - make sure the whole team is involved, not just the product owner
  - think about functional increments 
    - something that you'd get a progress payment for providing if you were a contractor
    - e.g. a bit of the UI plus the db query to support it
    - aka vertical slices / walking skeletons
  - **don't** split along technical boundaries
- successful story splitting approaches often involves splitting a compound story using SPIDR
  - spike
    - as a last resort, set aside time for learning rather than building new functionality
  - path
    - consider the paths through a story and split each path into its own story
      - look for paths that are big enough to deliver value
      - don't split out a story for every single path!
    - if a lot of complexity is in a single step, consider splitting that step into its own story
  - interface
    - different user interfaces eg. browsers, iOS, Android
    - complex ui
      - consider a first iteration without design/bells/whistles like typeahead, then add a better user experience in subsequent iterations
    - different data types
      - file import from csv, xlsx, xml, json
  - data
    - develop a limited initial story that supports only a subset of the required data
    - if you're developing a system that uses various factors to predict when a user should take action, consider each factor in separate stories
    - consider hardcoding data that will be provided by functionality in subsequent iterations
  - rule
    - this could be a business rule, an industry standard, a performance requirement
    - if a rule adds complexity, add it in a subsequent iteration
  - <https://blogs.itemis.com/en/spidr-five-simple-techniques-for-a-perfectly-split-user-story>
- remember the advantage of splitting a story is getting functionality in front of users and stakeholders more quickly so that you get feedback sooner
- don't spend too much time trying to split a story

### Add just enough detail, just in time

- what happens when you get it wrong
  - too much detail too soon risks wasting time if the story is dropped or postponed
  - too little detail, or detail provided too late, risks delaying progress as the team becomes stuck waiting for information
- remember
  - you cannot remove all uncertainty before starting work
  - trying to do so results in a 'too much too soon' scenario and loss of agility
- err on the side of too little information, too late
  - you avoid wasting time up front
  - the problem of too little detail becomes obvious much more quickly
    - team members are stalled
    - too many user stories being worked on concurrently
  - if you don't overlap work, and focus on eliminating uncertainty before anything starts, everything takes far long but the problem isn't obvious at all
- to help team members who are uncomfortable with uncertainty
  - if someone requests more detail, ask: do you need the answer to that question before you start on that story (or only before you finish work on it)?
  - in every sprint retro, ask: did we get answers just in time, in just enough detail?

### what getting it right looks like

- less time in product backlog refinement
- team members embrace uncertainty
- calendar time to deliver a feature reduces
- customers are delighted

### After story planning

- any areas of disagreement or those needing further exploration should be itemised and time prioritised for dealing with these before work starts on the story

- acceptance criteria must be binary: true or not
  
- sprint planning splits things further
  - that's when you break things down functionally, not in the user stories

### Tools for distributed teams

- <http://www.cardboard.com>
- <http://www.miro.com>
- <http://www.mural.com>
- <http://www.lucidchart.com>

## Other references I want to look into

- <https://www.jpattonassociates.com/user-story-mapping/>
- <https://www.mountaingoatsoftware.com/blog/five-story-splitting-mistakes-and-how-to-stop-making-them>
- <https://blogs.itemis.com/en/what-are-good-user-stories>
- <https://opensource.com/open-organization/17/6/experiment-impact-mapping>
- <https://www.angryweasel.com/ABTesting/modern-testing-principles/>
- <https://netmind.net/3-cs-user-stories-missing-c/>
- <https://www.cleverpm.com/2018/06/21/user-stories-arent-enough/>
- <https://www.mountaingoatsoftware.com/blog/why-the-three-part-user-story-template-works-so-well>
- <https://agileforall.com/resources/how-to-split-a-user-story/>
