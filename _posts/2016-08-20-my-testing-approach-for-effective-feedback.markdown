---
layout: post
title: "Choosing testing approachs for effective feedback"
comments: true
published: false
image: /assets/hybrid_mobile_app_architecture.png
---
This post is about testing - testing is fun. At least the automated kind is. I've never understood why some developers don't like testing. You program it right? programming is fun? And you need to design, refactor and cleanup tests-code just as much as production code - but this is not exactly what this post is about.

This post is about choosing testing approach's, to get the right feedback and optimize development. I've written previously about testing and TDD, and I'm a pretty enthusiastic TDDer. But even though, I don't always want to be TDDing at unit testing level - especially not from day one of a project - why? - Its too much work, and if you do all your testing at unit testing level, you're strangling your application by making the internals of your application, depend on the tests - this makes refactoring and working with the code more difficult, as you cant change it without breaking tests. 
Now I'm not saying that I don't want to do TDD at all at unit testing level, but I want to be smart about what level I test at. If you want to dig more in to this TDD Anti-pattern, you should go watch Ian Connery's [TDD where did it all go wrong](https://vimeo.com/68375232) conference video. And no, he isn't a TDD opponent either.

In this post I'll explain testing choices, in the example of a hybrid mobile application. I've been developing hybrid mobile apps a bit lately, and this kind of architecture is interesting, because the architecture enables multiple available approaches to testing, which we can evaluate as we see it giving the best feedback.

Basic architecture of a hybrid mobile app
------

{:.right}
![Open a project in emacs](/assets/hybrid_mobile_app_architecture.svg){:height="500px"}

Hybrid mobile application, is a general term for an application which you develop in one way, and which will run on multiple mobile platforms. This means that you develop the application in one way, and then can build it for both ios and android ex (Windows Phone is pretty dead right?).

There are frameworks out there, which cross-compile to native code - but in my experience they pretty much require you to know much about how you would develop the applications in native code, before you can effectively use it. Its more approachable to use a web-based hybrid framework, which is what I'm using. 

I've settled in Cordova/Ionic 2, which is one of the more popular web based hybrid app frameworks. Ionic 2 makes it possible to create Angular 2 applications, which are build into a web-view in the browser of a phone, and then styled a bit to suit the platform. This is not good enough for games and applications requiring a lot of fancy native performance and graphical work, but its fine for most other tasks.

The architecture of an Ionic application is thus based upon an application written in Html and Javascript, where the Javascript is a single page application based on Angular 2 (and typescript). The application is hosted in a web-view inside a template native application which is pretty bare bones, leaving the real application being the Angular 2 application. Though if the SPA needs to use a native api, it can use special Javascript api's to access and utilize these. Cordova on which Ionic is build, contains a library of these plugins, and you can develop your own if you need to.

Looking at the testing perspective, this makes it possible to create automated tests on several levels, as ex:

- Unit tests - basic JavaScript (parts not Angular dependant)
- Unit tests - including Angular
- End 2 end tests - web
- End 2 end tests - device

In the following sections, I will explain the path i took, when i started developing in this architecture.

Lets start out making a mess
------
Starting out I don't want to be held up fiddling with tests too much. Beginning a new project, we want to be able to try out a few directions before we settle. So I'll just start out coding, and trying different approaches.

This is a technique called spiking. We're purposely waiting on the nice design and the tests, to just try something out and help us figure out where we want to go. This can be to check out how some api works, or what design will work the best.

Now normally when i do a spike, i do it in a separate directory called ```spikes/``` and i start over on the real project, when I'm done exploring. 

In this example i didn't bother making a separate spikes/ directory, as i was just getting started, and it was a pretty blank slate to begin with. So i tried out some different UI layouts, and added a list and some settings working on stubbed data.

Beginning to need some assurance here
-------
Now we've played around a bit. We now have a better idea of where we want to go. But we are beginning to realize that playing time is over. The code is growing steadily, and theres some functionality in place, we want to secure in some way. We need some kind of assurance that we wont break the quick simple flow that we added while spiking, when we continue to add functionality. 

We need our first tests.

Now, we know that we have different kinds of tests to choose between. Lets consider what we are afraid could go wrong. We haven't really gotten any clever code in the system yet, nothing is calculating anything complex. The application is currently just a few pages with navigation and selecting from lists and maybe configuring some settings. 

So this wouldn't make any sense unit testing. Unit tests are to specific - too small scoped. Actually I'm not sure how i would unit test any of this. Navigation and settings, are only DOM elements being triggered which triggers an angular function, that in turn uses angular internals to change the page or save some settings. No, we need something larger scoped, which doesn't stub out the very parts we want to assure. 

Because of these reasons, I start out using an end to end approach. Its a little more involved to use the version where we actually test the real device (using the simulator), and its pretty straight forward to just use the served version, so this is the one we will use.

Most web end to end frameworks use Selenium in some way, and so do I here. ()[protractor] is an end-to-end test framework for Angulra which uses selenium under the hood. This is fine for the purpose and provides some of the default setup for Angular. This way we can write javascript based tests, which instruments a browser to open the hybrid app as a well page, and navigate around. It can even be instructed to take a screen shot if something goes wrong.

With this in place we code some basic tests confirming the page flows and settings work as they should. With the basic assurance we needed in place, we move on.

Okay this is getting complicated... unit tests please
----
The end-2-end tests are great for the testing the overall flow, and settings. But they become a bit large-scoped if you have some nitty gritty algoritm or some a bit more complex logic you need to get to work.


Working a bit more with dates - I really hate the poor date support in javascript and typescript - or am i missing something here?

Lets set it up.

Ahh. much better for these complex parts.. but it dosent help me much at all with the big picture

Getting feedback on the big picture
-----

Native plugins are causing problems.. the api's are a bit tricky..

We need larger scoped tests..

This is actually my next place to investigate..

Choosing testing strategies at the right scopes
------



