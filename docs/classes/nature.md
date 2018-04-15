# Different nature of classes

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

