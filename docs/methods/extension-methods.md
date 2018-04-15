# Creating Extension methods

C# allows you to define extension methods to existing classes when you don’t own their source code. For example you can add new methods to the existing System.String class.

## Enum methods

Extension methods can be used to add methods to Enumerate types. This can sometimes make your code more readable and object-oriented like.

## General applicability

You should only create an extension method on a type when the method concept is applicable and meaningful for all instances of that type. Remember that they show up in intellisense. If an extension method isn’t meaningful as a general concept defined on that type, it can be confusing.

## Competing with existing methods

Extension methods cannot be used to override existing methods. An extension method with the same name and signature as an instance method will not be called.

Do not create an extension method for a class that you own. Favour normal instance methods when you own the source of the type.

### Extension method resolution

For an extension method to be usable, the consumer code should declare a using statement for the namespace of the class which declares the extension method.

If multiple extension methods with the same signature are found (from different extension method classes) you will get a compile error. But if one of them is in the same namespace as the consumer code, that will be selected.
