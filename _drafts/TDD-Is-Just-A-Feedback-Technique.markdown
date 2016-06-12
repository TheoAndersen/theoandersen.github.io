---
layout: post
title: "TDD is just a feedback technique"
comments: true
published: true
---
Test Driven Development, is [a book](https://www.amazon.com/Test-Driven-Development-Kent-Beck/dp/0321146530), [a technique](http://www.jbrains.ca/training/the-worlds-best-introduction-to-test-driven-development/), [dead](http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html), [no alive](http://martinfowler.com/articles/is-tdd-dead/), but very much [talked about](https://vimeo.com/68375232), [blogged about](http://blog.cleancoder.com/uncle-bob/2014/12/17/TheCyclesOfTDD.html) and [screencasted about](http://www.letscodejavascript.com).

<center>
<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/hashtag/TDD?src=hash">#TDD</a> is about design more than it’s about testing. :) RT <a href="https://twitter.com/freire_da_silva">@freire_da_silva</a>: <a href="https://twitter.com/hashtag/TDD?src=hash">#TDD</a> is about design, not testing</p>&mdash; ☕ J. B. Rainsberger (@jbrains) <a href="https://twitter.com/jbrains/status/741299918971621380">June 10, 2016</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
</center>

TDD is also more about design that it's about testing.

Now I don't think @jbrains is wrong, but it's not the whole story. If you think generally about TDD, it's just a technique which gives you fast feedback. And the answer of this feedback, telling you if the code you wrote really does what you thought. It will also make the design of your code more obvious. Both as a result of writing the tests, and due to the refactor step, where you make schedule time to review your design. 

But mainly, TDD is a just feedback technique.

Think of how you develop without TDD. You write some code, open the application and click around to test the code manually - then switch back and forth like this until you're satisfied. Now for most applications, this isn't very efficient. Maybe you need to log in each time, or you need to click in four pages to get to the page your testing. Many times you actually need to code a fair bit, before you've coded enough to make the results even visible in the application. 

With TDD you start with a test, then make the test pass - it works, feedback! - then refactor, and you continue in steps until your done. You even get to choose the size of these steps, which depends on how big leaps or steps you want to take, before getting the feedback disclosing if your code works.

So don't get fooled by all the material, making TDD a technique with complex reasoning. It may be difficult to do effectively in many applications, because we have old habits (and languages) that make it harder to work like this. But the "why TDD" is simple - Because I want fast feedback of what I'm developing, and that makes me move faster.

