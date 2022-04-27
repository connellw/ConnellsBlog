---
layout: post
title: The Building Blocks of Domain-Driven Design
tags: ddd
---

When learning DDD, it's easy for us engineers to get caught up in all the technical jargon: "Aggregate Roots", "Domain Events", etc. Fundamentally though, Domain-Driven design is not about the technical details; it's about creating a model and language that the business experts share with the technical experts (the software engineers). It is, perhaps ironically, about *not* using technical jargon.

> The structure and language of the code should match that of the business domain.
> -- Eric Evans

However, putting that crucial point aside, Evans does define some types of objects that are commonly seen when implementing DDD, which Martin Fowler calls the [Evans Classification](https://martinfowler.com/bliki/EvansClassification.html). In this post, I'll explain these types and give some C# implementation examples.

## Domain Events

One of the key components of the domain model is the domain events. These are the things that happen in a domain that are of interest to a domain expert.

They usually represent a change to some kind of state.

There aren't many solid rules on how to implement them, but they tend to look like DTOs.

```c#
public class BlogWrittenEvent : IDomainEvent
{
    public string AuthorName { get; init; }

    public string Contents { get; init; }

    public IEnumerable<string> Tags { get; init; }

    public DateTime PublishedDate { get; init; }
}
```

Note the use of init-only properties. **Events are immutable**. They have already happened, which is why their names are written in past tense.

I've chosen to have them all inherit [an `IDomainEvent` interface here](https://github.com/connellw/Doodad/blob/master/src/Doodad/IDomainEvent.cs). This is helpful if I want to use a `ICollection<IDomainEvent>` later to store them in memory. It could also enforce conventional properties, such as an `Id`.

## Value Objects

Some domain concepts are defined by their value. Value Objects have **structural equality** with each other. That is, if they have all the same values on all the same members, they are considered equal.

```c#
public class BankDetails : ValueObject
{
    public BankDetails(string sortCode, string accountNumber)
    {
        SortCode = sortCode;
        AccountNumber = accountNumber;
    }

    public SortCode SortCode { get; }

    public AccountNumber AccountNumber { get; }

    protected override IEnumerable<object> GetEqualityComponents()
    {
        yield return SortCode;
        yield return AccountNumber;
    }
}
```

The `GetEqualityComponents` method is used by the equality comparison defined in [the base class](https://github.com/connellw/Doodad/blob/master/src/Doodad/ValueObject.cs) taken from Vladimir Khorikov's article, ["Value Object: a better implementation"](https://enterprisecraftsmanship.com/posts/value-object-better-implementation/).

They are also **immutable**. You can't mutate a string or an integer for the same reason -- they are just values.

But instead of init-only properties, I have used a constructor here as a standard, because we must ensure it is **impossible to construct an invalid value**. In this case the `SortCode` and `AccountNumber` types are also Value Objects which do their own validation.

```c#
public class SortCode : ValueObject
{
    public BankDetails(string value)
    {
        if(value.Length != 6)
            throw new ArgumentException(nameof(value), "A sort code must have 6 characters.");

        if(value.Any(c => !char.IsNumber(c)))
            throw new ArgumentException(nameof(value), "All digits of a sort code must be numerical.");
        
        Value = value;
    }

    public string Value { get; }

    protected override IEnumerable<object> GetEqualityComponents()
    {
        yield return Value;
    }
}
```

C# record types are quite a good fit for Value Objects, but [they're not perfect](https://enterprisecraftsmanship.com/posts/csharp-records-value-objects/). Perhaps in simple domains you could get away with using these instead.

## Entities

Other domain objects may have all the exact same values but would still not be considered equal. In practice, Entities have an `Id` property to identify them as unique from other entities.

Entities are mutable -- we can change their state. Therefore, they have a history, which we can represent through Domain Events.

```c#
public class Hair : Entity
{
    // ctor

    public Color Color { get; private set; }

    public Length Length { get; private set; }

    public void Cut(Length cutLength)
    {
        if(cutLength > Length)
            throw new ArgumentException(nameof(cutLength), "Cannot cut off more hair than there is.");

        Length -= cutLength;

        DomainEvents.Add(new HairCutEvent
        {
            Id = Id,
            CutLength = cutLength,
            NewLength = Length
        });
    }
}
```

We would only modify the entity through calling these methods. We can check that the action is valid, make the change, and then add the domain event to the `DomainEvents` collection which I have defined in [the `Entity` base class](https://github.com/connellw/Doodad/blob/master/src/Doodad/Entity.cs).

The base class also includes a standard `Id` property which all entities must have.

Entities are usually loaded from a database, so must have a constructor that can populate all properties. You'll usually need to create fresh new entities too, so you would either need a second constructor, or you could use a factory class or method to create fresh new entities in the default state.

```c#
public class Hair : Entity
{
    // ctor to be used when loaded from database
    public Hair(Color color, Length length)
    {
        Color = color;
        Length = length;
    }

    // factory method for application to create fresh entity
    public Hair Create(Color color)
    {
        return new Hair(color, Length.Zero);
    }
    
    // props and methods
}
```

## Aggregates

Aggregates are clusters of entities and value objects that are thought of as a single unit. They provide a consistency boundary. They should be persisted together as a whole.

One of the entities in the aggregate is the **aggregate root**. All other objects in the aggregate must be accessed from the outside through this root entity.

The root can therefore ensure the state of the aggregate is valid.

```c#
public class Car : Entity, IAggregateRoot
{
    // ctor
    
    public void Go()
    {
        HandBreak.TakeOff();
        GearBox.ShiftTo(Gear.First);
        SteeringWheel.Hold();
        AcceleratorPedal.PushDown();
    }
}
```

I have chosen to use [an `IAggregateRoot` marker interface](https://github.com/connellw/Doodad/blob/master/src/Doodad/IAggregateRoot.cs) here. The interface itself does not define any members. You could use an `AggregateRoot` base class which itself derives from `Entity` to achieve the same thing.

## Domain Service

When we need to make changes across many aggregates, a service class can be used. These classes can contain business logic that doesn't naturally fit in other domain objects.

There doesn't need to be any base type for these. They're just normal classes that encapsulate domain logic that can be used by your application.

## Repositories

We don't store individual entities, we write the entire aggregates using repositories.

```c#
public interface IRepository<T>
    where T : IAggregateRoot
{
    Task<T> Get(Guid id);

    Task Add(T aggregate);

    Task Update(T aggregate);

    Task Delete(T aggregate);
}
```

This is just an interface. The implementations would be in some persistence layer outside of the domain. This is just an abstraction for some way of storing aggregates.

It doesn't have to be a generic interface -- you may want to define a subtly different repository interface for each concrete aggregate.

You may not need an `Update` method if you have some kind of change tracking, such as if you're using Entity Framework.


## Unit Of Work

Perhaps controversially, we could also define an abstraction for a data transaction. We could use our repositories to do work such as add new aggregates, modify them, delete them, but none of that work actually happens until we commit the work, so it is all persisted as one atomic operation.

```c#
public interface IUnitOfWork
{
    Task Commit();
}
```

The Evans book says that aggregates should define the boundary for a transaction, but with an `IUnitOfWork` interface we can make changes across multiple aggregates and commit the changes together. Architects dispute whether this is a good thing or not.

This could be handy for bulk changes done in domain services that must be consistent with each other. Although some might argue that if this is required then the aggregate boundaries should be reconsidered.

If you're [dispatching domain events within the same transaction](https://lostechies.com/jimmybogard/2014/05/13/a-better-domain-events-pattern/
), your domain event handlers can make changes to other aggregates and you can avoid dealing with eventual consistency or problems caused by an error in the event handler.