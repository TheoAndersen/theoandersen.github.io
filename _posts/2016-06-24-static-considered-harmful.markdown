---
layout: post
title: "Static considered harmful"
comments: true
published: true
---
I consider myself a decent object-oriented programmer, in which the keyword ```static``` has often annoyed me, because of its general misuse. 
I still Often see applications, where almost everything done in static and there are almost no objects in use. This obviously makes for a very inflexible and hard to test application.
But the static keyword holds more problems than this excessive coupling, its also problematic when its used to create static data, especially when your running in a multi-threaded context - which everyone is these days.

On a larger project a few years ago, I was optimizing the typical ever growing automated test-suite, and found that it was only running on a single thread, executing the tests sequentially. The web-application in context, hosted a list of web-services, which runs in multiple threads, because its got multiple concurrent clients, so what would be the issues of making the tests also running in parallel, just like the real world does? - a lot I found out.

Parallizing our test suite, the build server maxed out is cpu-resources (as it should), which caused the threads to begin to switch a bit more, than when the machine runs normally. This made every threading problem in the application bubble up to the surface, and begin failing. It seemed that even though this was very much a multi-threaded application exposing web-services on multiple threads, it wasn't really coded to handle multithreading very well.

What I found was that every non thread-safe global (read static) data in the application, both on the test-side and in the production code failed, running the tests like this. We found several custom cache's which failed, we found that many third party libraries we used, contained static singletons which wasnt thread-safe, and thereby also failed (our mocking library fx) - I even found that the general database access logic we had in place, and which had been running for many years, contained a threading-vulnarability, which was exposed when run hard.

Nobody had thought about testing for or making the code thread-safe.

# Why do we need the concern of thread-safety?

When you think about it, 'thread-safe' is a weird word. Why should we have to worry if something is safe to be run in a thread? Everything runs in a thread.

> A piece of code is thread-safe if it manipulates shared data structures only in a manner that guarantees safe execution by multiple threads at the same time.

The above line is from wikipedia, and explains the problem.

We are sharing data between threads - not communicating between threads, but just sharing bare data. The static part makes the data shared between threads. So every thread now has raw access to the shared static data, and its up to them not to fall over each other as they try to use this data. This without the threads really knowing about each other - good luck. 

So in my view, The problem is that the threading model we use is a brittle one, and its very hard to do it right and to know that you've done it right (feedback), especially when sharing data.

There are other languages which use better models for concurrency, but thats a whole other blog-post in it self - but if you're interested, then take a look at how [Erlang](http://www.erlang.org) does concurrency. And if you ask Joe[^1], [object-oriented languages are really going about this all wrong](http://harmful.cat-v.org/software/OO_programming/why_oo_sucks), and I'm beginning to agree with him.

# What you can do about it

Well to begin with, focus on getting the right feedback by testing the application as its used in production. So if you've got an application thats used concurrent in production, why not run the automated tests concurrently? And make it easier for yourself and do this from the very beginning of your test-suite, so you get the feedback early and in increments - and not as i did in that example.

Identity early on the statics of your application (the globals) - your threads will fight over access to them. I wish I knew a more failsafe way to do this (no don't say thread-safe ;)). The way you safely share data across many actors is to make somebody responsible for the data. This way you change the problem from having many kinds fighting over the bowl of candy, to having one person holding the bowl, and handing candy out in turn - changing the solution from data-handling to communication. 
You could use a separate dedicated database for the caching. This way you will also consolidate the duplicated cache across web-servers, if you have more than one.

[^1]: Joe Armstrong - one of the creators of erlang

