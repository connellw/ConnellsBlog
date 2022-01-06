---
layout: post
title: Visualising Software in Shapes
tags: architecture
---

- Note that these layers are not necessarily csproj.

When I think of a software system, I see shapes and lines connecting groups of smaller shapes and smaller lines. Like a galaxy of solar systems with planets of continents, each with roads connecting cities with villages and hamlets. It's a structure that fits related concepts together and hides away complexity at different scales.

Sometimes I wonder if everyone views systems like this. We [whiteboard boxes and arrows](/drawing-boxes) to explain the shapes of these structures, creating these visual cues that help imagine how concepts fit together and where certain code might live within a solution.

# Layered Architecture

Many of us have worked with N-Tier or Layered solutions before. Traditionally you might find a **user interface** that depends on a **business logic layer** (BLL), which itself depends on a **data access layer** (DAL).

- diagram



# Hexagonal Architecture

In 2005, Alistair Cockburn suggested putting the logic at the bottom of the dependency hierarchy. He called it the [Hexagonal Architecture](https://alistair.cockburn.us/hexagonal-architecture/).

There is nothing special about the number six here, it is just a nice way to visualise the architecture. It's diagrams tend to use a hexagon in the middle, but the shape doesn't matter; it can just as easily be drawn as a circle.

He introduced the **Ports and Adapters** pattern, which later became another name for the architecture itself. A **port** is, like ports on electrical gadgets, an interface, where anything that "fits the mechanical and electrical protocols can be plugged in." An **adapter** is the [Gang of Four adapter design pattern](https://refactoring.guru/design-patterns/adapter) that implements one interface (the port) and encapsulates another.

Adapters are divided into two flavours by the flow of control. Those that make calls into the application core are called **primary** or **driving** adapters, which tend to be things in the user interface or presentation layer, the same as in an N-Tier architecture.

On the other side, **secondary** or **driven** adapters have inverted control. They are *called by* the business logic. This idea is what enables the inversion of dependency. The business logic defines an interface (a port) that the infrastructure layers implement.

# Onion Architecture

[Jeffrey Palermo proposed the Onion Architecture](https://jeffreypalermo.com/2008/07/the-onion-architecture-part-1/), which shares the same premise as the Hexagonal Architecture.

> Hexagonal architecture and Onion Architecture share the following premise:  Externalize infrastructure and write adapter code so that the infrastructure does not become tightly coupled.

However, the core is further divided into other layers and the dependencies point towards the center.

> The fundamental rule is that all code can depend on layers more central, but code cannot depend on layers further out from the core.  In other words, all coupling is toward the center.



## Castle Architecture

None of these diagrams have to be circular. These are just visual cues that help build a picture in our minds. To prove this, I invented the Castle Architecture.

- diagram

It's not limited to 3 vertical layers, and you can draw as many [merlons](https://en.wikipedia.org/wiki/Merlon) as you like on the top.

The code would be exactly the same as Onion Architecture. The projects and their dependencies are identical. The only difference is the visual resemblance to a medieval fort instead of a vegetable that makes you cry.

## Clean Architecture

[Uncle Bob Martin's Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) brings a number of different architectures together under one roof. Using the same Dependency Rule as Hexagonal and Onion, 

## Explicit Architecture

They're all quite generic approaches, providing a template that could work for different languages and frameworks. [Herberto Graca's Explicit Architecture](https://herbertograca.com/2017/11/16/explicit-architecture-01-ddd-hexagonal-onion-clean-cqrs-how-i-put-it-all-together/) is a much more detailed view, bringing DDD and CQRS into the equation, showing where Commands and Queries belong, giving more examples of adapters. The diagram is large, but beautiful.

# Olympic Rings

I had a go myself once. About 3 years ago, my default project structure for .NET solutions contained 5 projects (not including tests).

- diagram



# Vertical Slice Architecture

Jimmy Bogard then throws all that out the window and proposes splitting the logic into [Vertical Slices](https://jimmybogard.com/vertical-slice-architecture/) instead.