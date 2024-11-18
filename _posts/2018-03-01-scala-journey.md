---
title: My Journey to Scala or How I Unlearned Java
date: 2018-03-01
category: coding
tags: scala java journey
description: What I learned is important about Scala after years of coding in Java
---

I started using Scala about 3 years ago when I started working for [Yoco Technologies](https://www.yoco.co.za) in Cape Town.  I had used Java for many, many years, indeed I started playing around with v1.0 in 1995 and had built a production system using v1.0.2 in 1996 in a financial services setting.

Still, I was surprised when my interviewer from Yoco upon hearing me say that I had not used Scala before, observed, "It's okay, we have a really hard time finding people who have much experience using Scala.  I can see you've been using Java for a long time, I'm sure you'll be fine."

3 years later, we still struggle to find Scala developers and having made the journey to learn the language at a reasonably competent level, I simultaneously understand and am puzzled.  In my mind, Scala is the far superior language, some of the reasons for which I hope to make clear in the following paragraphs.  At the same time, I also appreciate that to use it effectively can be a daunting task.

By the way, I won't try to teach you Scala here -- there are excellent resources for that on the net.  I've listed some the ones I found particularly so at the end of this essay.

## Starting The Journey

So.  I had spent many years doing Java development before I started learning Scala.  I had written Java applications on diverse platforms: ranging from browser-based applets and desktop applications to server-side database, compute, and web page platforms.  I am comfortable with Java APIs and constructs for writing safe, multi-threaded applications, and I knew the networking APIs inside and out.

I think I was pretty facile with Java.  Yet, when I started using Scala, I found my efforts awkward and was often unhappy with them.  But in time, my code started to have a more fluid feel with fewer issues, both from a design standpoint, as well as decreased bug counts.  When I look back now, I ask myself: "What were the turning points in that journey?  What were the critical things that made me better at coding in Scala?"

It turns out that there are 3 aspects that, once I understood them, made me more effective at Scala:

- How to manage data using the Scala collections library
- Multi-threading in Scala
- Functions

But first, let's take a quick look at what is different between the languages, despite having many outward similarities.

## What Makes Scala Different?

At the end of the day, Scala is a language that provides a developer access to the same kind of facilities that Java does: object-orientation, automatic memory management, multi-threaded applications, data collections, database connectivity, networking, and so forth.  But the way that Scala approaches these facilities is fundamentally different from Java and to use it effectively, you need to be able to change your notions of what level you code at.  "Level" meaning here that Scala is a higher-level language with a richer set of facilities for abstraction than Java.

Certainly you can use Scala as a kind of improved Java, but in doing so, you miss the point.  Scala provides a large number of built-in facilities that make it easier to write the same application functionality as Java, but with far less boilerplate code.  Code that Java requires you to write but is simply so that you can connect your application logic with the various frameworks and APIs supported by the language.

I believe that, in the end, this can be attributed to one particular thing that Scala supports better than Java: functions.  The way that Scala implements functions is incredibly important and drives a number of related aspects of the language.  I discuss these other aspects first.

## Data Collections And Memory Management

Like Java, Scala has extensive data collections classes.  Also like Java, Scala has automatic memory management.  You never have to explicitly release memory back to the Scala runtime.  On the surface, there are many similarities.

Yet, when you use the Scala data collection classes and framework, they feel fundamentally different.  Why is that?  How did the Scala language designers achieve this?

In a nutshell, they accomplished it by making the default data collections _immutable_.  This means that when you create a collection of data elements, it cannot be changed afterwards.  Certainly, you can add items, you delete items, change items, and so forth to an existing collection.  But each time you perform one of these operations, you are creating a new data collection -- the original collection is unaffected by these changes.

This has incredibly important implications.  It means that when you pass a data collection to another piece of code, whether you wrote that code or it's some open-source library that has caught your fancy, it cannot be altered by that code.  That code might add or remove items to the collection, but that change does not affect _your_ collection.  It also means that if your data collection gets passed to another thread and your code and that code run in parallel, those parallel threads of execution will not trample on each other's work.

Thus, when you come to Scala from a language like Java which in its early days only supported mutable data collections which established a legacy of mutable data collections and the attendant challenges, Scala feels awkward. But once you get used to Scala's immutable idioms, you'll have a hard time imagining how you could have ever managed data any other way.

Of course, Scala has support for mutable data collections.  But the situations where you'll use those are few and far between.  When you use mutable collections, it will be a conscious decision that you make based on the requirements of your application.

Finally, the decision of whether to use immutable or mutable data collections is incredibly important with respect to the next topic: multi-threaded applications. 

## Multi-Threaded Applications

In this day and age, it's hard to think of writing applications that don't require some degree of multi-threaded or parallel execution.  Scala, like any modern programming language, has excellent support for writing multi-threaded applications.  But it is fundamentally different from Java.

In Java, there is vast divide between the code itself and mechanism by which you gain access to multi-threaded code execution.  Moreover, Java requires you to spend considerable effort ensuring that your code, when executed by multiple threads, allows concurrent access to any shared state, i.e. data.  Without using the proper and often complex synchronization facilities in Java, you risk faulty application behavior that can be very difficult to debug and fix.  The problem is further exacerbated by the fact that you often cannot determine whether any 3rd party software your application uses is thread-safe even if you've ensured that your own code is.

Java makes writing multi-threaded applications difficult for a number of reasons.  The first is the use of mutable data structures.  The second is that the multi-threading facilities are quite low-level; the Java thread library is mostly just a thin wrapper around operating system thread facilities.  But most damning is that Java implicitly encourages you to bind state to your applications components by requiring you to write _classes_ for all application logic.

Classes are a great mechanism for encapsulating and abstracting your data model and the way it used by other parts of your application.  However, because you have to pass an entire instance of a class (an object) around, the likelihood that you will pass a stateful object is _greatly_ increased.  Indeed, even if you originally write your class as a stateless object, another developer may extend it, adding state, and break your previous assumptions.

How does Scala help you from using this ample supply of rope to hang yourself, so to speak?  

- The first is the broad use of immutable data collections as was discussed earlier.
- The second is that in Scala, you are encouraged to write your application logic in stateless functions.  

To demonstrate how truly simply it is to write multi-threaded code in Scala, all you have to do is wrap that code in a `Future` block, like this:

```
def futureFibs(n: Int) = Future(fibs.take(n).toList)
```
(The `fibs` function is defined and discussed in the next section.)  

The point here is that any piece of code by virtue of being wrapped in a `Future` block can now be run concurrently with the enclosing code.  And while, one can argue, "I can do that with a Java `Runnable`", the point here is that there is still less code and Scala's ability to compose multiple futures is far more high-level than Java's.

Once I gained a basic mastery of how Scala manages multi-threaded code, coupled with my better appreciation of immutable data structures, my multi-threaded code become easier to understand and debug, _and_ it had fewer bugs.

For the interested reader, I strongly recommend Daniel Westheide's blog on Scala Futures: ["Welcome to the Future"](https://danielwestheide.com/blog/2013/01/09/the-neophytes-guide-to-scala-part-8-welcome-to-the-future.html).

## Functions

Functions as first-class citizens are the big kahuna in Scala.  What exactly do we mean by "functions are first-class citizens"?

It's really quite simple: Functions in Scala are a top-level type and can be instantiated separately from objects, making them very powerful as compared to Java.  You can pass them as arguments to functions, you can store them for later use, you can run them concurrently, and very importantly, function objects are _type-safe_.  So when you need a caller to pass in a function to your code, you can clearly specify that function's signature, meaning it's inputs and outputs.

But that's not the end of the story.  Although, I haven't discussed it much in previous sections, Scala provides a remarkable amount of syntactic sugar, which combined with its sophisticated type inference system, makes writing functions on the spot very easy.  So easy, that frequently you may not even see it as writing functions.  I'll digress a bit into some actual code to illustrate how simple this is.

For example, consider that we have a list of the Fibonacci numbers generated by this elegant piece of code:

```scala
def fibs: Stream[Int] = 0 #:: fibs.scanLeft(1)(_+_)
```

So given that function, we can create the a list of the first 10 Fibonacci numbers like this (output here is captured from using the Scala REPL):

```scala
fibs.take(10).toList
res65: List[Int] = List(0, 1, 1, 2, 3, 5, 8, 13, 21, 34)
```
Suppose that we want to retrieve only the odd values from the list of Fibonacci numbers?  We can easily do that like this:

```scala
fibs.take(10).toList.filter(n => n > 0 && n % 2 != 0)
res66: List[Int] = List(0, 1, 1, 3, 5, 13, 21)
```

So what's going on here?  Well, the Scala `List` class defines a `filter` function as follows:

```scala
def filter(p: (A) ⇒ Boolean): List[A]
```
The first clue that `filter` takes a function, called "p", is the `=>` operator, often referred to as the "rocket operator".  In particular, this specifically means that the evaluation of the function "p" will be deferred until we actually invoke `filter`.  The second takeaway here is that this function "p" take a single parameter of type "A" and should return a Boolean.  Scala supports generic types and so the "A" is simply a short-hand to let you know that, whatever you've stuffed into your list, your function needs to accept that as the type of the argument.

So, we could've written a standalone function, `isOdd` that looks like this:

```scala
def isOdd(n: Int): Boolean = n > 0 && n % 2 != 0
```
and then written the previous code like this:

```scala
fibs.take(10).toList.filter(isOdd)
res67: List[Int] = List(0, 1, 1, 3, 5, 13, 21)
```
Some readers may even prefer this as it makes it clearer what's going on and provides an opportunity for re-use of the `isOdd` function.  

The point here is that while you can write standalone functions, there is often little point in doing so.  Because functions can be written on the fly so easily in Scala, they frequently just part of the fabric of your application logic.  What makes this so natural is that the authors of these libraries made the effort to make their code extensible via not  sub-classing or via anonymous interfaces, which is what we probably would've done in Java, but have simply indicated that we need to pass in a piece of code, i.e. a function that does something useful.

Once I began to understand that the basic building block of functionality in my programs, especially application logic, should be in functions and not classes, the intent of my code became clearer and the code simpler. This approach can be summed up as follows:

- Use classes for orchestration.  
- Use functions for application logic.

# Final Words
This last observation bears repeating: Scala's function-oriented focus has let me focus on writing more useful code and less boilerplate.  Boilerplate code still needs to be debugged, still needs to be understood, and the less effort you need to put into understanding a colleague's code or some OSS framework, the more time you have for doing useful shit.

One thing I've discovered about the language is that learning it _is_ a journey, and a journey well worth embarked upon.  I like to think it didn't take me 3 years to become productive in Scala but what I find very interesting is that the longer I use the language, the **more** productive I have become.  In other words, I have yet to plateau with the language.

I hope that you decide to embark on this journey, too.

# Resources
I found Bruce Teckel's ["Atomic Scala"](https://www.atomicscala.com) to be an excellent text in the early parts of my journey.  It was recommended by a good friend of mine here in Cape Town who uses Scala extensively.

Again, I also thought that Daniel Westheide's blog series, ["The Neophyte's Guide To Scala"](https://danielwestheide.com/scala/neophytes.html) was both well-written and incredibly informative, especially on topics such as Scala futures.

#### About Me
I've been working in computing since the mid-1980s mostly in fintech, mostly on Unix-based platforms.  My language journey has been C, C++, Java, JavaScript, C#, and most recently Scala.  Along the way, there have been dalliances with LISP, Prolog, Perl, and myriad markup languages.  I am always keen to learn new languages in my pursuit of increasing my productivity and reducing my bug count.
