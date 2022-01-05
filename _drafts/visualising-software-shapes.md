---
layout: post
title: Visualising Software in Shapes
tags: architecture
---

- Note that these layers are not necessarily csproj.

When I think of a software system, I see shapes and lines connecting groups of smaller shapes and smaller lines, like a galaxy of solar systems with planets of continents, each with city roads. It's a structure that fits related concepts together and hides away complexity at different scales.

Sometimes I wonder if everyone views systems like this. We [whiteboard boxes and arrows](/drawing-boxes) to explain the shapes of these structures. These visual cues help imagine how these systems fit together and where things might live within a solution.

# Layered Architecture



# Hexagonal Architecture

**Ports and Adapters** was originally called the [Hexagonal Architecture when Alistair Cockburn proposed it](https://alistair.cockburn.us/hexagonal-architecture/) proposed it.

There is nothing special about the number six here, it is just a nice way to visualise the architecture. It's diagrams tend to use a hexagon in the middle, but the shape doesn't matter; it can just as easily be drawn as a circle.

..... bit on driving and driven adapters ?? .....

# Onion Architecture

[Jeffrey Palermo proposed the Onion Architecture](https://jeffreypalermo.com/2008/07/the-onion-architecture-part-1/), which shares the same premise as the Hexagonal Architecture.

> Hexagonal architecture and Onion Architecture share the following premise:  Externalize infrastructure and write adapter code so that the infrastructure does not become tightly coupled.

However, the core is further divided into other layers and the dependencies point towards the center.

> The fundamental rule is that all code can depend on layers more central, but code cannot depend on layers further out from the core.  In other words, all coupling is toward the center.



## Castle Architecture



## Clean Architecture

[Uncle Bob Martin's Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) expands on this idea. Using the same Dependency Rule, 

## Explicit Architecture

They're all quite generic approaches, providing a template that could work for different languages and frameworks. [Herberto Graca's Explicit Architecture](https://herbertograca.com/2017/11/16/explicit-architecture-01-ddd-hexagonal-onion-clean-cqrs-how-i-put-it-all-together/) is a much more detailed view, bringing DDD and CQRS into the equation, showing where Commands and Queries belong, giving more examples of adapters. The diagram is large, but beautiful.

# Olympic Rings

I had a go myself once.

# Vertical Slice Architecture

Jimmy Bogard then throws all that out the window and proposes splitting the logic into [Vertical Slices](https://jimmybogard.com/vertical-slice-architecture/) instead.