---
layout: post
title: "Three programming language execution models"
comments: true
published: true
image: /assets/executionmodel_actors.png
---
Every code written in a programming language, is executed in a specific way when run. The executions depends on the language and defines how it interacts with the hardware. This is called an [execution model](https://en.wikipedia.org/wiki/Execution_model).

I'm no compiler expert, but I do think that all programmers should learn multiple programming languages and along with it, know the basics of the execution models, in which they run. 

The execution model typically underlines the purpose of the language, and can explain how the language deals with parallel programming. The aspect of how we do parallel programming, is becoming more and more important these days as CPU manufacturers no more increase the CPU speed, but now [focus on adding more cores](http://www.gotw.ca/publications/concurrency-ddj.htm). This means that we need become better and developing programs that utilize multiple CPU's at once.

What you should know is that not all languages/execution models are born equal in this regard. And that the execution model of a language, has everything to do with whether this is easy or hard to program.

This post will explains the execution models, in regards to parallel programming, of three popular execution-models, each which have their own approach, advantages and disadvantages.

# Threads in most object-oriented languages#
The first is the probably most known one; the basic threading execution-model. This is used by most object-oriented languages, as Java, C#, C, C++, Python, PHP and so on. The basics of this one is that your program runs in threads, which can access shared data. 

Therefore multiple threads can live in their own execution space, and communicate by using data which they share.

{:.center}
![Threads](/assets/executionmodel_threads.svg){:height="350px"}

Thinking back, this was the only model I was taught in University. Actually our whole Distributed Programming course, was centered on this model. The teacher didn't really say that the shared data was a problem, but every concept of parallel programming in this model, is centered around the synchronizing that shared data between threads (locks as semaphores and monitors).

This problem with the shared data is that the threads very easily fall over each other when using it at the same time. Two threads change or read the same data in an unfortunate order or timing, and suddenly the data is in a faulty state and the program crashes, leaving typically very little help for figuring out what went wrong.

To the rescue comes language notions in the form of different kinds of locks as semaphores of monitors, but its still every easy for even the best programmers to make errors.

In University this was one of the courses which taught me how hard programming was, but actually thinking back on this now, I think I was wrong. Parallel programming in this model is difficult, but programming shouldn't be this hard. We need to have languages and execution models which help us more.

I'll be honest and say that i think languages which use the threading execution model is the absolute most difficult to code parallel programs (see my [blog post on 'static'](http://www.nettreo.com/2016/06/24/static-considered-harmful.html). And I'm afraid it wont make it easier to understand whats going on, with async-await syntax being added to most object-oriented languages.

# Event-loop in Node
The popular server-side JavaScript runtime [NodeJs](https://nodejs.org/en/), uses an event-driven, non-blocking IO execution model, which has some interesting benefits.

First of, shared data doesn't really matter, as there is only one thread to begin with. NodeJS runs your code in a single threaded event-loop, in which every IO operation (file access, network etc.) is run asynchronously. So every outgoing IO call, is called along with a callback-function, which is then called when the IO call returns.

So if you debug a NodeJS program, the call out to a database will continue along to the next thing, and execution will later suddenly appear in the callback function, when the database returns - because it happens asynchronously.

{:.center}
![Nodejs](/assets/executionmodel_nodejs.svg){:height="310px"}

The benefits of this model, is that you don't need all the bother of threads using locks to try and orchestrate using shared data sanely. There's only that single thread, and all other threads are working behind the scene asynchronously on IO. This makes Node good at handling a lot of IO fast and not that good at expensive CPU work (as it would halt the event-loop).

The asynchronous IO part is clever. What traditional threads do with synchronous IO, is that they halt and block the thread, until IO is returned. Making all this IO asynchronous, frees up resources for more work. This is also partly the reason why async/await is being pushed to other languages as C#, so that they can better argue to supports asynchronous IO. 

The disadvantage of the callback based IO as implemented in JavaScript, is though that you get a very nestled code-structure, which can be harder to understand. Another is that it that using only one thread, your not utilizing all the computers resources. The solution for this is normally to create several instances of the event-loop on the machine. Do then mind that the global data in each instance isn't shared, so if your instances need to collaborate, you need some communication mechanism.

# Actors in Erlang

Erlang is an old language and runtime, which is gaining more and more popularity, due mainly to its execution model, which is an implementation of whats called the [actor-model](https://en.wikipedia.org/wiki/Actor_model).

The main idea is that all work is focused in really lightweight processes, which only communicates via messages. Each process has its own memory, garbage collection and message inbox. And note that whereas we normally have maybe no more than a half hundred threads on a normal pc, you can have millions of Erlang-processes on that same computer. 

The way you organize these processes, is that you give some of them the role of supervising other processes that do the work (workers). This way if a worker fails, the supervising process will know (via messaging), and restart a new one to do the work.

So work done in the Erlang execution model, seem more similar to how humans work.

{:.center}
![Actors](/assets/executionmodel_actors.svg)

<fieldset class="bytheway">
    <legend class="bytheway">Elixir - another language on the Erlang VM</legend>
If you wish to look more into Erlang and its execution model, i suggest taking a look at <a href="http://elixir-lang.org/">Elixir</a>. Its a modern dynamic functional language, also on the Erlang VM, and is looking pretty interesting. 
</fieldset>

This model is nice for parallel programming, because the abstraction lends itself well to it. There can be no shared data between processes, and thus no locking problems. All communication happens by messages of immutable data (copied). and the model includes support of supervision, which helps organize all these processes in a nice way. 

Generally working in this model forces you to think about how processes interact much earlier, which by default makes you code programs that work well in parallel, instead as in the threaded model, where its normally added as a side-thought (with the pain).

The Erlang execution model performs well and makes it easy to orchestrate parallel work. For these and other reasons, I think the Erlang execution model will continue growing and gain popularity, so watch out for it.
