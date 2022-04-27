---
layout: post
title: Well-Crafted Software
tags: refactoring agile
---

Great engineers truly care about what it is they're building. Not just the end result, making a customer happy and getting paid. They enjoy the engineering process, they love the tools they use, and they get a kick out of the job itself.

> If you do what you love, you'll never work a day in your life.

The best engineers know the tiny details in their tools. They know exactly when to use a hammer and when to use a drill. In software engineering, our tools are our language features, frameworks, design patterns and practices.

The best software engineers don't just solve problems, they build well-crafted solutions. They write code not just for the customer, but also for their future selves and for their peers who maintain the solution as it evolves. They make it clear, concise, intentional, understandable and maintainable.

Well-crafted doesn't mean perfect. We'll still get things wrong, through inexperience with a new tool or through experimentation, and we will learn from it, becoming experts in everything in our toolkit. We'll know when (and when not) to apply certain patterns and practices. We'll get good of course, but perhaps counter-intuitively to some, we'll get **fast**.

# Why Quality Suffers

By far the most common push-back is, *"we need to get this done quick"*, assuming that means trading off against doing it well.

It's usually pedalled by project managers, sometimes even by other engineers, and often served with a side of misunderstood agile jargon.

Sometimes it's *"only the MVP"*. Agile is about releasing working software frequently to get early feedback for further iterations. So if this statement is meant to be a reason not to write well-crafted code, then it assumes that code quality improves over time. In most cases, however, [software entropy](https://en.wikipedia.org/wiki/Software_entropy) will prevail -- the quality will deteriorate over time.

One way to avoid that is the promise of *"repaying tech debt later"*. We'll add something to the backlog. Let's be honest, this rarely comes to fruition. Usually because the person making the promise doesn't see the risk in "tech debt", forgets about it, and moves on to the next project.

The phrase *"tactical solution"* has somehow become synonymous with a workaround. [Building something quickly isn't the same as building something tactical](http://www.codingthearchitecture.com/2007/06/21/the_tactical_solution.html). Tactics are **the concrete steps you take towards a strategic goal**, not something you do in lieu of a strategy. Agile development is tactical by nature -- it's about delivering incremental value and responding to change. A tactical solution iterates towards your goal and should be architected appropriately.

# Duct Tape

Don't get me wrong, duct tape has its place. If there is a hole in the water pipe, and the room is beginning to flood, a great engineer's first action wouldn't be to drive to the nearest plumbing supply store and buy a replacement pipe. Put a plaster on it, stop it leaking first, *then* get your proper fix done.

One-off solutions that no-one will ever use again, sure. The sort of thing you wouldn't even push up a git repo for. What's the point in version control if you're never going to manage changes? If it's truly one-off, throw it away. Far too often do short-term solutions become ingrained.

> Nothing is more permanent than a temporary solution.

But these should be exceptions to the rule. If the world isn't on fire, and if there's a good chance the code will be looked at again in future, there's no reason engineers shouldn't care about their designs.

In fact, businesses should encourage engineers to care. Not only will carefully-crafted software enable the frequent delivery of working software, a culture that supports a love for the code itself will attract engineers who have cared about their craft and learned how to do it well.