# Naming classes
Classes, interfaces, structures should be named as a noun or noun phrase, like `Customer`, `WikiPage`, `Account`, and `AddressParser`.

- A class name should not be a verb, adjective or adverb.
- Using adjectives before the name is perfectly fine. For example `SlowUserImporter` or `ParallelUserImporter`.
- It should be singular not plural.
- Also avoid words that are too broad like `XyzManager`, `XyzProcessor`, `XyzData`, `XyzEntity` or `XyzInfo` in the name of a class.
- Avoid names that are too generic in isolation like `Item`, `Importer`, `Generator`, ... Ensure you add enough additional words to make the purpose of the class clear.
 
 ## Suffixes
 Some suffix words in programming have special meaning associated to them.
 Those meanings often come from popular design patterns or framework concepts.
 
 Examples include `Controller`, `Factory`, `Provider`, `Exception`, `Attribute`, `Service`, `Collection`, etc.
 
 You should only use those names when their meaning is the same as the commonly understood meaning.
 Otherwise it can confuse other developers and lead them to make false assumptions.

> Variables and objects should also be named in the same way.
