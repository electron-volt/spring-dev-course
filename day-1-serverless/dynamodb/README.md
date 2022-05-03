---
description: Serverless, schemaless NoSQL database
---

# DynamoDB

## What is DynamoDB?

In AWS' own words,

* Amazon DynamoDB is a fully managed NoSQL database service that provides fast and predictable performance with seamless scalability.&#x20;
* DynamoDB lets you offload the administrative burdens of operating and scaling a distributed database so that you don't have to worry about hardware provisioning, setup and configuration, replication, software patching, or cluster scaling.

DynamoDB is the database of choice for most serverless architectures in AWS. It is a schemaless (non-relational) database that consists of key-value pairs.

#### Optional reading: For those with an SQL background

If you are used to SQL databases, then schemaless will at first seem really weird. We are accustomed to defining a schema for a table, e.g. a table of cars that all have a license plate, an ID, an owner, a make, age and so on. If there is a row in the table for a car that does not have an owner, then you have to put NULL in the owner field.&#x20;

In a schemaless table, you can just specify that all cars have an ID that is the primary key. That is it. That is the only mandatory attribute. You do not need to specify any other attributes when you create the table. You can add them later and they are all optional. Then some cars can have a license plate, some might not. Some cars have a colour, some don't. If your table has a thousand cars and 999 have an owner and one doesn't, you do not need to put NULL as the owner for the one lonely car - that item in the table will just not have an attribute called "owner".&#x20;

## DynamoDB core components

Before we are able to get started with our first lab, we have to introduce some core components of DynamoDB such as

* tables
* items
* attributes
* partition keys
* sort keys

### Tables, items and attributes

In DynamoDB, tables, items, and attributes are the core components that you work with. A _table_ is a collection of _items_, and each item is a collection of _attributes_.

#### **Tables**&#x20;

Similar to other database systems, DynamoDB stores data in tables. A _table_ is a collection of data.

#### Items&#x20;

Each table contains zero or more items. An _item_ is a group of attributes that is uniquely identifiable among all of the other items.&#x20;

#### **Attributes**

Each item is composed of one or more attributes. An _attribute_ is a fundamental data element, something that does not need to be broken down any further.

![Example of a People table with three items. PersonID is an attribute.](<../../.gitbook/assets/image (173).png>)

### Partition keys and sort keys

When you create a table, in addition to the table name, you must specify the primary key of the table. The primary key uniquely identifies each item in the table, so that no two items can have the same key.

DynamoDB supports two different kinds of primary keys:&#x20;

#### Partition key

A simple primary key, composed of one attribute known as the _partition key_. (E.g. PersonID)

#### **Partition key and sort key**&#x20;

Referred to as a _composite primary key_, this type of key is composed of two attributes. The first attribute is the _partition key_, and the second attribute is the _sort key_.

Now we have the information necessary to create our first table.&#x20;

## Additional information

If you want to learn more about DynamoDB, check out the DynamoDB guide:

{% embed url="https://www.dynamodbguide.com" %}
