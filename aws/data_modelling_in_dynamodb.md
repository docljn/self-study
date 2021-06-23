# Data Modelling in DynamoDB

Alex DeBrie gave a presentation at AWS re:Invent 2019: Data modeling with Amazon DynamoDB (CMY304)

[https://www.youtube.com/watch?v=DIQVJqiSUkE](https://www.youtube.com/watch?v=DIQVJqiSUkE)

This page is a summary of the information provided in that presentation.

# Background

## What is DynamoDB

- no sql database fully managed by aws
- auth is via HTTPS using IAM (not tcp) so stateless authorisation
- single millisecond latency regardless of scale

## Why use DynamoDB

- hyperscale: when the volume and throughput of data would overwhelm a standard sql database
- hyper-ephemeral compute: when demand is spiky and your system needs to scale rapidly up and down with demand (cf. Global Request Router)

## Definitions

### Table

- all data in dynamodb is stored in a table
- best practice is to use a single table to store multiple different entity types

### Item

- each record in a DynamoDB table is referred to as an item

### Attributes

- items do not need to have a pre-defined structure, unlike a row in a relational database
- attributes can vary between items allowing for very flexible data storage

### Primary Key

- a unique identifier, chosen by the database designer, and required on each item in a DynamoDB table
- all data access is driven off the primary key so correct design is critical to database performance
- the primary key does not need to be the same format between different item types
- the primary key **must** contain a partition key and **may** contain a sort key
    - **simple primary key** contains only a partition key
    - **composite primary key** is made up of a partition key and sort key
- partition key === hash_key in terraform
- sort key === range_key in terraform

### Secondary Indexes

- there are two types Global Secondary Index (GSI) and Local Secondary Index (LSI) with most use-cases requiring a GSI
- secondary indexes allow for more data access patterns than those optimised for in the primary key
- a secondary index results in automatic data replication (conceptually similar to a database view)

### Sparse Index

- an index which only contains items which contain the specified attribute(s)
- particularly useful to allow filtering on an uncommon attribute as it greatly reduces the number of items in an index to be queried

### API Actions

- **item-based** actions (write, update, delete)
    - act on single items or batches of items
    - always require provision of the full item primary key
    - there is no equivalent to "DELETE * FROM" as this would involve queries across table partitions
- **query** actions
    - fetch multiple items in a single request
    - partition key is required
    - sort key is optional
    - **You cannot query across partitions**
- **scan** operation
    - should be avoided as it is both slow and expensive

# **The Basics of Data Modelling for DynamoDB**

- start with an entity relation diagram
- define your data access patterns—all the ways in which you will
use/fetch/manipulate or otherwise interact with the data stored in the
table
- design a Primary Key and Global Secondary Index(es) to handle those access patterns
- remember that you are NOT normalising data—you are pre-aggregating data and
denormalising it so that it is stored in the shape in which you want to
retrieve it

## Modelling for Querying Data

*Start by defining your access needs and patterns.* Knowing how the various entity types interact (1 to 1, 1 to many, etc) as well as how you will be accessing/querying the data is critical to getting the database built correctly. 

The  speaker recommends using "PK" and "SK" as the generic names for the Primary/Partition and Sort Keys to allow for different entity types to be stored in the same table.

The speaker recommends using prefixes in the PK and SK which show the entity type as this reduces the risk of collisions and simplifies both debugging and data retrieval.

Each entity should follow a specific PK and SK pattern.

As 1 -> many is the trickiest of data models to get right, the speaker went through several different options

### One to Many Data Model Examples

1. No need for direct access and a limited number of variations e.g. a User has many Addresses but you do not need to find users based on their address
    - add an attribute "addresses" onto each item and use a map/list/other data structure to store the data
2. Where there is a need for direct access, and there is no limit on the number of variations e.g. a User has many Orders and you need to retrieve all orders for a given user
    - create a Composite Primary Key i.e. Partition Key + Sort Key
        - Partition Key: USER#user_name
        - Sort Key: ORDER#order_id
    - this means that all orders for a single user are in the same partition
    - a query for all items in that partition with a Sort Key beginning with ORDER# will return all orders for that user
3. Where there is a need for direct access, and there is no limit on the number of variations, but the 1 of the 1 to many is the object of a 1 to many relationship itself e.g. an Order has many OrderItems and you need to retrieve all OrderItems for a single Order
    - You cannot use a composite primary key in this situation as Order is in a many to one relationship with User
        - Partition Key: ITEM#item_id
        - Sort Key: ORDER#order_id
    - This means that Orders and their OrderItems have the same SK
    - to be able to access all items in a single order you need an 'inverted
    index' where the PK and SK are reversed compared to the original
    table
    - this results in all OrderItems for a single order being in the same partition, and querying by ORDER#order_id will return both Order and OrderItems

## Modelling for Filtering Data

Forget what you know from sql. A "filter expression" does exist in the context of DynamoDB but it reads the entire table into memory in 1MB chunks before filtering out the results which are excluded. That is going to cost you in both time and money

S*tart by defining your access needs and patterns.*  

Every query must include a partition key and may include a sort key.  **You cannot query across partitions.**

Remember that not all filtering will be for user-facing needs e.g. filtering all "open" orders so that warehouse staff could pick items to fulfil an order regardless of the user who placed it.

### One to Many Data Model Examples

1. filter to get all orders placed by a given user
    - these are in the same partition as the Order Composite PK has a Partition Key of USER#user_id and any order-type entities can be selected by using the prefix #ORDER
2. filter to get all orders by order status for a single user
    - **you cannot query DynamoDB by attribute** so even though "status" is an attribute on an order you need a new Secondary Index
    - solve using a **Composite Sort Key**
        - create a new attribute on all orders OrderStatusDate in the format  STATUS_TYPE#created_at_date
        - create a Global Secondary Index using the value of that new attribute
            - Partition Key: USER#user_name
            - Sort Key: OrderStatusDate
    - filter items in that partition by sort key prefix which will be e.g. OPEN, SHIPPED etc to return all orders for a given user with a specific status
    - this can also be used for date-based filtering of orders for one user with a specific status
3. filter to get all open orders for warehouse staff
    - this is a tricky query type for dynamodb as it is, by definition, across partitions and thus global
    - create a **Sparse Index** which only replicates the relevant data
        - When an order is PLACED an attribute placed_id is created, and when it has been PICKED that attribute is deleted
        - Partition Key: placed_id (no sort key in this index)