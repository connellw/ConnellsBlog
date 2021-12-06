---
layout: post
title: Domain Events vs Integration Events
tags: ddd events
---

There are often different kinds of events in an event-driven system. They often represent the same thing that has happened, but are used in different ways.

# Domain Events

In **Domain-Driven Design** (DDD), domain events are the things that happen that are of interest to a domain expert. They are written in the ubiquitous language of the domain.

```c#
public class SomethingHappenedEvent : IDomainEvent
{
    // props
}
```

These are raised when methods are called making changes to domain **aggregates**. When domain events are dispatched, their handlers usually make more changes to other aggregates.

```c#
internal class SomethingHappenedEventHandler : IDomainEventHandler<SomethingHappenedEvent>
{
    // ctor

    public async Task Handle(SomethingHappenedEvent domainEvent)
    {
        var otherAggregate = _otherRepository.Get(domainEvent.SomeProperty);

        otherAggregate.DoAnotherThing();
    }
}
```

## Transaction Boundary

Whether or not domain events should be handled within the same data transaction is disagreed amongst architects. Some argue that the aggregate is the consistency boundary.

> Any rule that spans Aggregates will not be expected to be up-to-date at all times. Through event processing, batch processing, or other update mechanisms, other dependencies can be resolved within some specific time.
>
> -- <cite>Eric Evans</cite>

However, Jimmy Bogard proposes [dispatching domain events immediately before committing the transaction](https://lostechies.com/jimmybogard/2014/05/13/a-better-domain-events-pattern/), so the side-effects of handling the events are all committed together as one unit of work, enforcing immediate consistency. I like this approach myself. It's simpler to consider one request as one atomic transaction. From the outside, I send my service a request, all the work is done, the transaction is committed, the request has either been fulfilled or there was an error.

![CQRS command sequence-ish diagram](/images/diagrams/sequence-ish-command.png)

## MediatR Pipeline

When all requests behave this way, it's easy to define a single behaviour, writing this once and not violating the Single Responsibility Principle.

We can implement our own [decorator pattern](https://refactoring.guru/design-patterns/decorator) to ensure that events are dispatched when we commit the unit of work. Or, if we use [MediatR](https://github.com/jbogard/MediatR) to send the requests to our application, we can implement a **pipeline behavior**.

```c#
internal class CommitPipelineBehavior : IPipelineBehavior
{
    // ctor

    public async Task<TResponse> Handle(TRequest request, CancellationToken cancellationToken, RequestHandlerDelegate<TResponse> next)
    {
        await next();

        await _eventDispatcher.DispatchAll();

        await _unitOfWork.Commit();
    }
}
```

## In-memory

Events that are dispatched before the transaction do not themselves need to be persisted to a database. If they only exist in memory, we don't need to worry about serializing them, so we're not restricted on what data they can contain. For example, they could have recursive properties, or maybe even contain sensitive information.

# Integration Events

Committing all state changes within one transaction is not always possible, such as when changing some external state over an API call. What happens if the API responds successfully but then our own transaction fails to commit? We must ensure the state is kept consistent between the two systems.

In this case, we can change our own state, then ensure the external state change is done *after* our transaction has been committed.

To do this, Kamil Grzybek proposes [dispatching separate domain event notifications](http://www.kamilgrzybek.com/design/how-to-publish-and-handle-domain-events/) after the commit. This is a nice distinction and gives us the choice to handle inside or outside the transaction by handling the domain event or the domain event notification.

Because our database transaction has already been committed, we will need to **schedule a job** to ensure the notification is processed. Otherwise the notification handling could fail, or our application could crash, and our application could be left in an inconsistent state. We can implement an Outbox pattern, which writes the notification to a data table as part of the same transaction and processed it later.

One way of achieving this is by implementing a **generic domain event handler** for all domain events.

```c#
internal class DomainEventNotificationOutboxHandler<TEvent> : IDomainEventHandler<TEvent>
    where TEvent : IDomainEvent
{
    // ctor

    public async Task Handle(TEvent domainEvent)
    {
        _outbox.AddNotification(domainEvent);
    }
}
```

A separate background process would dispatch events from this outbox to the notification handlers. Exceptions in these handlers will not cancel the transaction, because it has already been committed. We will need a retry policy if these handlers fail. If they continuously fail, we must raise an alert for a human to deal with the problem.

## Eventual Consistency

The original request to commit the first transaction would return to the client before the above notification is processed.  From the client's point of view, their request was successful and all their changes *will* be reflected in the rest of the system, but maybe not immediately. The state changes will be propagated eventually through the handling of these notifications.

This appears to be the same concept as the [integration events defined in Microsoft's .NET Microservices Architecture e-book](https://docs.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/domain-events-design-implementation#domain-events-versus-integration-events):

> The purpose of integration events is to propagate committed transactions and updates to additional subsystems, whether they are other microservices, Bounded Contexts or even external applications. Hence, they should occur only if the entity is successfully persisted.

## Public Contracts

Domain events (and their notifications) are designed to be handled internally. However, integration events are often part of your public API contracts that clients are allowed to subscribe to.

Because they are public, they must be **versioned**. We can't make breaking changes to events that external systems are subscribed to.

These could be implemented by publishing events to subscribers over an Event Bus like RabbitMq, or some HTTP callback method like WebHooks or SignalR.

## Event Store

Another option is to allow clients to query for past events. We would need to persist each event to some storage.

```c#
internal class EventStoreHandler<TEvent> : IDomainEventHandler<TEvent>
    where TEvent : IDomainEvent
{
    // ctor

    public async Task Handle(TEvent domainEvent)
    {
        _eventStore.AddEvent(domainEvent);
    }
}
```

This could be the same persistence used for your notifications. Rather than removing notifications once they are handled, they could just be marked as published but remain in an event store for a period of time. This gives clients the option to query for events they may have missed or lost somehow.