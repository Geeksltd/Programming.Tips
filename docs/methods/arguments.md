# How many arguments?

The ideal number of arguments for a method is zero. Next comes one, followed closely by two. Three arguments should be avoided where possible. More than three requires very special justification—and then shouldn’t be used anyway.

> **A large number of arguments in a method is usually a code smell, that something is missing.** 

Often you realise that the grouping of those argument represents an abstraction, a concept, a missing class in your application. Try hard to **come up with a class name that represents that grouping.** If you succeeded, then do create the class and change the original method to take a single instance of that instead of multiple arguments.

You might be surprised that sometimes when you do that, you may realise that the original method can actually be moved to that class for a cleaner design.  In particular, if the method was large and doing a lot, you will see that in the context of the new class it can be nicely broken down into several methods with lower abstraction levels.

## Argument Objects

When a method seems to need more than two or three arguments, it is likely that some of those arguments ought to be wrapped into a class of their own. Consider, for example, the difference between the two following declarations:

```c#
Circle MakeCircle(double x, double y, double radius);
```

```c# 
Circle MakeCircle(Point center, double radius);
```

Reducing the number of arguments by creating objects out of them may seem like cheating, but it’s not. When groups of variables are passed together, the way x and y are in the example above, they are likely part of a concept that deserves a name of its own. 

## Params Array Arguments

Sometimes we want to pass a varying number of arguments into a method. Consider, for example, the String.format method:

```c#
string.Format("{0} worked {1:F2} hours.", name, hours);
```

If the variable arguments are all treated identically, as they are in the example above, then they are equivalent to a single argument of type Array. Such array method arguments are decorated as params in C#.
