Optimisation and Profiling
- ref. https://www.keycdn.com/blog/php-performance/


- performance != speed
  - speed / accuracy / scalability often tradeoff

- hardware and software choices need to be taken into account when determining performance parameters

- optimisation tips
  - native PHP functions over hand knitted code
    - http://php.net/manual/en/indexes.functions.php
  - JSON rather than XML
    - json_encode( ) and json_decode( )
    - if you must use XML, parse with regex rather than DOM manipulation
  - cacheing
    - Memcache for reducing database load
      - http://php.net/manual/en/book.memcache.php
    - APC, OPcache, eAccelerator for intermediate code i.e. storing precompiled script bytecode in memory so code will only be recompiled if it has changed since it was cached
      - http://php.net/manual/en/book.apc.php
      - http://php.net/manual/en/book.opcache.php
      - http://eaccelerator.net/
  - don't repeat calculations
    - store the value as a variable
  - use c
    - faster and simpler to determine if a value is greater than 0 than count( ), strlen( ) and sizeof( )
  - cut out unnecessary classes [TODO: I don't understand this]
    - only create classes and methods where you'll use them multiple times
    - and use derived class methods rather than methods in base classes for speed
  - turn off debugging
  - close database connections & un-set variables to save memory
  - use aggregate queries to minimise the number of hits to the database
    - up to 90% of execution time can be data access
    - use slow SQL logs to identify queries & then investigate their efficiency
  - consider which str function you're using
    - str_replace is faster than preg_replace
    - strtr is four times faster than str_replace
  - use single quotes unless you need to include variables
  - use === as the restricted comparison is quicker than ==
  - avoid code that can trigger a file stat, especially in a loop
    - file_exists(), filesize() or filetime()
  - make sure the filesystem isn't used for session storage [TODO: huh?]
  - profile your code if using OPCache etc doesn't give the improvements you need

- non-code bottlenecks to look out for
  - network capacity
  - CPU
  - shared memory
  - filesystem fragmentation
  - process management e.g. antivirus, mail servers, etc
  - other servers
  - external APIs
    - cache API output, or make API calls in the background
    - if possible, have a fallback to display without API response after a reasonable timeout

- logging gives valuable information, but can also slow things down

- keep an eye out for Just In Time compliation in PHP8
