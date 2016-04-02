---
layout: post
title: "Pull Requests are the new best practice"
comments: true
---
Code quality is not only about what code you write, but also about how the code is written and put into versioning. The way you tailor your process of writing code, by how you organize, name, review and build versions of your code, has much to do with the quality of the software you produce.

<!-- One of the keys to this is feedback. Good feedback is critical, if you want to develop the right thing fast and in good quality - and of course it is. How will you know if the software works, if you don't test it? If you think about it, many software methodologies have their roots in providing better feedback; Testing, Test Driven Development, Continous Integration, Reviewing and so on. -->

This post is about Pull Requests, which isn't a new thing at all, but a versioning workflow based on git and invented by github, that improves on the old standard of Continous Integration, by enhancing the isolation and improving the feedback of the versions of developed code.

<fieldset class="bytheway">
    <legend class="bytheway">Git is the new standard versioning tool</legend>
If you haven't noticed it by now, git - a distributed versioning tool created by the linux team, has taken over versioning. Dont take my word for it but take a look at <a href="https://www.google.dk/trends/explore#cmpt=q&q=/m/05vqwg,+/m/012ct9,+/m/02rvgkm,+/m/08441_,+/m/09d6g&cat=0-5">google trends for common versioning tools</a>.
</fieldset>

Now there's very few people doing open source, which are not pretty familiar with git and Pull Requests, but it seems the rest of the development world hasen't quite followed along yet, which is why I'm writing this post. The following should give an overview to what Pull Requests bring as opposed to basic Continous Integration, along with some of the related workflows for working with versioning in git, as microcommitting and rewriting your versioning-history.

Contionus Integration is misunderstood
---



How Pull Requests work
---


These will affect its quality on the line with how the actual code is written.

- What is this post about, and why am i supposed to read it? What is a pull request

  Quality in code is not only about the code but also about the process involved in getting the code written




- Why are you talking about best practices (Klaus quote)

> "'Best practise' is the lowest common denominator - you need to do better than that, because thats what everybody's doing"
> - @hamsjaelv

- How does a pull request work?
  
  
- [Diagram of a pull request flow]

- Explenation of the steps in more detail

<fieldset class="bytheway">
    <legend class="bytheway">Microcommitting</legend>
</fieldset>

- About keeping the history nice

Refactoring your history
====
(rebasing)

- Reference to Test Driven Development.. first get it written, then make it nicer..

- Diagram of rebased history as opposed to merging

- When am i allowed to rebase when am i not? and why?

- 
