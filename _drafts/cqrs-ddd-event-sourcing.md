---
layout: post
title: CQRS, DDD and Event Sourcing
tags: cqrs ddd events
notes:
  - mention that it's all about writing. Reads have no effect on transactions or events.
---

Three independent concepts that are often thought of together. Why are these commonly spoke about together? What do these ideas share?

... ven diagram ?

# Domain-Driven Design (DDD)



# Command/Query Responsibility Segregation (CQRS)

CQRS splits the application into two subsystems: one for reading, one for writing. This thinking moves away from CRUD, where we would read the same resource that we have written to a database.

- For reads, think about the view model. What data do we need to display?
- For writes, think about the task at hand. What data do we need to perform this operation?

## Task-based Non-technieness

I think CQRS is the natural way of thinking about many real-world problems, but engineers have been conditioned to think in terms of resources and CRUD. In most domains, the non-technical domain experts describe their problems in terms of tasks, not in terms of creating and updating these models.

This is where DDD naturally fits with CQRS commands. The task-based, non-technical design. The command names can inherit the ubiquitous language of the domain and sometimes map one-to-one to a method on a domain entity. Some architects argue that commands themselves should be part of the domain.

- diagram

But not *all* systems suit this design. A ticketing system, for example, might naturally have a more CRUD-like design, where a ticket resource is created. Its description, assignee, and other properties are displayed on screen, and those same properties are updated with a form-like UI.

## Transaction and Consistency Boundary

DDD aggregates also provide a consistency boundary. I like CQRS commands to be atomic, committing a unit of work at the end of a command. This thinking fits together pretty well, especially if you are only making one operation on an aggregate per command.



- Operational data vs reporting data.
- Events used for further operations

- Commands that return acceptance not fulfillment ?

# Event Sourcing

- Internal Read Store = different tables, within transaction, consistent

- External read store = different databases, outside transaction, resilient, eventually consistent

## Eventual Consistency

- Commands that return acceptance not fulfillment

## Event Stream

- Event store
- Get. Reporting data?
- Generic domain event handler

# Conclusion

But they're not the same ideas. All these can be used independently, or you can mix and match whichever suit the needs of your project.