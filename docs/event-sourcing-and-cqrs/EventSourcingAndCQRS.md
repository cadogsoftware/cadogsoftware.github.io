# Event Sourcing and CQRS

## Introduction
This article explores two design patterns that we have used extensively in the past, Event Sourcing and the Command Query Responsibility Segregation (CQRS) pattern. Benefits of each pattern are given and suggestions of when to use each one.

## What is Event Sourcing
Event Sourcing is a software pattern in which every change to the state of your application is captured. These state changes are called 'events'. For example, in a 'user' domain, you may have an event that records the creation of a user and multiple events that capture the changes performed on a user (such as changing their phone number). To get the current state of the user you have to 'aggregate' all of the user events in chronological order.

## What is CQRS
We first came across CQRS when reading articles by Martin Fowler and Greg Young. They say that at its heart, CQRS has the notion that you can use a different model to update information than the model you use to read information.
This is different to the traditional Create, Read, Update, Delete (CRUD) model that most people are familiar with, that has a single model object to carry out all operations.
A typical implementation of CQRS is to have two Microservices for each of your domains, one for updates and another for reads.

## A typical implementation
In one of our previous roles we developed a 'write' Microservice for each domain and used the Event Store product (https://www.eventstore.com/) to store our events. We also had 'projections' listening out for the events that were added to our Event Store. These projections were Microservices that copied some or all of the data from the events into a Mongo DB Read Model, which was then available to query by a separate read only Microservice. Using this model, each domain consisted of a Read Microservice, a Write Microservice, one or more Projections and once or more Read Models, along with the events stored in Event Store.
As you can see, there were quite a lot of services to develop here, so this something to bear in mind when deciding if Event Sourcing and CQRS is good for you.

## Benefits of Event Sourcing and CQRS

### Performance
The main benefit of CQRS is in handling high performance applications. As your reads are separate to your writes, you can scale each independently and a high write rate will not affect your read rate and vice versa.

### An Audit Trail out of the box
Event Sourcing means that you store every change to your data as an immutable event, so this can provide a valuable audit trail. This is not something that the CRUD model provides, unless you implement it yourself.

## Drawbacks of Event Sourcing and CQRS

### Many Services
As you can see from the typical implementation above, there are several services needed for each domain if you want to implement these patterns. Whilst these allow for independent scaling, they obviously need to be developed, maintained and monitored too.

### Eventual Consistency
As the data in your read model(s) is not updated straight away, it may be momentarily out of sync with your write model. This is called 'Eventual Consistency'. Whilst the time that the data is not consistent is usually very small, this needs to be taken into account when designing your system. If for example, a write is done by a user clicking a button on a web page and then a read is done almost immediately to display data on the next web page then you may get old data. Of course, there are ways around this (such as caching the data) but some thought needs to be put into this sort of data flow in your system.

### Hard to delete data
Whilst you may think that if data is hard to delete then this is a good thing, it can cause problems if you are, for example, required to delete all of the data that you hold for an individual. The General Data Protection Regulation (GDPR) may need you to do this, so you will need a strategy to do this.

### Monitoring
As with any distributed architecture involving lots of Microservices, you will need to have a good way of monitoring and tracking requests that travel through your system, including your Event Store.

## When to use Event Sourcing and CQRS
The two main reasons to use these patterns are:
 - When you need to separate the performance of reads and writes of your system.
 - When you need an audit log of all changes that have happened to your application state.

## When not to use Event Sourcing and CQRS
***In our view, Event Sourcing and CQRS adds significant complexity to your system.*** Unless you really need to separate your reads and writes for performance reasons then we would say not to use it.
Another option, if you need an audit trail for all of the changes within your system would be to consider writing your events to an event store as well writing your data to your traditional CRUD store. However, you should make sure that this does not affect the performance of your system that is visible to end users, perhaps by firing off asynchronous messages that in turn update your event store. In this way, your event store is acting as an audit trail and you can add projections if you need to create read models for things like reporting of management information.

## Conclusion
Event Souring and CQRS is a good way of separating your reads and writes for performance reasons. However, it does involve a significant mind shift from your traditional way of working and not every development team will be comfortable with it. Eventual Consistency can also cause issues and more Microservices than usual are required. For these reasons, we think that you should be very cautious about using these patterns.

## Useful information
Martin Fowler's explanation of CQRS: https://martinfowler.com/bliki/CQRS.html

Martin Fowler's explanation of Event Sourcing: https://martinfowler.com/eaaDev/EventSourcing.html

Greg Young's summary: http://codebetter.com/gregyoung/2010/02/16/cqrs-task-based-uis-event-sourcing-agh/
