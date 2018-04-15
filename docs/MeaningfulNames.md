# Meaningful Names

Names are everywhere in software. We name our variables, functions, arguments, classes, and packages. We name our source files and the directories that contain them.

Naming is not just coming up with identifiers for things. Naming is about how we think. Naming is how we see the problem domain, and how we think about the solution. Naming is about design. It’s about creativity. It’s an art, and a science.

Naming is the most vital activity you do as a programmer. If you learn to care and take pride in your naming skills, you will be a pro, a valuable member of society, a master.

Choosing good names takes time but saves more than it takes. So take care with your names and change them when you find better ones. Everyone who reads your code (including you) will be happier if you do.

## Use Intention-Revealing Names

 Names should reveal intent. The name of a variable, function, or class, should answer all the big questions. It should tell you why it exists, what it does, and how it is used.

**If a name requires a comment, then the name does not reveal its intent:**

```c#
int d; // elapsed time in days
```

The name d reveals nothing. It does not evoke a sense of elapsed time, nor of days. We should choose a name that specifies what is being measured and the unit of that measurement:

```c#
int elapsedTimeInDays;
int daysSinceCreation;
int daysSinceModification;
int fileAgeInDays;
```

Choosing names that reveal intent make it easier to understand and change code.

### Avoid Disinformation

Avoid leaving false clues that obscure the meaning of code. Of course you’d think “what?! Why do you think I would do that?”. But it’s easy to make that mistake depending on your focus and state of mind in that moment.
When coming up with a name, step back for a second and play devil’s advocate. Think:

> How else can this name be interpreted?!

For a moment forget about how much it makes sense to you right now. Put yourself in the shoes of someone else with no background, and think whether they can be confused by that name, thinking it might be something that you know it’s not. Avoid words whose entrenched meanings may vary from your intended meaning.

### Be considerate of human eyes

Avoid using names which vary only in small ways. How long does it take to spot the difference between:

    * XYZControllerForEfficientHandlingOfStrings
    * XYZControllerForEfficientStorageOfStrings?

In particular, when using code completion in Visual Studio you can type in a few characters and then a space or tab, to easily pick the wrong one!

### Avoid noise words

Noise words are another meaningless distinction. Imagine that you have a Product class. If you have another called `ProductInfo` or `ProductData`, you have made the names different without making them mean anything different. Info and Data parts are pointless here. The same goes for a, an and the prefixes for variables: they are unnecessary.
Noise words are redundant. For example:

* The word **table** should never appear in a table name.
* How are `NameString` or `strName` or `m_Name` any better than simply Name?
* Imagine finding one class named **Customer** and another named **CustomerObject**. What would you understand as the distinction?

### Use Pronounceable Names

If you can’t pronounce a variable, you can’t discuss it without sounding like an idiot. *“Well, over here on the bee cee arr three cee enn tee we have a pee ess zee kyew int, see?”*
This matters because programming is a social activity. Instead of made-up words or abbreviations, use proper English terms.

## Class Names

- Classes and objects should have noun or noun phrase names like `Customer`, `WikiPage`, `Account`, and `AddressParser`.
- It should be singular not plural.
- A class name should not be a verb, adjective or adverb.
 - Also avoid words that are too broad like `Manager`, `Processor`, `Data`, or `Info` in the name of a class.

## Method Names

Methods should have verb or verb phrase names like `Save`, `PostPayment`, `DeletePage`, `IsXyz`, `CanXyz`, `HasXyz`. You will learn more details later. For casting in particular, you can use `ToXyz` or `AsXyz`.

### Add Meaningful Context

There are a few names which are meaningful in and of themselves—most are not. Instead, you need to place names in context for your reader by enclosing them in well-named classes, functions, or namespaces. When all else fails, then prefixing the name may be necessary as a last resort.

Imagine that you have variables named `firstName`, `lastName`, `street`, `houseNumber`, `city`, `state`, and `zipcode`. Taken together it’s pretty clear that they form an address. But what if you just saw the **state** variable being used **alone** in a method? Would you automatically infer that it was part of an address? 

You can add context by using prefixes: `addressFirstName`, `addressLastName`, `addressState`, and so on. At least readers will understand that these variables are part of a larger structure. But a better solution is to **create a class** named Address. Then, even the compiler knows that the variables belong to a bigger concept.

Consider the method below. Do the variables need a more meaningful context? The function name provides only part of the context; the algorithm provides the rest. Once you read through the function, you see that the three variables, number, verb, and pluralModifier, are part of the “guess statistics” message. Unfortunately, the context must be inferred. When you first look at the method, the meanings of the variables are opaque.

```c#
void PrintGuessStatistics(char candidate, int count)
{
 string number;
 string verb;
 string pluralModifier;

 if (count == 0) 
 {
    number = "no";
    verb = "are";
    pluralModifier = "s";
 } 
 else if (count == 1) 
 {
    number = "1";
    verb = "is";
    pluralModifier = "";
 } 
 else 
 {
    number = count.ToString();
    verb = "are";
    pluralModifier = "s";
 }

 var guessMessage = $"There {verb} {number} {candidate}{pluralModifier}";
 Print(guessMessage);
}
```

The function is a bit too long and the variables are used throughout. To split the function into smaller pieces we need to create a `GuessStatisticsMessage` class and make the three variables fields of this class. This provides a clear context for the three variables. They are definitively part of the `GuessStatisticsMessage`. The improvement of context also allows the algorithm to be made much cleaner by breaking it into many smaller functions:

```c#
public class GuessStatisticsMessage
{
 string number, verb, pluralModifier;

 public string Make(char candidate, int count)
 {
  CreatePluralDependentMessageParts(count);
  return string.Format("There %s %s %s%s",
   verb, number, candidate, pluralModifier);
 }

 void CreatePluralDependentMessageParts(int count)
 {
  if (count == 0) ThereAreNoLetters();
  else if (count == 1) ThereIsOneLetter();
  else ThereAreManyLetters(count);
 }

 void ThereAreManyLetters(int count) 
 {
  number = count.ToString();
  verb = "are";
  pluralModifier = "s";
 }

void ThereIsOneLetter()
{
  number = "1";
  verb = "is";
  pluralModifier = "";
}

 void ThereAreNoLetters()
 {
  number = "no";
  verb = "are";
  pluralModifier = "s";
 }
}
```

### Don’t add unnecessary context

In an imaginary application called “Gas Station Deluxe,” it is a bad idea to prefix every class with GSD. Always think whether a context is already implied and avoid adding the obvious as it’s just noise.

Likewise, if you have a Mailing Address class in GSD’s “accounting” application, if you named it **GSDAccount** Address. Ten of 17 characters are redundant as they are already implied.

Shorter names are generally better than longer ones, so long as they are clear. Add no more context to a name than is necessary.
The names `accountAddress` and `customerAddress` are fine names for instance variables of the class `Address` but could be poor names for classes. Address is a fine name for the class. If I need to differentiate between MAC addresses, port addresses, and Web addresses, then maybe you need to call it `PostalAddress`.

The essential rule for naming is: **Add as few words as necessary, to uniquely identify what you are naming, considering all implied context, and without causing ambiguity.**
