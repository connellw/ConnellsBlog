---
layout: post
title: You Don't Need Logging Code
tags: aop logging decorator
---

- Link from Domain-Driven Boundaries
> I have made the mistake of letting the domain model be influenced by technical requirements: maintainability, performance, connectivity with other domains, or [monitoring concerns](/dont-need-logging-code.md).

- More drawing boxes
- Extra box for "stuff I want to log"
    - Just use the existing boxes.

Single Responsibility. Separation of Concerns.

The time consumed talking about it. What to log, when to log them, what level to log at.

Too much boilerplate.

Cross Cutting Concerns.

# Decorators

The Decorator pattern 

Custom decorators for classes
DI Framework Support.
- Autofac's RegisterDecorator
Make it configurable.

# Pipelines

Middlewares. PipelineBehaviors.
DelegatingHandler

# ILWeaving vs Type Interception

PostSharp. Scary?

- Aspectos github