# Methods

## Size matters

Methods should be short and small. I mean really small. How short should they be? It depends. Ideally methods should be between 1 to 5 lines. But this is of course not always possible.

> In fact, you cannot directly aim for a particular size. What you can do, is to aim to break **larger abstractions** into smaller ones. Go for **micro abstractions** and turn each one into a method with a name that represents that abstraction. 

Find the smallest piece of logic that you can give a name to. This is an art. Once you master it you will enjoy programming more. And you will feel more proud of your code.

## Command/Query Separation

Methods should either do something or answer something, but not both. Either your method should change the state of an object (Command), or it should return some information about that object (Query). Doing both often leads to confusion and unintended side-effects.

### Naming boolean query methods

Question methods, that return a Boolean result, must be named as a fact. Good fact-like names are:

```c#
public bool IsSomething() { ... }
public bool HasSomething() { ... }
public bool CanSomething() { ... }
public bool ExpiresOn(...) { ... }
public bool RelatesTo(...) { ... }
public bool CaresFor(...) { ... }
```

Avoid command-like verbs for these. For instance avoid a Boolean method named `Validate()` or `Submit()`, etc.

### Naming non-boolean query methods

If the method returns anything other than Boolean, and it has no side effect, its name should start with verbs that imply a process that results in something.

```c#
public SomeType GetSomething() { ... }
public SomeType FindSomething() { ... }
public SomeType SelectSomething() { ... }
public double CalculateSomething() { ... }
public SomeType DetectSomething() { ... }
public IEnumerable<SomeType> FilterBy( ... ) { ... }
public SomeType Parse( ... ) { ... }
public static SomeType CreateFrom(...) { ... }
public string GenerateXyzReport() { ... }
```

The key point here is that the method name should start with a verb. For example the following examples are poor choices as they don’t start with a verb:

```c#
public Company Company() { ... }
public void Transactions() { ... }
public void SlowlyProcess() { ... }
```

The last example above starts with an adverb. A better name for it would be `ProcessSlowly()`.

### Naming command methods

Some methods are meant to change the state of the parent object or the outside world, such as in the database or file system. Those methods should be named with command verbs as opposed to query verbs. Good examples would be:

```c#
public void Archive()  { ... }
public void RestoreToLive()  { ... }
public async Task UpdateParent(...) { ... }
public async Task SendToXyz(...) { ... }
```

#### What should a command method return?

Ideally command methods usually return **`void`** (or **`Task`** , in async programming).

Sometimes command methods return a **`Boolean`** value that represents success or failure of the operation. This is normally not a good practice. Instead the method should **throw an exception** in the case of a failure. Though, if you have a  **critical performance situation** where the microseconds overhead added by exceptions is not acceptable, then returning Boolean would be ok.

If the method will create a new object as a result of the command, and it’s clear from the name of the method, then it’s ok to return that. For example a method named `CreateDailyTimesheet()` may add a new *timesheet* record in the database, and immediately return it.

```c#
public Timesheet CreateDailyTimesheet(Employee employee)
{
    var result = new TimeSheet { ... };
    Database.Save(result);
    return result;
}
```

Avoid returning arbitrary values from command methods. For example your `CreateDailyTimesheet()` must not return a decimal value to represent the total hours worked in a month!





### How many arguments?

The ideal number of arguments for a method is zero. Next comes one, followed closely by two. Three arguments should be avoided where possible. More than three requires very special justification—and then shouldn’t be used anyway.

**A large number of arguments in a method is usually a code smell, that something is missing.** 

Often you realise that the grouping of those argument represents an abstraction, a concept, a missing class in your application. Try hard to **come up with a class name that represents that grouping.** If you succeeded, then do create the class and change the original method to take a single instance of that instead of multiple arguments.

You might be surprised that sometimes when you do that, you may realise that the original method can actually be moved to that class for a cleaner design.  In particular, if the method was large and doing a lot, you will see that in the context of the new class it can be nicely broken down into several methods with lower abstraction levels.

#### Argument Objects

When a method seems to need more than two or three arguments, it is likely that some of those arguments ought to be wrapped into a class of their own. Consider, for example, the difference between the two following declarations:

```c#
Circle MakeCircle(double x, double y, double radius);
```

```c# 
Circle MakeCircle(Point center, double radius);
```

Reducing the number of arguments by creating objects out of them may seem like cheating, but it’s not. When groups of variables are passed together, the way x and y are in the example above, they are likely part of a concept that deserves a name of its own. 

#### Params Array Arguments

Sometimes we want to pass a varying number of arguments into a method. Consider, for example, the String.format method:

```c#
string.Format("{0} worked {1:F2} hours.", name, hours);
```

If the variable arguments are all treated identically, as they are in the example above, then they are equivalent to a single argument of type Array. Such array method arguments are decorated as params in C#.

#### Avoid Side Effects

Side effects are lies. Your method promises to do one thing, but it also does other hidden things such as making unexpected changes in:

- Fields or properties of its own class.
- Fields or properties of the parameters passed into the method
- Global stuff such as database, user cookies, …

Methods that have side effects can deceive the caller, cause hard to find bugs, and lead to strange couplings and dependencies. Consider the following example.

```c#
public class UserValidator
{
  public bool CheckPassword(string userName, string password)
  {
     var user = ... // logic to get user from database
     if (user == null) return false;

     if (... == false) // logic to check password
         return false;

     Session.initialize(user);
     return true;
  }
}
```

The side effect is the call to `Session.initialize()`. The `CheckPassword` method, by its name, says that it checks the password. The name does not imply that it initializes the session. So a poor caller who believes the name of the method, runs the risk of erasing the existing session data if it is merely trying to check the validity of the user.

That is, `CheckPassword()` can only be called at certain times, when it is safe to initialize the session. If it is called out of order, session data may be inadvertently lost. There is a term for this problem: temporal coupling.

### Temporal coupling

Temporal coupling occurs when there's an implicit relationship between two, or more, members of a class requiring clients to invoke one member before the other. This tightly couples the members in the temporal dimension.

Temporal couplings are confusing, especially when hidden as a side effect. If you must have a temporal coupling, you should make it clear in the name of the method. In this case we might rename the method `CheckPasswordAndInitializeSession`, though that certainly violates the “Do one thing” rule.

A better option will be to come up with a name that is one level of abstraction higher than the two steps of checking password, and initializing the session. In this example I call that method Login(). Then it will be doing “one thing” at its level of abstraction.
