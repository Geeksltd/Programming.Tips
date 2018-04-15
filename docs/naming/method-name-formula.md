# Naming instance methods

A method's name should follow 3 main rules:

- Clarify the intent
- Be as short as possible
- Be as long as needed to be unambiguous

And it should be based on the following formula:

> Verb + [{[Preposition] + Noun}] per argument

In simple words, a method name starts with a verb. Then if it takes arguments, for each argument, you *can optionally* add a noun to represent that argument, which *optionally* can be prefixed with a preposition.

To see how it works, let's look at an example.

## Case study: Shopping basket

Let’s say you have a ShoppingBasket class where you want to define a method to add a product to it.

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

   - `myBasket.Add(product, 3)`
   - `myBasket.AddProduct(product, 3)`
   - `myBasket.AddProductQuantity(product, 3)`
   - `myBasket.AddToBasket(product, 3)`
   - `myBasket.AddProductToBasket(product, 3)`
   - `myBasket.AddProductQuantityToBasket(product, 3)`
   - `myBasket.AddProductQuantityToBasketToBeCheckedOut(product, 3)`

Let’s immediately rule out the last option. Although it is the most clear and descriptive option, but it also is verbose and ignores the fact that there is a context that mustn’t be repeated unnecessarily. The “ToBeCheckedOut” part is redundant. Not only is it already implied anyway, but also it’s irrelevant to the method’s responsibility.

## The instance context

The context of an instance method (non-static) always implicitly contains the declaring class. For example, every instance method defined in the ShoppingBasket class, always has the Shopping Basket as a given. Therefore, the **“ToBasket”** part is is redundant.

As a rule of thumb, your instance methods should almost never add the defining class’s name to the end of the method.

## Arguments in the method name

So far we have ruled out some of the name options of this method, and are left with:

  - `myBasket.Add(product, 3)`
  - `myBasket.AddProduct(product, 3)`
  - `myBasket.AddProductQuantity(product, 3)`

So let’s go back to the “optionally” part mentioned at the beginning of this section in relation to adding the argument names to the method name.

### When you must include….

There are cases where you must add the argument name to the method name, and that’s when there is an ambiguity to be resolved.

In this example, what if you had another method that added a percentage of the whole inventory to the basket? Then having simply `AddProduct(product, 3)` would be ambiguous. Are we adding 3 as quantity or as percentage? To disambiguate you’d need to explicitly specify that to the method name. 

If you didn’t have an ambiguity though, it will be optional to add the name on.

### When it is optional

In this example there appears to be no ambiguity about the method, so all 3 options would work. However, for cleanness and brevity, it’s generally preferred to omit the argument names which are deemed obvious and which add no real further clarity to the method’s purpose. 

I would argue that simply **`Add()`** is a perfect name in this case.

## When there is ambiguity

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

  - `item.Update(myTimeSpan)`
  - `item.UpdateEstimation(myTimeSpan)`
  - `item..UpdateDeveloperEstimation(myTimeSpan)`


Which option would be the best name?
  
   - The first one is lacking sufficient context.
      - When you see that invocation, the context tells you that a task is being updated to a new time value.
      - But it doesn’t tell you the meaning of that time value. Is it the estimation date?
      - Is it the actual time it took to complete? …

   - The second example gives you a clearer picture. It’s the estimation of the task being updated, as opposed to any other timespan data of the task.
   - The third one is adding some more context information. It’s saying it’s the Developer estimation, as opposed to, say, client’s estimation.

The right choice between the second and the third item depends on  **whether in the domain of the project there is any ambiguity.** If in that application the only estimation being recorded is that of the developer, then the best name will be simply `UpdateEstimation()` in order to avoid stating the Obvious.

## Preposition: Further clarify the argument’s role

When passing an argument to a method, if you think about its role in that context, usually there will be a preposition that applies to the argument in the context of that method.

In the above example, you could use the “To” preposition and add it to the method name:

   - `item.UpdateEstimationTo(myTimeSpan)`

Adding prepositions to most methods can arguably make them read more naturally. However, by convention, if the argument’s role is completely obvious you can drop the preposition. Use what makes more sense and which feels more clear. For example, which of the following feels clearer?

   - `myLocation.GetDistanceFrom(anotherLocation)`
   - `myLocation.GetDistance(anotherLocation)`


The right answer usually depends on the bigger picture within the containing project. For example if Distance means the geo distance (as opposed to driving distance), then Distance From and Distance To would be the same. In that case adding “From” can potentially be harmful, for example when in a given client code scenario the argument is considered the  destination, and so passing to a method whose name ends with From would cause confusion.

On the other hand, there are cases where adding a preposition can improve readability, such as `TimeSpan.FromMinutes(...)` as opposed to `TimeSpan.Minutes(...)`.
