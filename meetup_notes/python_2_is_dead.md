# Python 2 is dead
@rebkwok
Becky Smith at ecometrica (mapping company)
slides: https://twitter.com/rebkwok/status/1022238874054127616

## Why Upgrade?
- python 2.7 support ends Jan 2020
- pandas, numpy, django, scipy all dropping python 2
+ https://snarky.ca/why-python-3-exists (unicode issue: string overloaded in python 2)
+ better iterations & memory use
+ restriction on comparators
+ advanced unpacking: assign part of an iterator to a variable
+ new dictionaries
+ keyword only arguments: robust API
+ f-strings: string interpolation with functions etc, runtime execution so faster
+ asyncio

## Dependency Checking
- pip install caniusepython3
  - relies on projects being classified in pypi
  - can check files, packages, etc.
  - wait until the list of blocking dependencies is manageable
  - check that you actually use those dependencies!
- 2to3
  - turns python2 into python3 via source code
  - gives a diff of changes to make python3 compatible
- pylint --py3k filename.py
  - picks up things which 2to3 misses


## making the change
- virtualenv to control python versions
- find/read guidance on differences between 2 and 3: python3porting.com ; python-future.org cheatsheet
- make sure test coverage is decent before you make the change
- working with django helped: project is broken up into separate apps
- run 2to3 to interactively change code, fix tests, commit app by app
- run django to find broken things that the tests didn't cover
- review and refactor app by app
  - 2to3 can add things you don't need e.g. extra brackets - review and remove
  - simplify and correct each section of code
- run pylint --py3k to interactively change code, fix tests, commit app by app
- extgensive user testing especially expert users who know what they expect from a given data set
- oops: a missed dependency (don't just check the requirements files, look at deployment process)

## gotchas
- rounding: python2 always rounds away from zero, python3 uses bankers rounding 2.5->2, 3.5->4
- exceptions: Exception.message is gone - use str(e) and 2to3 doesn't find it
- hash(): randomisation is on by default i.e. a different value each time you start python, same result if in same process: bombed use as cache keys
- pickle: just redo it from scratch, new pickle protocols (serialize and deserialize)
- comparisons/sorting: python2 would compare incompatible types particularly 'None'
- unicode regex: accents are now classed as alphanumeric

## lessons
- smoother than expected
- try and make your existing code as compatible as possible with the new version before you start
- be familiar with changes
- tools are only useful up to a point: review and verify
- tests are your friend
- check all dependencies, especially those involved at deployment time
- be prepared to upgrade 3rd party libraries

## persuasion
- keep bringing it up
- hope for developer time
