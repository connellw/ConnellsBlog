---
layout: tag
tag: cqrs
title: Command/Query Responsibility Segregation
---

**Command/Query Responsibility Segregation** is about dividing a system into two subsystems -- a query system that can only read data and a separate command system that can only write.

Commands and Queries themselves are usually messages or DTOs passed in to these subsystems.