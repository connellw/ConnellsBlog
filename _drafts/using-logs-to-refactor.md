---
layout: post
title: Using Logs to Refactor
tags: logging refactoring
notes:
- Sometimes it's difficult to refactor. This is because it's already spaghetti code. Methods scope variables etc.
---

One of my pet peeves in software is seeing so much boilerplate logging code everywhere. It mainly frustrates me because it's usually very easy to avoid, and avoiding it almost always leads to better code. It's as if having log lines encourages poor separation of concerns.

Take this example:

```c#
public bool IsReady()
{
    if(certainObject.HasSomeFlag)
    {
        _logger.Information("Flag is set. Not ready.");

        return false;
    }

    try
    {
        _processAThing.YeahYeah();

        _logger.Information("This was fine. We're ready.");

        return true;
    }
    catch (SpecificException exception)
    {
        _logger.Error("Specific error doing a thing.");
        
        return false;
    }
}
```

This method reports two different outputs:
1. To the caller of the method, whether or not a thing `IsReady`.
2. To a human reading the logs, the specific state of a thing at the time the caller needed to determine whether or not it `IsReady`.

Why do we need two different outputs for each state here? We can design the method to **return a value that is useful to both programs and programmers**.

```c#
public ReadyState GetReadyState()
{
    if(certainObject.HasSomeFlag)
    {
        return ReadyState.HasThatFlag;
    }

    try
    {
        _processAThing.YeahYeah();

        return ReadyState.Ready;
    }
    catch (SpecificException exception)
    {        
        return ReadyState.SpecificError;
    }
}
```

Now all I have to do is log the return value, and the program can just check it is `ReadyState.Ready`.