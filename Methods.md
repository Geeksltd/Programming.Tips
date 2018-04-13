# Methods (Functions)

## Size matters
The first rule of functions is that they should be small. The second rule of functions is that they should be smaller than that. How short should a function be? It depends. Ideally methods should be between 1 to 5 lines. But this is of course not always possible.

In fact, you cannot directly aim for a particular size. What you can do, is to aim to break larger abstractions into smaller ones. Go for micro abstractions and turn each one into a method with a name that represents that abstraction. 

Find the smallest piece of logic that you can give a name to. This is an art. Once you master it you will enjoy programming more. And you will feel more proud of your code. 


## Command Query Separation (CQS) Law

Functions should either do something or answer something, but not both. Either your function should change the state of an object (Command), or it should return some information about that object (Query). Doing both often leads to confusion and unintended side-effects.

### Name of Query functions (Boolean)

Question methods, that return a Boolean result, must be named as a fact. Good fact-like names are: 


```c#
public bool IsSomething() { ... }
public bool HasSomething() { ... }
public bool CanSomething() { ... }
public bool ExpiresOn(...) { ... }
public bool RelatesTo(...) { ... }
public bool CaresFor(...) { ... }

```

Avoid command-like verbs for these. For instance avoid a Boolean method named Validate() or Submit(), etc.

### Name of Query functions (non Boolean)

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

The last example above starts with an adverb. A better name for it would be ProcessSlowly().

### Name of Command functions

Some methods are meant to change the state of the parent object or the outside world, such as in the database or file system. Those methods should be named with command verbs as opposed to query verbs. Good examples would be:

```c#
public void Archive()  { ... }
public void RestoreToLive()  { ... }
public async Task UpdateParent(...) { ... }
public async Task SendToXyz(...) { ... }
```

**What should a command function return?**


Ideally command methods usually return **`void`** (or **`Task`** , in async programming). 


Sometimes command methods return a **`Boolean`** value that represents success or failure of the operation. This is normally not a good practice. Instead the method should **throw an exception** in the case of a failure. Though, if you have a  **critical performance situation** where the microseconds overhead added by exceptions is not acceptable, then returning Boolean would be ok.


If the method will create a new object as a result of the command, and it’s clear from the name of the method, then it’s ok to return that. For example a method named CreateDailyTimesheet() may add a new timesheet record in the database, and immediately return it.

```c#
public Timesheet CreateDailyTimesheet(Employee employee)
{
    var result = new TimeSheet { ... };
    Database.Save(result);
    return result;
}
```

Avoid returning arbitrary values from command methods. For example your CreateDailyTimesheet() must not return a decimal value to represent the total hours worked in a month!


## Micro abstractions



### Do One Thing

The following advice has appeared in one form or another for 30 years or more.


   **FUNCTIONS SHOULD DO ONE THING.
    THEY SHOULD DO IT WELL.
    THEY SHOULD DO IT ONLY.**


But what is the meaning of “one thing”? Of course a method can often have multiple statements. It can perform Boolean logic, call other methods or do string or arithmetic calculations. Does that mean it’s doing one thing or multiple things?

It’s not about the number of statements or logic but rather the level of abstraction of each piece of the method’s body.

When we say a method should do one thing, what it really means is that it should do **one thing in one level of abstraction**. As part of the method body it can invoke other functions and run steps that are **one level below the stated name of the function**. In that case the function is doing one thing. After all, the reason we write functions is to decompose a larger concept (i.e, the name of the function) into a set of steps at the next (lower) level of abstraction.

To know if a function is doing more than “one thing”  try to extract another function from one or a bunch of its statements and give it a sensible name. Then think whether this new function (concept) is:
   -  A level of abstraction below the original method; or
   - At the same level as the original one, merely a restatement of its implementation.

**Functions that do one thing cannot be reasonably divided into sections. They don’t need #Region blocks or headline comments to divide the implementation into sections.**


### One Level of Abstraction per Function

In order to make sure our functions are doing “one thing,” we need to make sure that the statements within our function are all at the same level of abstraction.

Mixing levels of abstraction within a function is always confusing. Readers may not be able to tell whether a particular expression is an essential concept or a detail. Worse, once details are mixed with essential concepts, more and more details tend to accrete within the function.

#### Reading Code from Top to Bottom: The Stepdown Rule
Code should read like a top-down narrative. Every function should be followed by those at the next level of abstraction so that we can read the program, descending one level of abstraction at a time as we read down the list of functions. I call this The Stepdown Rule. 


To say this differently, we want to be able to read the program as though it were a set of “To” paragraphs, each of which is describing the current level of abstraction and referencing subsequent TO paragraphs at the next level down. 

   - To do A we do B and then C. 
   - To do B, if E we do F and otherwise we do G
   - To determine if E, we …
   - To do F we … 
   - To do G we... 
   - To do B we…
   - To do C we… 

Learning to think this way is very important. It is the key to keeping functions short and making sure they do “one thing.” Making the code read like a top-down set of TO paragraphs is an effective technique for keeping the abstraction level consistent.


**Dependent methods:** If one method calls another, they should be vertically close in the source file, and the caller should be above the callee where possible. This gives the program a natural flow and enhances the readability of the whole module.


#### The Newspaper Metaphor

Think of a well-written newspaper article. You read it vertically.

   - At the top you see a headline that will:
      - tell you what the story is about
      - allow you to decide if you want to read it.
   
   - The first paragraph gives you a synopsis of the whole story which:
      -  Hides all the details
      - Gives you the broad-brush concepts.
   - As you continue downward, the details increase until you have all the dates, names, quotes, claims, and other minutia. </li>

**We would like a source file to be like a newspaper article:**

   - The name should be simple but explanatory.
   - The name, by itself, should be sufficient to tell us whether we are in the right module or not.
   - The topmost parts of the source file should provide the high-level concepts and algorithms.
   - Detail should increase as we move downward, until at the end we find the lowest level functions and details in the source file.     
   
Would you read a newspaper that is just one long story containing a disorganized agglomeration of facts, dates, and names? A newspaper is composed of many articles, and most are very small. Very rarely articles are a full page long. This makes the newspaper usable.


### Switch Statements vs Polymorphism

It’s also hard to make a switch statement that does one thing. By their nature, switch statements always do N things. Unfortunately, we can’t always avoid switch statements. Imagine the following example in an Employee class:

```c#
public decimal CalculatePay()
{     
     switch (this.EmployeeType) 
     {       
              case COMMISSIONED:
                   return CalculateCommissionedPay();      
              case HOURLY:
                  return CalculateHourlyPay();     
              case SALARIED:
                  return CalculateSalariedPay();      
              default:
                 throw new NotSupportedException();
    }
}
```
Basically it is saying that depending on the type of the employee, run a different logic. Now if in your application this was the only thing that depends on the employee type, this code is acceptable. 

However, if you find that a switch statement on an employee’s type is being repeated, for example for:

   - IsPayday(Date date)
   - DeliverPay(decimal pay)
   - ...

Then it’s a smell that something is missing. When you find yourself **repeating switch statements on a particular type of value**, you should change that to use polymorphism instead. 

In the above example you will need to create a specialist employee class per employee type, that inherits from an abstract employee. The base Employee class will define the `CalculatePay()`, `IsPayday()`, and `DeliverPay()` methods as abstract (without implementation). And each of the child classes, ie `CommissionedEmployee`, `HourlyEmployee` and `SalariedEmployee` will implement it. 


### Naming method arguments

Of course you wouldn’t send a random argument to a method for no reason. Arguments are passed to methods because they are needed. The method that receives an object in, will either query it for information, or it operates on it. But regardless, the argument is there with a purpose, it has a role to play in that context. That role is what the argument should be named after.

To see if an argument name is a good one, you should be able to make a statement as below, and it should make sense with no ambiguity:

   _**In the context of {method name}, this {argument type} is
     the {argument name}.**_
     
Consider the following example from a chat application:

```c#
class ChatSession
{
    ...
    public void SendMessage(User user, string message)
    {
       ...
    }
}
```

The statement sentence would be: “In the context of SendMessage, this user instance is the user.”

If you heard that, you’d be immediately asking “what do you mean by user? The user who sends, or the one who receives the message?”. 


The user argument here is named after its type, which is a very common thing among programmers. However, “user” is not a role in this context. If the argument was named sender or receiver, then it would be a clear role, or purpose for that argument. 


Consider another example: 

```c#
class TradingService
{
    ...
    public void ExecuteTransaction(Company company1, Company company2, Product product)
    {
       ...
    }
}  
```

In this example, the last argument, product, seems to be named after its type. However, its role in that context can also be called a “product” as the following statement makes clear sense: “In the context of ExecuteTransaction, this Product instance is the product.” 


But can you say the same thing for company1 and company2? Of course not. Those names do not clarify the role of these arguments in that method. A better name would be **buyer** and **seller**.


**In other words, we’re not saying that argument must never be named after their type. But rather, that their type is not what determines their name, even though the two can be the same in many occasions.**


Consider another example: 

```c#
public void UpdatePrice(Product product, decimal value)
{
     ...
}
```

Although the purpose of **`value`** may not be ambiguous to you, one could still be asking, is this value thing the new price, or the increase in the price? Furthermore, it doesn’t form the best statement sentence: In the context of UpdatePrice, this decimal instance is the value.

If, instead, you named it after its **true role** in that context, which would be **newPrice**, then not only would the statement sentence make perfect sense, but also it would make the code immediately disambiguous.

### Naming instance methods

You previously learnt that a method’s name should start with a verb, and that it should be clear and unambiguous. So what should go after the verb?

To name a method that takes arguments, the verb can optionally be followed by a preposition, and then a noun per argument.

**Method name = Verb + {[Preposition] + Noun} per argument**

But how should you decide on the “optionally” part?

#### Case study: Shopping basket

Let’s look at an example. Let’s say you have a ShoppingBasket class where you want to define a method to add a product to it.

```c#
class ShoppingBasket
{
    public void {SomeMethodName}(Product product, int quantity)
    {
          ...
    }
}
```

What method name would be best? Let’s try a few different choices, from the point of view of the client code:

   - myBasket.Add(product, 3)
   - myBasket.AddProduct(product, 3)
   - myBasket.AddProductQuantity(product, 3)
   - myBasket.AddToBasket(product, 3)
   - myBasket.AddProductToBasket(product, 3)
   - myBasket.AddProductQuantityToBasket(product, 3)
   - myBasket.AddProductQuantityToBasketToBeCheckedOut(product, 3)

Let’s immediately rule out the last option. Although it is the most clear and descriptive option, but it also is verbose and ignores the fact that there is a context that mustn’t be repeated unnecessarily. The “ToBeCheckedOut” part is redundant. Not only is it already implied anyway, but also it’s irrelevant to the method’s responsibility.

#### The instance context
The context of an instance method (non-static) always implicitly contains the declaring class. For example, every instance method defined in the ShoppingBasket class, always has the Shopping Basket as a given. Therefore, the **“ToBasket”** part is is redundant.

As a rule of thumb, your instance methods should almost never add the defining class’s name to the end of the method.

#### Arguments in the method name
So far we have ruled out some of the name options of this method, and are left with:

  - myBasket.Add(product, 3)
  - myBasket.AddProduct(product, 3)
  - myBasket.AddProductQuantity(product, 3)

So let’s go back to the “optionally” part mentioned at the beginning of this section in relation to adding the argument names to the method name.

**When you must include….**

There are cases where you must add the argument name to the method name, and that’s when there is an ambiguity to be resolved.

In this example, what if you had another method that added a percentage of the whole inventory to the basket? Then having simply AddProduct(product, 3) would be ambiguous. Are we adding 3 as quantity or as percentage? To disambiguate you’d need to explicitly specify that to the method name. 

If you didn’t have an ambiguity though, it will be optional to add the name on. 

**When it is optional**

In this example there appears to be no ambiguity about the method, so all 3 options would work. However, for cleanness and brevity, it’s generally preferred to omit the argument names which are deemed obvious and which add no real further clarity to the method’s purpose. 

I would argue that simply <strong> Add() </strong> is a perfect name in this case.

#### When there is ambiguity

Now consider the following example. Let’s say in an issue tracking system, we have a WorkItem class which needs  a method to update the estimation time for the work item:

```c#
class WorkItem
{
       public void {MethodName}(Timespan estimation)
       {
              ...
       }
}
```

Again let’s consider 3 name choices, and consider them from the client code’s point of view:

  - item.Update(myTimeSpan)
  - item.UpdateEstimation(myTimeSpan)
  - item..UpdateDeveloperEstimation(myTimeSpan)


Which option would be the best name? 
  
   - The first one is lacking sufficient context.
      - When you see that invocation, the context tells you that a task is being updated to a new time value. 
      - But it doesn’t tell you the meaning of that time value. Is it the estimation date?
      - Is it the actual time it took to complete? …         

   - The second example gives you a clearer picture. It’s the estimation of the task being updated, as opposed to any other timespan data of the task.
   - The third one is adding some more context information. It’s saying it’s the Developer estimation, as opposed to, say, client’s estimation. 

The right choice between the second and the third item depends on  **whether in the domain of the project there is any ambiguity.** If in that application the only estimation being recorded is that of the developer, then the best name will be simply `UpdateEstimation()` in order to avoid stating the Obvious.

#### Preposition: Further clarify the argument’s role
When passing an argument to a method, if you think about its role in that context, usually there will be a preposition that applies to the argument in the context of that method.

In the above example, you could use the “To” preposition and add it to the method name:

   - `item.UpdateEstimationTo(myTimeSpan)`

Adding prepositions to most methods can arguably make them read more naturally. However, by convention, if the argument’s role is completely obvious you can drop the preposition. Use what makes more sense and which feels more clear. For example, which of the following feels clearer?

   - `myLocation.GetDistanceFrom(anotherLocation)`
   - `myLocation.GetDistance(anotherLocation)`


The right answer usually depends on the bigger picture within the containing project. For example if Distance means the geo distance (as opposed to driving distance), then Distance From and Distance To would be the same. In that case adding “From” can potentially be harmful, for example when in a given client code scenario the argument is considered the  destination, and so passing to a method whose name ends with From would cause confusion.

On the other hand, there are cases where adding a preposition can improve readability, such as `TimeSpan.FromMinutes(...)` as opposed to `TimeSpan.Minutes(...)`.

### How many arguments?

The ideal number of arguments for a function is zero. Next comes one, followed closely by two. Three arguments should be avoided where possible. More than three requires very special justification—and then shouldn’t be used anyway.

**A large number of arguments in a method is usually a code smell, that something is missing.** 

Often you realise that the grouping of those argument represents an abstraction, a concept, a missing class in your application. Try hard to **come up with a class name that represents that grouping.** If you succeeded, then do create the class and change the original method to take a single instance of that instead of multiple arguments.

You might be surprised that sometimes when you do that, you may realise that the original method can actually be moved to that class for a cleaner design.  In particular, if the method was large and doing a lot, you will see that in the context of the new class it can be nicely broken down into several methods with lower abstraction levels.

#### Argument Objects

When a function seems to need more than two or three arguments, it is likely that some of those arguments ought to be wrapped into a class of their own. Consider, for example, the difference between the two following declarations:

```c#
Circle MakeCircle(double x, double y, double radius);
```

```c# 
Circle MakeCircle(Point center, double radius);
```

Reducing the number of arguments by creating objects out of them may seem like cheating, but it’s not. When groups of variables are passed together, the way x and y are in the example above, they are likely part of a concept that deserves a name of its own. 

#### Params Array Arguments

Sometimes we want to pass a varying number of arguments into a function. Consider, for example, the String.format method:

```c#
string.Format("{0} worked {1:F2} hours.", name, hours);
``` 

If the variable arguments are all treated identically, as they are in the example above, then they are equivalent to a single argument of type Array. Such array method arguments are decorated as params in C#.



### Creating Extension methods
C# allows you to define extension methods to existing classes when you don’t own their source code. For example you can add new methods to the existing System.String class. 

#### Enum methods

Extension methods can be used to add methods to Enumerate types. This can sometimes make your code more readable and object-oriented like.


#### General applicability

You should only create an extension method on a type when the method concept is applicable and meaningful for all instances of that type. Remember that they show up in intellisense. If an extension method isn’t meaningful as a general concept defined on that type, it can be confusing.

#### Competing with existing methods
Extension methods cannot be used to override existing methods. An extension method with the same name and signature as an instance method will not be called.


Do not create an extension method for a class that you own. Favour normal instance methods when you own the source of the type.

#### Extension method resolution
For an extension method to be usable, the consumer code should declare a using statement for the namespace of the class which declares the extension method.

If multiple extension methods with the same signature are found (from different extension method classes) you will get a compile error. But if one of them is in the same namespace as the consumer code, that will be selected.

#### Avoid Side Effects

Side effects are lies. Your function promises to do one thing, but it also does other hidden things such as making unexpected changes in:
  - Fields or properties of its own class.
  - Fields or properties of the parameters passed into the function
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

The side effect is the call to `Session.initialize()`. The CheckPassword function, by its name, says that it checks the password. The name does not imply that it initializes the session. So a poor caller who believes the name of the function, runs the risk of erasing the existing session data if it is merely trying to check the validity of the user.

That is, `CheckPassword()` can only be called at certain times, when it is safe to initialize the session. If it is called out of order, session data may be inadvertently lost. There is a term for this problem: temporal coupling.

**Temporal coupling**

Temporal coupling occurs when there's an implicit relationship between two, or more, members of a class requiring clients to invoke one member before the other. This tightly couples the members in the temporal dimension.

Temporal couplings are confusing, especially when hidden as a side effect. If you must have a temporal coupling, you should make it clear in the name of the function. In this case we might rename the function CheckPasswordAndInitializeSession, though that certainly violates the “Do one thing” rule.

A better option will be to come up with a name that is one level of abstraction higher than the two steps of checking password, and initialising the session. In this example I call that method Login(). Then it will be doing “one thing” at its level of abstraction. 

### You are a storyteller 
When developing any system, you are designing a language to describe the domain of that project. Functions are the verbs of that language, and classes are the nouns. Together, they are a Domain-Specific-Language.

**The art of programming is, and has always been, the art of language design.**

Master programmers think of systems as stories to be told rather than programs to be written. They use the facilities of C# (as a host language) to construct a much richer and more expressive language that can be used to tell that story. 
