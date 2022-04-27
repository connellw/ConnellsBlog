---
layout: post
title: DDD & CQRS Learning Resources
tags: cqrs ddd events architecture
---

A few people have asked for online resources where I have learned about DDD and CQRS.

This post is a bit of a link dump.

The big one is the whole [Tackling Business Complexity in a Microservice with DDD and CQRS Patterns](https://docs.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/) section of this Microsoft e-book and the example [Ordering Service in eShopOnContainers repository](https://github.com/dotnet-architecture/eShopOnContainers/tree/dev/src/Services/Ordering).

Then Herberto Graca's [DDD, Hexagonal, Onion, Clean, CQRS, … How I put it all together](https://herbertograca.com/2017/11/16/explicit-architecture-01-ddd-hexagonal-onion-clean-cqrs-how-i-put-it-all-together/) adds the Onion/Clean Architecture.

Some sample repositories that use this architecture are [Jayson Taylor's CleanArchitecture repo](https://github.com/jasontaylordev/CleanArchitecture) and [TaskoMask](https://github.com/hamed-shirbandi/TaskoMask).

For DDD base type implementations, Vladimir Khorikov's [entity implementation](https://enterprisecraftsmanship.com/posts/entity-base-class/) and [a better Value Object implementration](https://enterprisecraftsmanship.com/posts/value-object-better-implementation/). In fact, I learned a lot from many of the posts on Vladimir Khorikov's blog. You can also check out [my own implementations](https://github.com/connellw/Doodad/tree/master/src/Doodad) too.

Regarding events and integration events, Kamil Grzybek's [Handling Domain Events: Missing Part](http://www.kamilgrzybek.com/design/handling-domain-events-missing-part/) and the post that it follows have been useful.

For higher-level domain architecture, I refer to [Microsoft's e-book again](https://docs.microsoft.com/en-us/dotnet/architecture/microservices/architect-microservice-container-applications/identify-microservice-domain-model-boundaries), [Context Mapping](https://www.infoq.com/articles/ddd-contextmapping/) and then a guide comparing a [Shared Kernel, Conformist & Anti-Corruption Layer](https://inside.getyourguide.com/blog/2019/11/18/tackling-business-complexity-with-strategic-domain-driven-design).

The following blog posts have ended up in my bookmarks too:
- [A Brief Intro to Clean Architecture, Clean DDD, and CQRS](https://medium.com/software-alchemy/a-brief-intro-to-clean-architecture-clean-ddd-and-cqrs-23243c3f31b3)
- [Event-Sourcing as a DDD pattern](https://medium.com/swlh/event-sourcing-as-a-ddd-pattern-fea6de35fcca)
- [CQRS: What? Why? How?](https://sderosiaux.medium.com/cqrs-what-why-how-945543482313)
- [Commands and Queries are Holistic Abstractions](http://scrapbook.qujck.com/commands-and-queries-are-holistic-abstractions/)
- [Read-models as a Tactical Pattern in Domain-Driven Design (DDD)](http://gorodinski.com/blog/2012/04/25/read-models-as-a-tactical-pattern-in-domain-driven-design-ddd/)
- [Busting some CQRS myths](https://lostechies.com/jimmybogard/2012/08/22/busting-some-cqrs-myths/)
- [Are You Making These 10 DDD Mistakes?
](https://danielwhittaker.me/2020/01/22/are-you-making-these-10-ddd-mistakes/)