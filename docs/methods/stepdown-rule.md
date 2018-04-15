# Reading Code from Top to Bottom: The Stepdown Rule

Code should read like a top-down narrative. Every method should be followed by those at the next level of abstraction so that we can read the program, descending one level of abstraction at a time as we read down the list of methods. I call this The Stepdown Rule.

To say this differently, we want to be able to read the program as though it were a set of “To” paragraphs, each of which is describing the current level of abstraction and referencing subsequent TO paragraphs at the next level down. 

- To do A we do B and then C.
- To do B, if E we do F and otherwise we do G
- To determine if E, we …
- To do F we …
- To do G we...
- To do B we…
- To do C we…

Learning to think this way is very important. It is the key to keeping methods short and making sure they do “one thing.” Making the code read like a top-down set of TO paragraphs is an effective technique for keeping the abstraction level consistent.

**Dependent methods:** If one method calls another, they should be vertically close in the source file, and the caller should be above the callee where possible. This gives the program a natural flow and enhances the readability of the whole module.

## The Newspaper Metaphor

Think of a well-written newspaper article. You read it vertically.

- At the top you see a headline that will:
    - tell you what the story is about
    - allow you to decide if you want to read it.

- The first paragraph gives you a synopsis of the whole story which:
    -  Hides all the details
    - Gives you the broad-brush concepts.
    - As you continue downward, the details increase until you have all the dates, names, quotes, claims, and other minutia.

## We would like a source file to be like a newspaper article

- The name should be simple but explanatory.
- The name, by itself, should be sufficient to tell us whether we are in the right module or not.
- The topmost parts of the source file should provide the high-level concepts and algorithms.
- Detail should increase as we move downward, until at the end we find the lowest level methods and details in the source file.

Would you read a newspaper that is just one long story containing a disorganized agglomeration of facts, dates, and names? A newspaper is composed of many articles, and most are very small. Very rarely articles are a full page long. This makes the newspaper usable.
