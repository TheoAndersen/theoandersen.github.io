---
layout: post
title: "Pull Requests are the new best practice"
comments: true
---
Code quality is not only a result of the code you write, but also of how the code is written and put into versioning. The way you tailor the process of writing code, by how you organize, name, review and build versions of your code, has much to do with the quality of the software produced.

This post is about <a href="https://help.github.com/articles/using-pull-requests/">Pull Requests</a>, which isn't a new thing at all, but a versioning workflow based on git and invented by Github, which improves on the Continous Integration practice, by enhancing isolation and improving feedback of the code written. If you don't know basic git, you will propably miss a few things in this posts - but fear not, and use a little time googling it - you'll understand it quick enough, and it will be time well invested.

<fieldset class="bytheway">
    <legend class="bytheway">Git is the new standard versioning tool</legend>
If you haven't noticed it by now, <a href="https://git-scm.com">git</a> - a distributed versioning tool created by the Linux team, has taken over versioning. Dont take my word for it but take a look at <a href="https://www.google.dk/trends/explore#cmpt=q&q=/m/05vqwg,+/m/012ct9,+/m/02rvgkm,+/m/08441_,+/m/09d6g&cat=0-5">google trends for common versioning tools</a>.
</fieldset>

Now git and Pull requests, are well known topics in the open source world. I imagine there's very few people doing open source, which are not very familiar with git and Pull Requests. It seems though that the rest of the development world hasn't quite followed along yet, which is why I'm writing this post. The following gives an overview of how Pull Requests work and what they bring to a development process, as opposed to basic Continous Integration alone. In addition we will touch on a few related workflows for working with versioning in git, as microcommitting and rewriting your versioning-history.

Now you probably stumbled a bit over me calling pull requests a best practice. A friend of mine once told me:

> "'Best practise' is the lowest common denominator - you need to do better than that, because thats what everybody's doing"
> - @hamsjaelv

So when i say that pull requests are a best practice, what i mean is that its what i see everybody use, especially in open source. It wont make you special or one the leading-edge of software development, to use Pull requests. But it sure will make you on the slow-edge if you don't even know it. 

Continuous Integration is misunderstood
---
<a href="https://www.thoughtworks.com/continuous-integration">Continous Integration</a> is the practice of integrating your local work with the shared repostory often (or continuously), to detect errors early. As thus this is a method for improving feedback, to prevent having on something broken for too long. The larger the team gets, the more important this get because its the first feedback of somebody 'breaking the build' and introducing errors into the main code-track. When breaking the build other developers working on the code, will inevitable fetch it, making theyr local setup fail as well.

This is where most implementations of Continuous Integration fails. Your process shouldn't make it possible to introduce errors which blocks the work of your teammates. That feedback should have been yours before you integrated. Some people argument that this was the idea behind Continuous Integration to begin with.

If you were to check if your work could be integrated _before_ actually integrating it with the shared repo (ill call it master, for now) - you would never have the problem of introducing errors for others on the team. Pull requests provides this feature, among other benefits.

How Pull Requests work
---
A Pull Request is a request to pull changes into a branch from another branch (possible in another cloned repository). This way the general contributor doesn't have access to write commits directly into the central repository, but can create a clone of the repository that he can work on, and then tell the owners of the central repo to pull new changes from his, by creating a pull request. The process is depicted in the diagram below.

{:.center}
![bla bla](/assets/pull_request.svg)

The process of a Pull request, as described in the diagram above. 

#### The repositories in a pull request
The setup for working with pull requests, is three kinds of repositories. Now you dont necessarily need three different repositories, but this is the most known version, as its the one used on github. 

<b>Central<b>
: The central repository is where you want the commits to end up. In the classical versioning workflow, you would simply have access to write commits directly to this repository, from your local machine.
<br /><br />
<b>Private<b>
: This is the repository where the branch with the new changes are. This repository you do have full access to write commits to, and it is from this repository you create a pull request to the Central repository. On Github, this is a forked repository you own, but in other setups this can be a specific branch on the central repository.
<br /><br />
<b>Local</b>
: The local repository, is the place on your local development machine, where you work and create your commits. In git this is also a repository.

#### The steps of working with a pull request
The process of working with pull requests is by the following steps.

1. <b>Pull changes</b><br />
To begin working on, you do what you always do when working in versioning - you update your local working repository so that your working on the latest version. This is typically a `git pull <central>` in git.

2. <b>Do Work</b><br />
Now its the time to start doing some development. Remind you that it is always best to do work on a separate branch. Remember that a branch is just a label to a certain commit in history, so its easy to change.


3. <b>Push new changes</b><br />
When your happy with the code, you `git push <new_branch> <private>` it, _not_ to the central repository (if you were to have access to it), but to your private fork. And again push it to a separate branch, just as on your local repository.

4. <b>Update/create pull request</b><br />
Once you have your changes in the private repo, you can create the pull request, via the interface of your hoster (as Github). A pull request will apear on the page of the Central repository, and the maintainers or whomever has access to it, will will review the change, give feedback and possibly merge it if everything checks out. If you already have created the pull request, pushing additional changes to the private repo will automatically update the pull request. Every time you change the commits in the pull request, a new build will be started to verify the code (it its setup on the Central repository

Now this cycle can repeat it self a few times, according to how many iterations is needed before the pull request can be merged into the target branch in the central repository.


Refactoring your versioning history
====
Most open source maintainers will want you to keep your pull request, in the form of one single commit, because it makes the history clear and easier to understand. "One single commit!?" you will probably think, "how am i ever going to get the changes right, in the first try?". But don't worry, you don't.

<fieldset class="bytheway">
    <legend class="bytheway">Microcommitting</legend>
    Due to the ability to rewrite commits in git, we commit much more often than in other versioning systems. In central versioning systems, you normally either work in a long time on something before committing anything to the repo, or you commit a lot of meaningless commits with all your errors, stumbles and fixes to the repository. In git you typically commit very often, and then use its history rewriting features to clean up the mess before pushing the commits.
</fieldset>

Git gives you the option of rewriting your commits. Git has an alternative to merging which is called rebase, which can be used to refactor the history of your commits. Now everywhere this is explained, you are told that you must not rebase (ie rewrite) commits you have pushed out of your local repository, as it can cause a lot of trouble for anybody not knowing that you just rewrote some of the commits, they also have a copy of (the old copy). But the commits you have in the Private repository which is part of the pull request, which has not been merged yet, it still yours alone. Therefore you can rebase commits on your branch and use the otherwise discouraged `git push --force` command, to overwrite the commits in the pull request with your new, rewritten, more explanatory commit.



- Reference to Test Driven Development.. first get it written, then make it nicer..

- Diagram of rebased history as opposed to merging

- When am i allowed to rebase when am i not? and why?

- 
