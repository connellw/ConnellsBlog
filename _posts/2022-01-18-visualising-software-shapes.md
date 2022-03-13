---
layout: post
title: Visualising Software in Shapes
tags: architecture
---

When I think of a software system, I see shapes and lines connecting groups of smaller shapes and smaller lines. Like a galaxy of solar systems with planets of continents, each with roads connecting cities with villages and hamlets. It's a structure that fits related concepts together and hides away complexity at different scales.

Sometimes I wonder if everyone views systems like this. We [whiteboard boxes and arrows](/drawing-boxes) to explain the shapes of these structures, creating these visual cues that help imagine how concepts fit together and where certain code might live within a solution.

We continue to build our systems based off the diagrams. Whilst each box might not perfectly match up to a class or class library, the structure, the concepts and ideas and how they relate -- that's what's important.

# Layered Architecture

Many of us have worked with N-Tier or Layered solutions before. Traditionally you might find a **user interface** that depends on a **business logic layer** (BLL), which itself depends on a **data access layer** (DAL).

![N-tier vertical diagram](/images/diagrams/n-tier-vertical.svg)

# Hexagonal Architecture

In 2005, Alistair Cockburn suggested putting the logic at the bottom of the dependency hierarchy. He called it the [Hexagonal Architecture](https://alistair.cockburn.us/hexagonal-architecture/).

There is nothing special about the number six here, it is just a nice way to visualise the architecture. It's diagrams tend to use a hexagon in the middle, but the shape doesn't matter; it can just as easily be drawn as a circle.

![N-tier vertical diagram](/images/diagrams/hexagonal.svg)

He introduced the **Ports and Adapters** pattern, which later became another name for the architecture itself. A **port** is, like ports on electrical gadgets, an interface, where anything that "fits the mechanical and electrical protocols can be plugged in." An **adapter** is the [Gang of Four adapter design pattern](https://refactoring.guru/design-patterns/adapter) that implements one interface (the port) and encapsulates another.

Adapters are divided into two flavours by the flow of control. Those that make calls into the application core are called **primary** or **driving** adapters, which tend to be things in the user interface or presentation layer, the same as in an N-Tier architecture.

On the other side, **secondary** or **driven** adapters have inverted control. They are *called by* the business logic. This idea is what enables the inversion of dependency. The business logic defines an interface (a port) that the infrastructure layers implement.

# Onion Architecture

[Jeffrey Palermo proposed the Onion Architecture](https://jeffreypalermo.com/2008/07/the-onion-architecture-part-1/), which shares the same premise as the Hexagonal Architecture.

> Hexagonal architecture and Onion Architecture share the following premise:  Externalize infrastructure and write adapter code so that the infrastructure does not become tightly coupled.

However, the core is further divided into other layers and the dependencies point towards the center, where a domain model lives at the core of the architecture.

![Onion architecture diagram](/images/diagrams/onion-circular.svg)

> The fundamental rule is that all code can depend on layers more central, but code cannot depend on layers further out from the core.  In other words, all coupling is toward the center.

## Castle Architecture

None of these diagrams have to be circular. These are just visual cues that help build a picture in our minds. To prove this, I present to you the Castle Architecture.

![Castle architecture diagram](/images/diagrams/castle-architecture.svg)

It's not limited to 3 vertical layers, and you can draw as many [merlons](https://en.wikipedia.org/wiki/Merlon) as you like on the top.

The code would be exactly the same as Onion Architecture. The projects and their dependencies are identical. The only difference is the visual resemblance to a medieval fort instead of a vegetable that makes you cry.

## Clean Architecture

[Uncle Bob Martin's Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) brings a number of different architectures together under one roof. Using the same Dependency Rule as Hexagonal and Onion, dependencies go inwards.

The names of the objects in the layers are more refined, explaining how far from the core certain ideas should be.

It adds an outermost layer contains the Frameworks and Drivers, where not much actual code lives, which is distinct from the Interface Adapters in the layer below where the Controllers and Repositories live.

Additionally, a flow of control is defined, from Controllers, through Interactors, to Presenters.

## Explicit Architecture

They're all quite generic approaches, providing a template that could work for different languages and frameworks. [Herberto Graca's Explicit Architecture](https://herbertograca.com/2017/11/16/explicit-architecture-01-ddd-hexagonal-onion-clean-cqrs-how-i-put-it-all-together/) is a much more detailed view, bringing DDD and CQRS into the equation, showing where Commands and Queries belong, giving more examples of adapters. [The diagram](https://docs.google.com/drawings/d/1E_hx5B4czRVFVhGJbrbPDlb_JFxJC8fYB86OMzZuAhg/edit) is large, but beautiful.

# Olympic Rings

I had a go myself once. About 3 years ago, my default project structure for .NET solutions contained 5 projects (not including tests).

![Olympic Rings diagram](/images/diagrams/olympic-rings.svg)

Dependencies go downwards. The flow of control goes from left to right.

I thought I was being very clever. But if you combine the Domain (entites) and Persistence rings, and also the Logic and Contracts, then you just have an N-tier architecture. If you combine the Logic, Contracts and Domain all together, you just have Ports and Adapters.

It's kind of nice to really enforce structure. But all I'd really done was move some project internals into other libraries.

# Vertical Slice Architecture

Jimmy Bogard then throws all that out the window and proposes splitting the logic into [Vertical Slices](https://jimmybogard.com/vertical-slice-architecture/) instead.

![Vertical slice diagram](/images/diagrams/vertical-slice.svg)

This architecture gets rid of "layers" entirely and divides the code by requests instead. Database code can be coupled with the logic at first, but slices are decoupled from each other. There's no need for generalised abstractions (e.g. a controller must send commands which load aggregates).

This architecture is iterative and agile. It depends fundamentally on refactoring as the code grows and **pushing logic down** to the domain.