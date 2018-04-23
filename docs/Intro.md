# Introduction

It’s easy to write code that makes sense to you. At the time of writing, you will be deep in an understanding of the problem you’re trying to solve. But later on, other people maintaining the code, including the future you, aren’t going to have so deep an understanding.

The majority of the cost of a software project is in long-term maintenance. As systems become more complex, they take more and more time for a developer to understand. The clearer the author can make the code, the less time others will have to spend understanding it, and the happier everyone will be. 

This guide will teach you how to write more expressive and intention revealing code that’s a pleasure to read and to build upon.

## It starts with caring

The most important way to be expressive is to try. All too often we get our code working and then move on to the next problem without giving sufficient thought to making that code easy for the next person to read. Remember, the most likely next person to read the code will be you.

So take pride in your workmanship. Spend a little time with each of your functions and classes. Choose better names, split large functions into smaller functions, and generally just take care of what you’ve created. Care is a precious resource.

## A dirty little secret

 Writing software is like any other kind of writing. When you write an article, you get your thoughts down first in whatever order it comes to you. The first draft will be clumsy and disorganized. Then, so you refine and restructure it until it reads well.

Writing code is the same. No one, and I mean NO ONE, can write clean code from start. It always starts with a mess.

When you start to write functions, you are focused on coming up with the logic or algorithm to solve a problem or fulfil a requirement. You can write long and complicated functions with lots of indenting and nested loops. They can have long argument lists. The names are arbitrary, and there is duplicated code.

Once you solved the problem, you have a working code. Clumsy programmers will feel they are done at this point, and move on to the next item in the project. They are shameless enough to push that code into Git. But not you!

At this point you start to refactor, massage and refine that code, splitting out functions, changing names and eliminating duplication. You shrink the methods and reorder them. Sometimes you break out whole classes. 

In the end, you wind up with functions that follow the rules laid down in this guide. That’s the code you push into Git. That’s the code you present to the team, while keeping your head up. The mess of a code beforehand is a dirty little secret between you and yourself (or perhaps your partner in crime, if you did pair programming).

## Note: GCop and Tidy C# 

This guide does not include tips which are covered automatically via Tidy C# and GCop. There are many important elements in those tools and so you must ensure to install and use them in your applications. This guide mainly covers concepts which are not automatically enforced or implemented using those tools, and which you need to consider and apply.
