# What to avoid when naming things


## Avoid noise words

Noise words are another meaningless distinction. Imagine that you have a Product class. If you have another called `ProductInfo` or `ProductData`, you have made the names different without making them mean anything different. Info and Data parts are pointless here. The same goes for a, an and the prefixes for variables: they are unnecessary.
Noise words are redundant. For example:

* The word **table** should never appear in a table name.
* How are `NameString` or `strName` or `m_Name` any better than simply Name?
* Imagine finding one class named **Customer** and another named **CustomerObject**. What would you understand as the distinction?

## Avoid Disinformation

Avoid leaving false clues that obscure the meaning of code. Of course you’d think “what?! Why do you think I would do that?”. But it’s easy to make that mistake depending on your focus and state of mind in that moment.
When coming up with a name, step back for a second and play devil’s advocate. Think:

> How else can this name be interpreted?!

For a moment forget about how much it makes sense to you right now. Put yourself in the shoes of someone else with no background, and think whether they can be confused by that name, thinking it might be something that you know it’s not. Avoid words whose entrenched meanings may vary from your intended meaning.


## Avoid unnecessary context

In an imaginary application called “Gas Station Deluxe,” it is a bad idea to prefix every class with GSD. Always think whether a context is already implied and avoid adding the obvious as it’s just noise.

Likewise, if you have a Mailing Address class in GSD’s “accounting” application, if you named it **GSDAccount** Address. Ten of 17 characters are redundant as they are already implied.

Shorter names are generally better than longer ones, so long as they are clear. Add no more context to a name than is necessary.
The names `accountAddress` and `customerAddress` are fine names for instance variables of the class `Address` but could be poor names for classes. Address is a fine name for the class. If I need to differentiate between MAC addresses, port addresses, and Web addresses, then maybe you need to call it `PostalAddress`.

The essential rule for naming is: **Add as few words as necessary, to uniquely identify what you are naming, considering all implied context, and without causing ambiguity.**

## Avoid cryptic names
Names should reveal intent. The name of a variable, function, or class, should answer all the big questions. It should tell you why it exists, what it does, and how it is used.

**If a name requires a comment, then the name does not reveal its intent:**

```c#
int d; // elapsed time in days
```

Choosing names that reveal intent make it easier to understand and change code.
The name d reveals nothing. It does not evoke a sense of elapsed time, nor of days.
We should choose a name that specifies what is being measured and the unit of that measurement:

```c#
int elapsedTimeInDays;
int daysSinceCreation;
int daysSinceModification;
int fileAgeInDays;
```

## Be considerate of human eyes

Avoid using names which vary only in small ways. How long does it take to spot the difference between:

    * XYZControllerForEfficientHandlingOfStrings
    * XYZControllerForEfficientStorageOfStrings?

In particular, when using code completion in Visual Studio you can type in a few characters and then a space or tab, to easily pick the wrong one!
