# Classes

## Different nature of classes

Typical programming courses teach you what classes are and how to use them in general. On the other hand, programming standards and best practices often prescribe guidelines as if they are universal rules that apply to every class. But they may not always feel right (for a good reason, explained later).

Classes can be categorized differently depending on our perspective. Here we offer a few examples of categories of classes. It’s not an exact science, and the distinction is not 100% clear. But thinking in these terms help us organize our thoughts and make better sense of the programming best practices. Moreover, related tips at the end of each part will help us come up with better design and architecture.

### Entity classes

One of the early activities in enterprise application projects is a process called data modeling. During that process business concepts, data elements and relationship between them are defined, and such data elements are referred to as Entities. Also, Entities, often are objects that have an identity  that stays unchanged over time (e.g. a `GUID` property named ID).

Typically Entity classes are real objects and encapsulate data and behavior. As they are indeed abstractions of real world concepts, the programming best practices fully apply to entity classes.

### DTO classes (Data Transfer Objects)

A Data Transfer Object is an object that is used to encapsulate data, and send it from one subsystem of an application to another. DTO classes typically have data fields and properties, but no methods or events. They are dumb classes.

A common use case for DTOs is to create ViewModel classes in ASP.NET Mvc applications. As the name suggests, a ViewModel is the model for the view. Every view (e.g. a cshtml file) needs data to render itself,  which it receives from the Controller that received the http request in the first place.

Another common use case for DTOs is in systems integration and APIs. Since they are essentially a structured bag of data, they are suitable for serialization (e.g. as Json) and transferring across the network.

#### Value objects

Value objects are instances of classes that are immutable and distinguishable only by value of their properties/attributes. That is, unlike an Entity, which is known by its unique identifier, a value object is only identified by the value of its properties. Two entity objects are equal if they have equal ids, on the other hand, two Value Objects are equal if value of all their properties are exactly equal.

For example, an instance of a Date class can be a value object if its properties cannot be modified after creation and does not include an Id attribute:

```c#
class Date {
	public int Day { get; }
	public int Month { get; }
	public int Year { get; }

	public Date(int day, int month, int year) {
    		Day = day;
    		Month = month;
    		Year = year;
	}
	public Date AddDays(int day) {
    		int nextDay = /* … logic for calculating next day */
   		// this is immutable so state will never change,
		// instead a new instance will return
    		return new Date(nextDay, Month, Year);
	}
}

 ```

#### Service classes

In DDD, the business layer consists of two type of objects; Domain Model entities and pieces of domain that don’t logically fit into any of those entities. Logically related behaviors that don’t fit into Entities group together and shape domain services.

Avoid Domain Services as much as you can! Too much separation of Data in Entities and behavior in Services will cause an anti-pattern called Anemic Domain Model in which Entities are no more than a set of getters and setters (more or less DTO objects) and behaviors and domain logic are entirely moved into service classes while they should have been part of the domain model entities.

#### Helper classes

Also known as Utility classes, are often classes that don’t have a state of their own, consist only of static methods (so client code doesn’t need creating an instance of the class to access those methods), are used by many other classes. A good example is Convert class in .net (e.g Convert.ToInt32). 

#### Repository classes

Repository is a layer that mediates between domain model and data access layer. It hides the complexity of loading data from one or possibly multiple sources and mapping them to objects that can be used by the domain model.

#### Orchestration classes

## Framework implementation classes

What is a framework anyway? Wikipedia defines it as:

_An abstraction in which a software that provides generic functionality can be selectively changed by additional user-written code, in order to provide an application-specific software. A framework is different from a library in that the overall program's flow of control is not dictated by the caller, but by the framework. A user can extend the framework, usually by selective overriding._

### Example: ASP.NET Controllers

ASP.NET Mvc is a framework. If you just create a random class somewhere in an ASP.NET application, the framework will have no idea what it is or how to use it. Instead, you must implement very specific classes in very specific ways.

For example ASP.NET recognizes a concept named Controller. For your application to benefit from that concept, you should create classes that inherit from a special class in the framework called System.Web.Mvc.Controller. In other words you create a Controller Class in your project.

Unlike other classes that you would normally design, you have very limited freedom in how you design a Controller Class. For example: 

- You can’t control its life cycle. The framework will instantiate and destroy it when it wants to.
- You can’t inherit from any other abstraction that you already have. It has to inherit (ultimately) from System.Web.Mvc.Controller. 
- You should not treat your Controller Class as a reusable concept anywhere else in your project for any purpose other than what the ASP.NET framework wants it to mean.
- ...

### Framework implementation classes vs Abstraction classes

Controller is not the only example of a “Framework implementation” class. Unit Test classes, WPF pages, Http modules and many other such classes also have the same nature.

Most programming best practices and guidelines are primarily applicable to the classes that represent an abstraction. But, your framework implementation classes are often not real abstractions of concepts in your project’s domain. As a result, commonly you may find a general rule to not make sense or feel like it’s not applicable in your case. For example, the method naming rules are often inappropriate for naming unit test methods. 

## Smart vs Dumb classes

It’s essential that you understand the difference between smart and dumb objects: 

- Smart objects hide their data behind abstractions and expose functions that operate on that data.
- Dumb objects are simply a grouping of data fields or properties. They expose their data publicly and have no meaningful functions.

In the following example, the Geometry class operates on the three shape classes. The shape classes are dumb objects without any behavior. All the behavior is in the Geometry class.

```c#
public class Square
{
    public Point topLeft;
    public double side;
}

public class Rectangle
{
    public Point topLeft;
    public double height;
    public double width;
}

public class Circle
{
    public Point center;
    public double radius;
}

public class Geometry
{
    public double Area(object shape)
    {
        if (shape is Square s) return s.side * s.side;
        else if (shape is Rectangle r)) return r.height * r.width;
        else if (shape is Circle c) return Math.PI * c.radius * c.radius;
        else throw new NoSuchShapeException();
    }
}
```

Although this code is using classes, but its nature is procedural as opposed to Object Oriented. For example, if I add a new shape, I must change `Area()` and all the other potential functions in Geometry to deal with it. So there is no polymorphism in use here.

Now consider the object-oriented solution below with smart shape objects. Here the `Area()` method is polymorphic. No Geometry class is necessary. If I add a new shape, none of the existing functions are affected.

```c#
public class Square : Shape
{
    Point topLeft;
    double side;
    public double Area() => side * side;
}

public class Rectangle : Shape
{
    Point topLeft;
    double height, width;
    public double Area() => height * width;
}

public class Circle : Shape
{
    Point center;
    double radius;
    public double Area()=> Math.PI * radius * radius;
}
``` 

You should generally use smart objects and a proper object oriented approach unless you have a good reason not to. Smart objects allow you to provide better abstractions and distribute the overall logic and complexity in a nice way.

On the other hand, Dumb objects are preferred for data transfer across different applications through Json, XML or Binary serialization.

Another scenario where dumb objects are preferred is when you have completely alternating logic implementations that can be switched at runtime. But that’s a rare case.

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

## Cohesion

A class had maximum cohesion when every class field is used in every method. It has the minimum cohesion when each class field is only used by a single method. Of course, most classes fall in between.

Imagine if you create a graph for your class where each method and field is a node. You then draw a line from each method to the fields or other methods that it uses. If you end up with a fully connected graph, you have a cohesive class. But if you end up with isolated islands, you have an incohesive class.

![Cohesion](_media/Cohesion.png)

As a rule of thumb, high cohesion is good, as it means that the methods and fields of the class are co-dependent and belong together as a logical whole (right side).

But when cohesion is low, and you have unconnected islands, it’s as if you are forcing two separate logical units together (left side). It’s as if another class is trying to get out of the larger class. You should try to separate the fields and methods into two classes, so that the new classes are more cohesive.

### Breaking large methods via Cohesion

Imagine the following scenario:

- You have a large method with many variables and lots of code.
- You want to extract one small part of that method into a separate method.
- The code fragment that you want to extract uses four of the variables declared in the big method.
- You first think to pass the four variables into the new function as arguments.
- You then remember the rule that methods should not have too many arguments.
- Instead, you promote those four variables to class fields, to make them accessible to the new method and end up with no arguments to pass.

If the class had no other methods, everything would be fine. But imagine that the class already had other methods that have nothing to do with the new class fields that you have created. This means that you have reduced the cohesion of the class.

But wait! If there are a few functions that want to share certain fields, doesn’t that make them a class in their own right? Of course it does. When classes lose cohesion, split them!

Breaking a large method into many smaller ones often gives us the opportunity to split several smaller classes out as well. This gives your program a much better organization and a more transparent structure.
