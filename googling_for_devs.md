# Googling for Developers

We use google / duckduckgo / whatever all the time, but most of us have no training in how to use a search engine effectively.

It's a lot easier than it used to be, with natural language search, but far too often you can end up feeling like you're going round in circles

## Sneaking up on the answer

- when you'll know what you're looking for when you find it...
- School assignment: write a two page report on "The climate of Krasnoyarsk and how it affects the plants, animals and people living there."
- eh?
- Start by simply pasting the assignment description into your search engine of choice
  - scan the results page without actually clicking into anything
  - if you're lucky, it will work, but usually the results are miles away from what you need

- Search for part of your query: `Krasnoyarsk climate`
  - again, scan for useful information
  - `Krasnoyarsk experiences a humid continental climate (Köppen climate classification Dfb)`
  - `Köppen Classification: Continental Subarctic Climate`

- Change your query to use the new key words you've found:
- `"humid continental climate"`
  - `"humid continental climate" AND "plants and animals" AND "adaptation"`
    - oops, too specific: I've got a page of science journals
  - `"humid continental climate" AND "plants and animals"`
    - that's better, but not as much as I'd hoped
  - `"humid continental climate" plants animals`
    - bingo: lots of links to general information
- `"Continental Subarctic Climate" plants animals`
  - not nearly as much information

- Now I'm worried because I don't know which description is correct
  - `map:climate zones russia`
  - `map:krasnoyarsk`
  - Huh. It looks like the region of Krasnoyarsk includes both of those climatic regions plus true Arctic Tundra as it stretches all the way up to the Northern coast of Siberia
  - better check if I'm supposed to be researching the city or the region!

## If the search engine is deliberately misunderstanding you

### Look at the "People also ask" and "Searches related to" sections on google

- this shows what google thinks you might also be looking for
- if it's way off, think about refining your query to exclude some of those keywords
- if it's close, click on one and see where it takes you, or add one of the keywords to your own search

### Try a different search engine

- duckduckgo is to be particularly good if you are searching on an error trace, taking you straight to the accepted stackoverflow answer (and doesn't steal your data)
- google has a much more extensive advanced search query syntax

### Use the google Advanced Search option

- <https://www.google.com/advanced_search>
- particularly useful is the ability to limit your search to a specific time period
  - you really don't want to be following a Rails tutorial written in 2012, or a React tutorial written in 2018!
- also great if you're looking for conference slides `filetype:pptx Railsconf`

### Build your own advanced query

#### Use quotes to be more (or less) specific

- `rails scopes` will return results with those words in any order
  - returns a whole page of rifle spares
- `"rails scopes"` will return precisely that phrase
  - still a lot of rifle parts but the first few results are all ruby on rails
- "ruby" will exclude synonyms, which google includes by default

#### Add operators and additional search words to refine (or expand) results

- `"rails scopes" ruby` increases the desired results
  - `+` doesn't make any difference as far as I can see, so I assume that's the default
  - the search is NOT case sensitive
- `"rails scopes" -rifle` or, indeed, `"rails scopes" NOT rifle` reduces the unwanted firearms, but `rails scopes not rifle` will actually return only gun parts
- use `OR` or `|` with caution as often two separate searches will be more productive than one combined search
  - `("rails scope" OR "rails scopes") ruby` gives general ruby tutorials, and not the specific rails scope(s) information we wanted
- wildcards `*` can be useful
  - less so now that google automatically includes synonyms and spelling variants
  - more so to make a phrase less specific `"some long error trace * with my code * specifics removed"`
- adding `"how to"` can increase the number of instructional results as opposed to documentation
- get specific comparisons `rspec vs minitest`

#### Search within, or excluding, a specific web domain

- `site:stackoverflow.com scaffolding ruby` will search within the specified site, getting around the generally useless internal search options for a lot of tutorial sites
- `css grid -site:w3schools.com` will search everywhere BUT the specified site
- `link:ruby-doc.org -site:ruby-doc.org` will find sites that link to the specified site, and exclude internal links
- generally, use the advanced search for this kind of thing
- particularly useful when you're trying to find a ruby gem and keep getting jewellery
  - `ruby gem money` vs. `money site:rubygems.org`

#### Search only in a specific part of the web page

- `allintitle:rails scopes` will return only results containing all the specified words in the page title
  - useful if you're trying to re-find that really useful article from three weeks ago
- generally, use the advanced search page for this kind of thing

## When nothing is working

If the first 5-88 links on the results page of your most recent search have already been visited (and you actually read them rather than just opening a background tab!), and you're no closer to finding a useful answer, you may be looking in the wrong places. It's easy to believe that 'just one more search' will uncover the perfect solution, but...

Stop, take a break, and think about other ways you could phrase your query

Ruby is full of aliases and synonyms for methods: perhaps searching using `chunk` instead of `group_by` or `slice` will get you what you want in terms of splitting an array into equally-sized pieces (spoiler, rails has `in_groups_of` but ruby doesn't)

Remember that videos can also hold useful information, and the closer to the time a technology was first introduced, the more beginner friendly they will be

Ask actual humans (!) - they may not know the answer, but they could point to you a more appropriate search query or site, or pair with you to clarify what the issue is, or...
