# Growing an Early Career Developer

## References
### Video
- [Gretchen Ziegler @ RailsConf 2018](https://www.youtube.com/watch?v=S7cYJumzuL4)
- [K Wu @ RailsConf 2014](https://www.youtube.com/watch?v=GJW46x27W1w)
- [Mark Lehman @ RubyHACK 2018](https://www.youtube.com/watch?v=a85F-e_a0pg)
- [Mercedes Bernard @ RubyConf 2018](https://www.youtube.com/watch?v=Qspf0SX_oPs)
- [Louisa Barrett @ RailsConf 2015](https://www.youtube.com/watch?v=SaNlfSeTWwI)
- [Rebecca Poulson @ eurucamp 2015](https://www.youtube.com/watch?v=RK6l_l7TdB8)
- [Rachel Warbelow @ GORUCO 2015](https://www.youtube.com/watch?v=tYOx8mA5p2c)
- [Sumeet Jain @ RailsConf 2018](https://www.youtube.com/watch?v=K0vxOBIyhF0)
- [Markus Prinz @ Kod.io 2014](https://www.youtube.com/watch?v=cBZoZd44PeA)
-

### Audio
- [CodeNewbie Podcast with Ben Orenstein](https://www.codenewbie.org/podcast/how-do-i-level-up)
- [Good Life Project Podcast with Prof K. Anders Ericsson](https://www.goodlifeproject.com/podcast/anders-ericsson/)

### Text
- https://graphics8.nytimes.com/images/blogs/freakonomics/pdf/DeliberatePractice(PsychologicalReview).pdf

## Learn how to systematically improve your skills
- there's lots of advice out there on how to get better at specific parts of coding, but it's hard to find a system that will make you a better coder
- [Learning How to Learn](https://www.coursera.org/learn/learning-how-to-learn) is a woefully underrated skill
- simply wanting to improve isn't enough — you also need well-defined goals and the help of a teacher who makes a plan for achieving them: this is the concept of Deliberate Practice.
- without a coach, you can engage in Purposeful Practice, but the outcome is not guaranteed as you do not have the same short feedback loop and guidance
  - e.g. write down things which are taking you a long time to achieve, or where you feel inefficient, and then spent 30 mins a day sharpening your tools
  - engage in spaced repetition to help you consolidate your learning


## Levelling up: early career developer actions
### on your own
- challenge yourself / accept a challenge from your manager
- fix bugs
- practice: koans and similar on sites like [Exercism](https://exercism.io/), open source contributions [Code Triage](https://www.codetriage.com/) and [hacktoberfest](https://hacktoberfest.digitalocean.com/), and online courses can be very useful e.g. [The Odin Project](https://www.theodinproject.com/) & [freeCodeCamp](https://learn.freecodecamp.org/), however, be aware that a lot of these are aimed at beginners rather than those hoping to level up. [Upcase](https://thoughtbot.com/upcase/practice) and, to a lesser extent, [gitconnected](https://levelup.gitconnected.com/)  may be an exception there, along with some of the MOOCs on sites like like Udemy & Coursera, & Pluralsight if your employer is paying.
- read actual books: they can be horribly intimidating, but remember that an author has had a lot more time and input in honing their message than most blog posters ever will. (Apprenticeship Patterns: Guidance for the Aspiring Software Craftsman - Hoover/Oshineye)
- teach: nothing helps you solidify your learning better: [codebar](https://codebar.io/), [coderdojo](http://coderdojoscotland.com/clubs/)
- go to meetups: you'll be exposed to ideas and people that you otherwise might have missed
- study design & notice connections
- write down what you've learned: whether that's just for yourself, something to share via slack, or even a blog post
- work on your communication, networking and inter-personal skills
- be selective with what you decide to spend your time on: if it isn't a clear yes, then it must be a clear no
- if you aren't comfortable with unit testing and TDD, now is the time to start
- if you don't understand how git works, and how and why version control is vital, learn that now
- things like SOLID, DRY etc, along with CS concepts like Big O Notation can be worth spending time on - if only because it will help you follow what a more experienced coder is talking about and help you learn more quickly
- vim can be useful, but there are very few situation where you won't be able to use a different editor so be wary of spending too much time learning vim if it won't make much of a difference to your workflow
### with others
- participate in code review with your peers and seniors [Maria Khalusove & Trisha Gee](https://vimeo.com/182087729)
- pair / mob program
  - discuss expectations before you start
  - one computer only (possibly multiple keyboards)
  - the less experienced developer has the keyboard first and most often
  - in a mob, the most experienced developer should not use the keyboard much, if at all
  - if you tune out, take a break
- ask for help (rubber duck, then human)
  - when to ask for help
    - no forward progress for 30 mins, AND
    - taken a break from the computer and still stuck  
  - information to have written down before you ask for help
    1. situation details
    2. end goal
    3. everything you've tried
    4. what google searches you've made
    5. errors received
    6. how to reproduce the problem
- take the lead in a project
  - decide on architecture
  - break up project into manageable tasks
  - track progress
  - ensure test coverage
  - QA (manual and automated)
  - deploy
  - fix post deploy bugs


## Levelling up: manager/supervisor actions
- define what you want to achieve!
- be a janitor, not a rockstar: ensure your team gets the credit rather than you

### why do early career developers suffer
A lot of developers believe that their true skill lies in their ability to solve any problem alone, given enough time.
With this attitude, if a colleague appears to need mentorship, guidance or encouragement, the developer can see this as offensive, and a sign of personal failure on the part of that colleague.

### Empowerment
Making someone stronger and more confident through encouragement and support of their ability.

### Mentoring
Being skilled at Mentoring does not follow automatically from being skilled at your job: mentoring skills must be actively learned
- I've got a separate set of notes on that...

### Things to try when growing your early career developers
1. Codify a format to use when asking questions
  - see above...
2. Help your early career developers to formulate their questions to match the expected format
  - they will learn far more than if you just jump to the answer
  - some questions can't be formulated like this, and that's OK too
3. No technical conversations in private messages
  - everyone should be able to participate and learn from your experience
  - the team lead can see what the most frequent questions are, and hence where training might be needed
4. It's not that questions are stupid, but asking a certain type of question can make the asker feel stupid
  - provide guidance to a safe space to ask those questions e.g. anonymous public IRC
5. Seniors often assume that they need to explain basic coding knowledge to early career developers: more often, the lack is business knowledge or tooling knowledge
  - if your application has atypical abstractions, dependencies or patterns: document them & discuss their effect & the reasons you chose to implement them with your new developers
  - small classes are easy to read and reason about, but make understanding the overall functionality more difficult
  - consider screencasts as documentation: get a senior to talk though why a particular piece of code exists and what it does
6. Pairing
  - can fail miserably if the gap in skills/knowledge is too large
  - sometimes just watching an experienced dev work can teach you more, as the senior doesn't know what they know and hence cannot teach it
  - this is particularly useful for debugging in production, reading technical docs, etc.
7. Remember Retention and Promotion
  - affirm the hire: remind your early career developers that you know it is difficult and you believe they belong on the team
  - simply say: "you were a good hire"  
8. Support time-wasting
  - make sure people know that taking breaks and socialising at work is part of the job
  - to do that: as a manager, join in the fun/banter/gif-sharing so that it's obviously acceptable
9. Better use of investment time
  - prioritise depth over breadth (breadth is everywhere...)
  - give your early career developers something big to build that doesn't have stakeholders breathing down their neck
  - let them take the time to learn in depth: avoid context-switching
  - develop a culture where it's ok to fail, and learn from failure
10. Long form technical writing
  - write a tutorial / blog article every time you learn something as a early career developer
  - solidifies their knowledge
  - seniors can then add to/clarify/edit the article and everyone learns more
  - builds your company brand
12. Listen to your early career developers: they are generally more competent than you give them credit for  
13. Get your documentation right
  - "Can't you just use the annotations to see who wrote it?" is not documentation
14. Plan an appropriate first project for your early career developer
  - if you are onboarding properly, that first project is actually assigned to a pair made of your new early career developer and an experienced senior
  - also, consider a second first-project which can be done solo *at the same time* by the early career developer to build confidence/independence
    - something that will be visible...
15. Explicitly assign a mentor to each early career developer with a weekly meeting/catchup
  - that mentor should be available, and evaluated on their performance as a mentor
  - the company must understand that developing early career developers is just as important as building clean code for the future success of the firm
  - if you can't do that accept that you need to allow a lot of additional time for your early career developer to teach themselves
11. If any of your devs want to become a senior
  - let them 'audit' technical meetings and learn by watching
  - show them the work they will be doing after their next promotion
  - teaching and mentoring is one of the best ways to learn: so let your seniors and mid-level devs mentor your early career developers  
16. Ask for feedback from your team often, and remember to provide positive feedback whenever you notice them doing something well
17. Check in with your team
  - what skills do they want to work on
  - how are they feeling
  - don't assume that you know what challenges they are ready for
18. Identify the skills your company values in a senior engineer, and work out where the gaps are in your current team (often these are the things you, as a lead, have to do)


## What makes a senior dev
### Skills
- technical communication
- time management
- meeting facilitation
- self direction
- consulting
- client engagement
- product vision

### Experience
- making decisions
- accountability
- big picture
- leading a team

### To become a senior, an early career developer needs to be able to practice the necessary skills
- as a senior/lead:
  - how are you a silo - how do you hoard knowledge and experience that your team needs?
  - how can you be the ground crew rather the pilot, guiding your team towards success
- an early career dev could take on a lead role for a single feature in a larger project, with the support of the seniors, design experts, and the rest of the team
- make sure the project is big enough to share responsibility
- make sure your team believe that implementing these changes will be sustainable: achievable while still being challenging

## How to help your early career devs avoid typical problems/pitfalls
- NB: expert !== good teacher
### Problem 1: Structure (or the lack thereof)
  - develop a long-term plan
    - minimal essential skills that this developer needs now, next week, next month, next year
    - how are we going to get there
  - create a structure for pairing
    - how often
    - with whom
    - junior/junior AND junior/senior pairing
    - success = how much has the junior learned, not how much work was done
  - create a structure for giving and receiving feedback
    - career
    - code
    - personal  

### Problem 2: Imposter Syndrome    
  - make sure they know that they are expected to ask for help, and value their questions
  - have and share realistic expectations  
  - allow your new devs to see other developers struggle

### Problem 3: Surface-level understanding
  - use proven teaching techniques
    - whiteboarding/diagrams mean you process information differently
    - analogies bridge current and new knowledge
  - provide projects that build on or reinforce concepts
    - slight differences can help cement knowledge by highlighting patterns

## Strategies for Success
- build your network
- commit to a few regular meetups, and keep showing up
- grow into a mentor/mentee relationship
- consider company types: product vs consultancy/agency vs startup
- consider company size
- consider the ratio of juniors to seniors in your team: mostly juniors isn't a good place to learn - you'll suddenly be the most experienced person
- code reviews & pairing (make sure you're typing)
- speak up when you don't understand
- take notes
- play to your strengths, but work on the rest too
- set learning objectives, and track your progress
- side projects?
- be kind to yourself


## It's hard to be a junior
### so much to learn! help?
- how do I get people to want to help me?
- make it easy for people to help you
- realise that sometimes things are confusing because they're confusing
- make sure you tell people when they were helpful / you used their advice
- don't be afraid of making mistakes: it's not a matter of if you break production, but when, and building processes that make it easy to recover
- work out what your learning style is, and what your mentor's teaching style is
- talk about when it's ok to interrupt
- (as a senior, try to be in a position to listen in on junior conversations so you can jump in when they need help/clarification/direction)
- (as a senior, take responsibility for providing your junior with the space to learn and extra help if it looks like deadlines will be missed)
- narrow the scope: prioritise what to learn next with your mentor (timing matters)
- learning: do you need to be goal oriented or just playing?
- team processes and product knowledge are far more useful than general programming skills / tool optimisation
- have confidence in your ability to learn

### how can I be useful?
- your tech contributions matter: if you don't build it, nobody will
- don't compare your beginning with someone else's middle
- ask good questions:
  - are we working on the right thing
  - is there a reason we're doing it this way
  - don't put people on the defensive: you want to learn...
- give good feedback
  - in the right place, to the right person, at the right time
  - speak up when you have good things to say, too
  - if you don't have an opinion, it can be worth saying so too
- make your team look good to other teams
  - give awesome demos (prepare! why should your audience care?)
  - be responsive, thorough and empathetic

### pitfalls
- women often don't get credit for being helpful, because it's just expected of them
- make sure you don't get sidelined into the non-tech stuff instead of learning as a developer
- stay focussed on your goal  





### How to stay motivated
- Define Mastery Goals, Not Performance Ones, For Difficult Problems
http://www.effectiveengineer.com/blog/frame-your-goal-to-increase-motivation

### When informal onboarding stops working
http://www.effectiveengineer.com/blog/how-to-build-a-good-onboarding-process-for-new-hires-at-a-startup
https://www.quora.com/Quora-company/What-is-the-on-boarding-process-for-new-engineers-at-Quora-Is-there-any-such-process-at-all-Does-Quora-train-their-employees-in-a-fashion-similar-to-Facebook-or-are-they-asked-to-start-writing-code-straight-away
#### some risks of an ad-hoc, absent, or ill-defined onboarding process include:

Weeding out good people who might have been productive had they been given a little more guidance, which would be a shame given how much effort is typically spent on recruiting someone to a company.

Not identifying low performers or bad hires soon enough because there aren’t enough opportunities to evaluate their work or because you’re wondering that maybe you just need to give them a chance to ramp up before they become more productive.

Losing productive output from the new hire because ramping up takes longer than it should have.

Increased stress or reduced happiness of new hires, particularly those who might not have worked in startup-like environments before.

These risks increase as more people get hired, especially if your recruiting pipeline biases toward more inexperienced hires, like college grads in their first full-time job.

As a company, a product, and a codebase get larger, the surface area of things to explore increases, and it becomes more and more difficult for a new person, without any guidance, to figure out on their own what to learn first.

The onboarding process is an opportunity to direct the learning and the activity of a new hire toward what the team believes matters most. Designing a good onboarding process can increase the effectiveness of the rest of the new hire’s time.







### Next steps
https://thoughtbot.com/upcase/practice

http://www.effectiveengineer.com/blog/how-to-become-a-10x-engineer
https://medium.com/commit-push/10-ways-to-grow-as-a-developer-in-2017-5f96f890a306
https://labs.spotify.com/2016/02/08/technical-career-path/


### But what if I know I don't want to be a manager?
Here are some examples of how you might amplify your impact without going into management (and without co-founding a startup, which typically leads to management), based on software engineers I know:

You build tools and abstractions that multiply the output of the engineering teams around you. For example, Jeff Dean, through his contributions to Protocol Buffers, MapReduce, BigTable, Spanner, and other systems infrastructure, has increased the output of other engineers at Google by over an order of magnitude. It’s no wonder why Google created the engineering level of Senior Google Fellow essentially for him.
You develop sufficient expertise to consult on software or experiment designs from other engineering teams, and your feedback is valuable enough that it shaves days or weeks worth of work or it turns key projects from failures into successes.
You become an expert on a deep, technical field that is material to a growing company. For example, you become a machine learning expert and then work on news feed ranking at Facebook, ads ranking at Google, or search ranking at Airbnb. The projects you ship directly translate into growth and revenue for the company.
You identify a critical business opportunity, perhaps by working with the sales and business teams, and you become part of the founding team within the company to build out a product to address that need.
You build out onboarding and mentoring programs to teach and train other engineers, and you make them significantly more valuable members of the team.
You play a key role in building out a solid hiring process, and you help recruit and close engineering hires.
You make significant contributions to building the engineering brand for your company. For example, if diversity is a strong part of your engineering brand, you may move forward the state of diversity in hiring in the industry.
