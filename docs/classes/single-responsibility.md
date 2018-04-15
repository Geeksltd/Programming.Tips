# The Single Responsibility Principle

## Classes Should Be Small

Just like methods, classes should be small. But what is small? To measure “size”: 

- For methods, we simply count code lines.
- For classes we count responsibilities.

It’s not about the number of methods. For example:

- A class can have 100 methods, yet few responsibilities.
- It can also have 2 methods and yet too many responsibilities.

The name of a class should describe what responsibilities it fulfills. In fact, naming is probably the first way of helping determine class size. If we cannot derive a concise name for a class, then it’s likely too large. You should also be able to describe the class in about 25 words, without using the words **if** , **and** , **or** or.

The more ambiguous the class name, the more likely it has too many responsibilities. For example, class names including words like **Processor** or **Manager** often hint at aggregation of responsibilities. Avoid God classes.

## The Single Responsibility Principle

The Single Responsibility Principle (SRP) states that a class should have one specific purpose, one responsibility, one “reason to change”. It basically means that **when designing a system, you should try to identify responsibilities across the system. Then for each one, define a clear abstraction, give it a good name and define a class for it.**

SRP is a very important concept in object oriented design. Yet unfortunately it is too often violated, and we regularly encounter classes that do too many things. Why? Because getting software to work, and making it clean, are two different activities. It’s perfectly normal that you first focus on fulfilling a business requirement and writing code that works. But resist the temptation to move on to the next task straight away. Instead, take a step back and break the overstuffed classes into decoupled units with single responsibilities.

Some developers fear that a large number of small classes makes it harder to understand the bigger picture. But that’s a fallacy. There is just as much to learn in the system with a few large classes. The question is: **Do you want your tools organized into toolboxes with many small drawers each containing well-labeled components? Or do you want a few drawers that you just toss everything into?**

### Example: Data mapping

When working with multiple layers, we often need to convert different types of data transfer objects. So, a mapping is required. Do not put the logic of mapping between two objects inside any of the two. No class needs to know about existence of the other. Every class has its responsibility. Adding the mapping responsibility to it, means breaking the SRC principle. Instead, create a separate class responsible for mapping between the two.
