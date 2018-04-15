# Exception Handling

When you program you should always be wary of errors. Input can be abnormal and devices can fail. Things can go wrong, and when they do, your code should do its part to help the situation.

## Use Try-Catch-Finally

Try/catch/finally blocks define a scope within your program. When you execute code in the try section, you are stating that execution can abort at any point and then resume at the catch.

Ideally try blocks should behave like transactions. Your catch/finally has to leave your program in a consistent state, no matter what happens in the try. Always think and ask yourself:

- What can possibly go wrong here? and
- What can I do to handle that situation?

Of course different types of exceptions can happen. C# allows you to catch each type in a separate block: `IOException`, `InvalidOperationException`, … or simply Exception. You should create a specialist catch block for a particular type of exception only if you can really do something special about it. Otherwise a single general purpose catch will be enough.

In most cases, irrespective of the type of error you need to do some clean up action and re-throw the error with a bit of improved error message and context.

## Provide Context with Exceptions

In C#, you can get a stack trace from any exception; however, a stack trace can’t tell you the intent of the operation that failed.

Stack trace shows you the stack of method calls to the point of failure. But it doesn’t reveal anything about the parameters, data fields and other context information.

>When throwing exceptions, create informative error messages. Mention the operation that failed and just enough context information, to give clues to the user or developer to know where to look in order to solve the problem and try again.

## Validating method arguments

The main purpose of validating arguments in methods is to inform the callers that they are using the method incorrectly. Therefore, this should be done only when the calling code is not in your control. The most common scenario is for when you are creating a public method in a library that is published for other developers.

### Is the caller in your control

Callers of nonpublic methods are always in your control. Therefore, you should generally not validate arguments in private and internal methods. If you are creating a public method that is meant to be called only by another code that you also own, again it's unnecessary to validate arguments.

In these cases, if the method is called with invalid arguments, it is a logical error within your control, that can be fixed at the source of the problem.

Remember that every unnecessary validation code is noise and comes at a cost. Short code is good code. Always think of code files as expensive real estate. Space is expensive because someone has to spend time to read that and think about it later on. Any line of code that doesn’t justify its existence must be eliminated.

### Debugging acceleration

There is a case for validating arguments even when it’s technically unnecessary, I.e. when you are in control of the calling code. And that’s to help speed up debugging in complex scenarios and algorithms.

Validating arguments at the beginning of such methods helps break early, rather letting the issue be cascaded down the call chain. Therefore, it makes it a bit faster to find the source cause of a bug.

But with this definition you should only add validations that truly help speed up the debugging process.

An example of where it doesn’t help is:

```c#
void SomeMethod(Customer customer)
{
   if (customer == null) 
       throw new ArgumentNullException(nameof(customer));
   
   customer.Owner.Notify();
}
```

In this example if the validation line didn’t exist you would just get a null reference exception. When debugging that, you would see immediately that the problem is with the argument being null. And you could just trace it back up the call chain to find the root cause of the bug. In other words, the validation code was useless and just unnecessary noise.

## Exceptions vs Error codes

Returning error codes from command functions is an obsolete technique when your language supports proper exception handling blocks.

Error code could be:

- Boolean (true for success, false for failure)
- Int (0 for success, a special number per failure reason)
- Enum

When calling a command, the caller  may **forget to handle the response error code** and as there will be no runtime exception or breaking of the execution this can lead to strange application behavior and hard to find bugs.

But even when the caller does handle the response error code, it can lead to deeply nested structures. For example:

```c#
if (DeletePage(page) == ActionResult.Ok)
{
    if (registry.DeleteReference(page.Name) == ActionResult.Ok)
    {
        if (configKeys.DeleteKey(page.Name.MakeKey()) != ActionResult.Ok)
        {
            Log.Error("ConfigKey not deleted");
        }
    }
    else
    { 
        Log.Error("DeleteReference from registry failed");
    }
}
else
{ 
    Log.Error("delete failed");
}
```

But if you use exceptions instead, it would be simply: 

```c#
try
{
   DeletePage(page);
   registry.DeleteReference(page.Name); 
   configKeys.DeleteKey(page.Name.MakeKey());
}
catch (Exception ex)
{
   Log.Error(ex);
}
```

If your error handling logic has any complexity or details, then it should be moved to a separate method.
