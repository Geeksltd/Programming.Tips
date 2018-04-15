# Naming method arguments

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

The statement sentence would be: *“In the context of SendMessage, this user instance is the user.”*

If you heard that, you’d be immediately asking *“what do you mean by user? The user who sends, or the one who receives the message?”*.

The user argument here is named after its type, which is a very common thing among programmers. However, *“user”* is not a role in this context. If the argument was named sender or receiver, then it would be a clear role, or purpose for that argument.

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
