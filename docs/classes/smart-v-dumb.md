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
