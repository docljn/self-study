# Ruby Interpreter Bug
Alyssa Ross (Dev Platform : FreeAgent)

- [https:://github.com/alyssais/ruby-fiber-talk](https:://github.com/alyssais/ruby-fiber-talk)

- website developed with `Middleman`
- static site generator
- i.e. stuff only gets done once so why crashing?

- cause: different version of Ruby (newer!) but works on LTS (2 months left)
- replication:
  `ab` -> apache bench
  `ab -n 1000 c 10 http://localhost:4567`
  - make 1000 requests, 10 at a time, to this address
- testing
  - `middleman` simple website didn't error
  - `freeagent` large, complex website using `middleman` did error
  - delete files one at a time until the error stops
    - then delete code one line at a time in the offending file
    - one four-line helper function using FastImage
  - repeat process with `FastImage` code in a simple script
    - 500 threads: error...
  - start deleting code
    - find the error: similar script with 500.times do Thread.new {}
    - `Fibre`
      - a block of code that can stop itself and then be started by outside code
      - generator/coroutine in other languages
  - now test: solaris, macOS, but not linux

## TODO: I need to find out about this
`git bisect run sh -c 'script'` : fail === bad, pass === good
- gotcha : turning on thread cache causes issue
- so, turn it on before testing each time
  - ruby maintainers use subversion!
- found where the issue was
- revert just that one change
- no errors
- **now you can write a good bug report**

## Dev Platform
- your customers are devs so you don't have to work nights to fix things
