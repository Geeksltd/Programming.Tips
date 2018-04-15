# Command/Query Separation

Methods should either do something or answer something, but not both. Either your method should change the state of an object (Command), or it should return some information about that object (Query). Doing both often leads to confusion and unintended side-effects.

## Naming boolean query methods

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

## Naming non-boolean query methods

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

## Naming command methods

Some methods are meant to change the state of the parent object or the outside world, such as in the database or file system. Those methods should be named with command verbs as opposed to query verbs. Good examples would be:

```c#
public void Archive()  { ... }
public void RestoreToLive()  { ... }
public async Task UpdateParent(...) { ... }
public async Task SendToXyz(...) { ... }
```

### Tip: What should a command method return?

Ideally command methods usually return **`void`** (or **`Task`** , in async programming).

#### Returning boolean
Sometimes command methods return a **`Boolean`** value that represents success or failure of the operation. This is normally not a good practice. Instead the method should **throw an exception** in the case of a failure.

Though, if you have a  **critical performance situation** where the microseconds overhead added by exceptions is not acceptable, then returning Boolean would be ok.

#### Returning a created object
If the method will create a new object as a result of the command, and it’s clear from the name of the method, then it’s ok to return that.

For example a method named `CreateDailyTimesheet()` may add a new *timesheet* record in the database, and immediately return it.

```c#
public Timesheet CreateDailyTimesheet(Employee employee)
{
    var result = new TimeSheet { ... };
    Database.Save(result);
    return result;
}
```

Avoid returning arbitrary values from command methods.
For example your `CreateDailyTimesheet()` must not return a decimal value to represent the total hours worked in a month!
