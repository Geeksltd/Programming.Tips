# System Architecture 

## What is over-engineering?

Overengineering is when you design or build too much code, or add unnecessary complexity. 

Would you build an orbital laser to light a cigarette? An orbital laser is really cool, but it (a) costs too much (b) takes too much time and (c) is a maintenance nightmare.

The tricky part is–how do you know when you’re over-engineering? What’s the line between good design and too much design?

When your design actually makes things more complex instead of simplifying things, you’re over-engineering. An orbital laser would hugely complicate the life of somebody who just needed to light a cigarette. There are obviously easier options that will meet the requirement with less complexity.

## Avoiding over-engineering 

The best way to avoid over-engineering is just don’t design too far into the future. Overengineering tends to happen like this: 

“Okay, I need some code to reverse a string. Well, might as well make a whole system for rearranging and modifying the letters in a string, since we might need that someday.” 

Essentially, somebody imagined a requirement that they did not know for a fact whether or not was actually needed. They designed too far into the future, without actually knowing the future.

Now, if that developer really did **know** he’d need such a system in the near future, it would be a mistake to design the code in such a way that the system couldn’t be added later. It doesn’t need to be there now, but you’d be under-engineering if you made it impossible to add it later.


## Consequences of over-engineering 

With over-engineering, the big problem is that:

 - It makes it difficult for people to understand your code.
 - It makes it harder to change the code. 
 - It obscures how the whole system works (since it’s now so complicated). 
 - It locks you into a particular design before you can actually be certain it’s the right one.
 
## How does over-engineering happen?
There are lots of common ways to over-engineer. Probably the most common ways are: 

 - Making something extensible that won’t ever need to be extended.
 - Making something way more generic than it needs to be.
 - Prematurely optimizing the code, at the cost of reduced readability, without having measured the performance and identified a bottleneck.
 
**Both of the above come out of a really good intention: to design a flexible software.**

After all you want to be a good engineer and create a flexible architecture that can incorporate future requirements without needing to be redesigned. 

## The myth of flexibility
Of course there are cases when adding abstractions, generalising components and making things extensible is good and can make your software flexible and maintainable. It would be under-engineering to avoid them. If you know something may change in a particular direction in the future, make it easy today. 

The problem comes from when you think you are adding flexibility whilst in reality making your software practically more rigid and harder to change.

To understand this better, you need to dive deep into the concept of `Change`, `Predictability` and the mind games that come with it.

The desire to predict the future, and preparing for it, is in our DNA. It has been vital for our survival. We do this in all aspects of our life. We are always thinking about the future. We have a burning desire to prepare for any foreseeable scenario and keep our options open. We feel safe when we cover all the bases. 

We instinctively want to extend that into our software design, to make it flexible for accommodating incorporating every future requirement that we can think of. And in theory there is nothing wrong with that. 

The problem comes from a simple truth. 

 - In software design, there is no such thing as general flexibility.
 - You can facilitate change, and add flexibility only in particular directions.
 - Every software artefact that facilitates flexibility (abstractions, factories, inversion of control, generalisations, providers, design patterns, …), does it for very specific types of changes in a particular angle of the overall system. 
 - Every artefact that supports flexibility, also adds complexity and dependencies upon itself. 
 
When an actual change is requested in the future, if it’s compatible with the directions that you have specifically supported, it will be easy to apply. In other words, you will have achieved flexibility. 

But when a change is not in exactly the angle that you supported, it will be harder to apply, because of the added overall complexity.

So every time you make a software architecture more flexible for A, you will have made it practically less flexible for B, C, D and E.

## YAGNI
This principle (You Ain’t Gonna Need It) basically means to resist the temptation to predict the future. 

True and absolute flexibility does not come from fancy design patterns. It comes from simplicity. 

The less code there is in your software the easier it is to understand what’s going on, and the easier it is to change it in any direction that the real future brings about.

If you want to show off your software design skills and wisdom, avoid over engineering. Keep things simple. Write brief code with clear abstractions and naming that reads like a beautiful newspaper. 

Know your design patterns and architectural skills and use them when they add true value and help manage and reduce complexity. But never add them if there isn’t a good genuine justification. The less code the better. 

## Continuous simplicity via Refactoring

You don’t have to over-engineer in a huge way, either, to mess up your system. Little by little, tiny bits of over-engineering can stack up into one huge complex mass.

Good design is design that leads to simplicity in implementation and maintenance. An over-engineered design leads to difficulty in implementation, makes maintenance a nightmare, and turns otherwise simple code into a twisty maze of complexity. 

The nature of software development is so that as you add or change things, mess piles up automatically and must be continually cleaned up. We do this by incrementally refactoring the code. 

For every few lines of code that you add, pause and reflect on the new design. Did you just degrade it? If so, clean it up and run the tests to demonstrate that we haven’t broken anything. Having automated tests eliminates the fear that cleaning up the code will break it!

During every refactoring step, aim to improve every aspect of good software design principle:

 - increase cohesion
 - decrease coupling
 - modularize system concerns, and separate concerns
 - shrink functions and classes
 - choose better names and ensure expressiveness
 - eliminate duplication
 - ...
 
## Eliminate Duplication
Duplication is the enemy of a well-designed system. It represents additional work, risk, and unnecessary complexity. 

Duplication manifests itself in many forms. Lines of code that look exactly alike are, of course, duplication. 

Lines of code that are similar can often be massaged to look even more alike so that they can be more easily refactored. And duplication can exist in other forms such as duplication of implementation. For example, we might have two methods in a collection class:

```c#
int Size() { } 
bool IsEmpty() { }
```

We could have separate implementations for each method. The `isEmpty` method could track a boolean, while size could track a counter. Or, we can eliminate this duplication by tying `isEmpty` to the definition of size:

```c#
bool IsEmpty() => 0 == Size();
```

Creating a clean system requires the will to eliminate duplication, even in just a few lines of code. For example, consider the following code:

```c#
public void ScaleToOneDimension(float desiredDimension, float imageDimension)
{
    if (Math.Abs(desiredDimension - imageDimension) < errorThreshold) return;
    float scalingFactor = desiredDimension / imageDimension;
    scalingFactor = (float)(Math.Floor(scalingFactor * 100) * 0.01f);
    Bitmap newImage = ImageUtilities.getScaledImage(image, scalingFactor, scalingFactor);
    image = newImage;
}
public async Task Rotate(int degrees)
{
    Bitmap newImage = GetRotatedImage(image, degrees);
    image = newImage;
}
```

To keep this system clean, we should eliminate the small amount of duplication between the `ScaleToOneDimension` and `Rotate` methods:

```c#
public class VacationPolicy
{
    Bitmap image;
    float errorThreshold = 0;
    public void ScaleToOneDimension(float desiredDimension, float imageDimension)
    {
        if (Math.Abs(desiredDimension - imageDimension) < errorThreshold) return;
        float scalingFactor = desiredDimension / imageDimension;
        scalingFactor = (float)(Math.Floor(scalingFactor * 100) * 0.01f);
        ReplaceImage(ImageUtilities.GetScaledImage(image, scalingFactor, scalingFactor));
    }

    public async Task Rotate(int degrees)
    {
        ReplaceImage(GetRotatedImage(image, degrees));
    }

    public Bitmap GetRotatedImage(Bitmap bitmap, float angle)
    {
        using (Graphics graphics = Graphics.FromImage(bitmap))
        {
            graphics.TranslateTransform((float)bitmap.Width / 2, (float)bitmap.Height / 2);
            graphics.RotateTransform(angle);
            graphics.TranslateTransform(-(float)bitmap.Width / 2, -(float)bitmap.Height / 2);
            graphics.DrawImage(bitmap, new Point(0, 0));
        }
        return bitmap;
    }

    public Bitmap ResizeImage(Image image, int width, int height)
    {
        var destRect = new Rectangle(0, 0, width, height);
        var destImage = new Bitmap(width, height);
        destImage.SetResolution(image.HorizontalResolution, image.VerticalResolution);

        using (var graphics = Graphics.FromImage(destImage))
        {
            graphics.CompositingMode = CompositingMode.SourceCopy;
            graphics.CompositingQuality = CompositingQuality.HighQuality;
            graphics.InterpolationMode = InterpolationMode.HighQualityBicubic;
            graphics.SmoothingMode = SmoothingMode.HighQuality;
            graphics.PixelOffsetMode = PixelOffsetMode.HighQuality;

            using (var wrapMode = new ImageAttributes())
            {
                wrapMode.SetWrapMode(WrapMode.TileFlipXY);
                graphics.DrawImage(image, destRect, 0, 0, image.Width, image.Height, GraphicsUnit.Pixel, wrapMode);
            }
        }
        return destImage;
    }

    private void ReplaceImage(Bitmap newImage)
    {
        image = newImage;
    }
}
```

As we extract commonality at this very tiny level, we start to recognize violations of SRP. So we might move a newly extracted method to another class. That elevates its visibility. Someone else on the team may recognize the opportunity to further abstract the new method and reuse it in a different context. This “reuse in the small” can cause system complexity to shrink dramatically. Understanding how to achieve reuse in the small is essential to achieving reuse in the large.

### Template Method
The TEMPLATE METHOD pattern is a common technique for removing higher-level duplication. For example: 

```c#
public class VacationPolicy
{
   public void AccrueUSDivisionVacation()
 {    
   // code to calculate vacation based on hours worked to date     
  // ...
      // code to ensure vacation meets US minimums       // ...
      // code to apply vaction to payroll record  
     // ...
 }
public void AccrueEUDivisionVacation()
 {    
   // code to calculate vacation based on hours worked to date    
   // ...
      // code to ensure vacation meets EU minimums       // ...
      // code to apply vaction to payroll record  
     // ...
   }
}
```

The code across `AccrueUSDivisionVacation` and `AccrueEuropeanDivisionVacation` is largely the same, with the exception of calculating legal minimums. That bit of the algorithm changes based on the employee type.

We can eliminate the obvious duplication by applying the TEMPLATE METHOD pattern.

```c#
public abstract class VacationPolicy 
{
  public void AccrueVacation()
  {
    CalculateBaseVacationHours();
    AlterForLegalMinimums();
    ApplyToPayroll();
  }

  private void CalculateBaseVacationHours() 
  {
    /* ... */
  }
  
  abstract protected void AlterForLegalMinimums();
  
  private void ApplyToPayroll() 
  {
    /* ... */ 
  }
}

public class USVacationPolicy : VacationPolicy 
{
  protected override void AlterForLegalMinimums()
  {
      // US specific logic
  }
}

public class EUVacationPolicy : VacationPolicy 
{ 
  protected override void AlterForLegalMinimums() 
  {
      // EU specific logic
  }
}
```

The subclasses fill in the “hole” in the `AccrueVacation` algorithm, supplying the only bits of information that are not duplicated. 

## Separation of Concerns

Concerns are the different aspects of software functionality. This principle states that a given problem involves different kinds of concerns, which should be identified and separated to cope with complexity, and to achieve the required engineering quality factors such as robustness, adaptability, maintainability, and reusability.

### Example: UI layer vs Domain layer
For instance, the "business logic" of software is a concern, and the interface through which a person uses this logic is another.

As a rule of thumb, the business logic code should be kept outside of your UI code. Your UI code should merely include logic related to visual presentation of your application and user interaction. 

As a rule of thumb, If any piece of code does not involve Html tags or access to the Web pipeline objects (request, response, etc), it probably belongs to your business logic layer.

### Example: Layout vs Style
Another example is when using HTML and CSS. The HTML file defines the document structure. The CSS file defines how the document is presented on your screen. Mixing css code inside the html markup is a bad idea.
