---
layout: post
title: CQRS, DDD and Event Sourcing
tags: cqrs ddd events
notes:
  - mention that it's all about writing. Reads have no effect on transactions or events.
---

Three independent concepts that are often thought of together.

... ven diagram ?

# Domain-Driven Design (DDD)



# Command/Query Responsibility Segregation (CQRS)

CQRS splits the application into two subsystems: one for reading, one for writing.

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