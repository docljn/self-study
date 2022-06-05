# Relational DB’s - Making queries, performance, pros/cons

## Making Queries

Two basic options

- write SQL
  - can be more performant if you really know what you're doing
  - useful to know SQL even if you're using an ORM so you can debug N+1 and other issues
- use an Object Relational Mapper like ActiveRecord in Rails
  - you get to write in a single language you already know well and reduce context switching
  - the db is abstracted so switching from one RBDMS to another is easier
  - you probably get support for transactions, migrations, seeds etc as part of the library
  - modern ORMs are very good at optimising performance
  - BUT the initial setup can be a pain, and you need to learn the DSL

## Performance

An infinitely large, infinitely complex field. I've heard of large companies with multiple teams responsible purely for optimising the db interactions.

Design/data modelling is important: both a logical data model and a physical model describing the types of entities to be included in the db.

- transactional applications (mainly inserting and updating data) need normalised data to minimise multiple db updates for a single data change and maintain consistency
- business intelligence applications (mainly reading data) need attention to db load and likely query type
- data integration applications (a bit of both) needs to find a way to update information in a data warehouse or equivalent while retaining a historical record. Tricky!

How you approach this will depend on where the bottlenecks are (obviously)
A bear-trap is when the db index becomes too large to hold in memory: performance falls off a cliff
N+1 queries are a common problem, especially where you're using e.g. React components and a sub-component makes a query to the db. The more items, the more queries.

Indexing can speed things up, but it's important to benchmark as it doesn't always work that way, especially if it's a write-heavy application where updating the index with each write can be costly.
Too many indices is as bad (or worse) than too few: and trust your query optimiser if it doesn't use what should be an available index then you've probably got the wrong index for that query!
Database partitioning is worth investigating, but does add another layer of complexity.
CPU/Memory resources will help, up to a point, as will switching to SSD (though be aware that they can fail suddenly and without warning)
Keeping dependencies up to date is often forgotten. Newer versions are usually more performant, and have a better query optimiser (if you remember to use it!).

## Pros/Cons

I'm assuming this means the pros and cons of relational databases vs other types of database.

I'd like to learn about the difference between Graph Databases and Relational Databases (got a bit of knowledge of NoSQL but Graph is different again)

### Relational Db

- good for entities with clear links i.e. customer->address->order->invoice, and allows for updates to one entity without having to update all related entities
- designed to be ACID compliant i.e. consistent, but slower
- main characteristics
  - built from a set of unique tables (also called relations) which have rows and columns (very structured!)
  - a table contains data about just one entity
  - tables must have a primary key
  - tables are linked by primary and foreign keys
- not great for scaling (vertical is expensive)
- rigid schema makes queries/interpretation easier, but changes to data structure/types much more complex
- performance is impacted by the number, size and complexity of tables

### Non-relational db aka NoSql

- often used when large quantities of complex, diverse, unstructured data need to be organised
- good when handling rapidly changing data and/or data types
- not ACID: BASE instead (basically available, soft-state, eventually consistent)
- should be designed to avoid joins (there's no relation in the model so denormalised data is preferred)
- types
  - key/value store: super simple e.g. Memcached
  - document databases (what most people think of when you mention NoSQL): JSON, BSON or XML format data, with related data embedded together making data retrieval much faster. No rigid db table structure so easier to shape the data to match the system requirements (denormalised)
  - column-oriented database: most commonly used for analytics/aggregation etc but rarely ACID compliant
  - graph databases: uses nodes to hold data, and edges define relationships between them i.e the connections are stored as part of the db, where in a relational db the links are implied; often a secondary db for specific business purpose alongside a more traditional db (e.g. ecommerce live recommendations need links between user and all previous behaviour, fast)
  - hierarchical databases: represent data in a tree-like form. Each ‘parent’ node can have many ‘children.’ The node without a parent is considered the root node. e.g. org chart
