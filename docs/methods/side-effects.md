# Avoid Side Effects

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

## Temporal coupling

Temporal coupling occurs when there's an implicit relationship between two, or more, members of a class requiring clients to invoke one member before the other. This tightly couples the members in the temporal dimension.

Temporal couplings are confusing, especially when hidden as a side effect. If you must have a temporal coupling, you should make it clear in the name of the method. In this case we might rename the method `CheckPasswordAndInitializeSession`, though that certainly violates the “Do one thing” rule.

A better option will be to come up with a name that is one level of abstraction higher than the two steps of checking password, and initializing the session. In this example I call that method Login(). Then it will be doing “one thing” at its level of abstraction.
