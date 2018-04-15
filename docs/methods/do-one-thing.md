# Methods should do ONE thing


## Size matters

Methods should be short and small. I mean really small. How short should they be? It depends. Ideally methods should be between 1 to 5 lines. But this is of course not always possible.

> In fact, you cannot directly aim for a particular size. What you can do, is to aim to break **larger abstractions** into smaller ones. Go for **micro abstractions** and turn each one into a method with a name that represents that abstraction. 

Find the smallest piece of logic that you can give a name to. This is an art. Once you master it you will enjoy programming more. And you will feel more proud of your code.


## What is one thing?!

The following advice has appeared in one form or another for 30 years or more:

> Methods should do one thing, do it well, and do it only.


But what is the meaning of *“one thing”*? Of course a method can often have multiple statements. It can perform boolean logic, call other methods or do string or arithmetic calculations. Does that mean it’s doing one thing or multiple things?

It’s not about the number of statements or logic but rather the level of abstraction of each piece of the method’s body. 

> When we say a method should do one thing, what it really means is that it should do **one thing in one level of abstraction**. As part of the method body it can invoke other methods and run steps that are **one level below the stated name of the method**. In that case the method is doing one thing.

After all, the reason we write methods is to decompose a larger concept (i.e, the name of the method) into a set of steps at the next (lower) level of abstraction.

## Is my method doing one thing?
To know if a method is doing more than “one thing”  try to extract another method from one or a bunch of its statements and give it a sensible name. Then think whether this new method (concept) is:

- A level of abstraction below the original method; or
- At the same level as the original one, merely a restatement of its implementation.

> **Methods that do one thing cannot be reasonably divided into sections. They don’t need `#Region` blocks or headline comments to divide the implementation into sections.**

## One level of abstraction per method

In order to make sure our methods are doing “one thing,” we need to make sure that the statements within our methods are all at the same level of abstraction.

Mixing levels of abstraction within a method is always confusing. Readers may not be able to tell whether a particular expression is an essential concept or a detail. Worse, once details are mixed with essential concepts, more and more details tend to accrete within the method.
