# Meaningful Names

Names are everywhere in software. We name our variables, functions, arguments, classes, and packages. We name our source files and the directories that contain them.

Naming is not just coming up with identifiers for things. Naming is about how we think. Naming is how we see the problem domain, and how we think about the solution. Naming is about design. It’s about creativity. It’s an art, and a science.

Naming is the most vital activity you do as a programmer. If you learn to care and take pride in your naming skills, you will be a pro, a valuable member of society, a master.

Choosing good names takes time but saves more than it takes. So take care with your names and change them when you find better ones. Everyone who reads your code (including you) will be happier if you do.


### Use Pronounceable Names

If you can’t pronounce a variable, you can’t discuss it without sounding like an idiot. *“Well, over here on the bee cee arr three cee enn tee we have a pee ess zee kyew int, see?”*
This matters because programming is a social activity. Instead of made-up words or abbreviations, use proper English terms.

## Class Names

- Classes and objects should have noun or noun phrase names like `Customer`, `WikiPage`, `Account`, and `AddressParser`.
- It should be singular not plural.
- A class name should not be a verb, adjective or adverb.
 - Also avoid words that are too broad like `Manager`, `Processor`, `Data`, or `Info` in the name of a class.
 

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
