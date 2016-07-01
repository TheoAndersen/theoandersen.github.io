---
layout: post
title: "Pull requests are a best practice"
comments: true
published: true
image: /assets/pull_request.svg
---
Code quality is not only a product of the code you write, but also of how the code is written and put into versioning. The way you tailor the process of writing code, by how you organize, name, review and build versions of your code, has much to do with the quality of the software you produce.

This post is about <a href="https://help.github.com/articles/using-pull-requests/">Pull Requests</a>. Now this isn't a new thing at all, but a versioning workflow based on git and invented by Github, which improves on the Continous Integration practice, by enhancing isolation and improving feedback of the code written. If you don't know basic git, you will propably miss a few things in this posts - but then it should be about time to start looking at it ;) -  you'll understand it quick enough, and it will be time well invested.

<fieldset class="bytheway">
    <legend class="bytheway">Git is the new standard versioning tool</legend>
If you haven't noticed it by now, <a href="https://git-scm.com">git</a> - a distributed versioning tool created by the Linux team, has taken over versioning. Dont take my word for it but take a look at <a href="https://www.google.dk/trends/explore#cmpt=q&q=/m/05vqwg,+/m/012ct9,+/m/02rvgkm,+/m/08441_,+/m/09d6g&cat=0-5">google trends for common versioning tools</a>.
</fieldset>

Now git and Pull requests, are well known topics in the open source world. I imagine there are very few people doing open source, which are not very familiar with git and Pull Requests. It seems though that the rest of the development world hasn't followed along yet, which is why I'm writing this post.

Now you probably stumbled a bit over me calling pull requests a _best practice_. A friend of mine once said this:

> "'Best practise' is the lowest common denominator - you need to do better than that, because thats what everybody's doing"
> - @hamsjaelv

So when I say that pull requests are a best practice, what I mean is that its what I see everybody use, especially in open source. It wont make you special or one the leading-edge of software development, to use Pull requests. But it sure will make you on the slow-edge if you don't even know it. 

Continuous Integration is misunderstood
---
<a href="https://www.thoughtworks.com/continuous-integration">Continous Integration</a> is the practice of integrating your local work with the shared repostory often (or continuously), to detect errors early. This is a method for improving feedback, to help avoid working on something broken for too long. The larger a team gets the more important this get, because its gives fast feedback of the integration state of the build, and makes it clear when somebody  'breaks the build' and introduces errors into the main code-track. When breaking the build other developers working on the code, will inevitable fetch it, making theyr local setup fail as well - so its nice to get this feedback _before_ others download the faulty code.

This is where most implementations of Continuous Integration falls short. Your process shouldn't make it possible to introduce errors which blocks the work of your teammates. That feedback should have been yours before you integrated. Some people even argue that this was the idea behind Continuous Integration to begin with.

If you were to check if your work could be integrated _before_ actually integrating it with the shared repo (ill call it master, for now) - you would never have the problem of introducing errors for others on the team. Pull requests provide this, among other benefits.

How Pull Requests work
---
A Pull Request is a request to pull changes into one branch from another branch (possibly located in another cloned repository). Normally the its the suitation where the a contributor doesn't have access to write commits directly into the central repository, but can create a clone of the repository that he can work on, and then tell the owners of the central repo to pull new changes from his, by creating a pull request. The process is shown in the diagram below.

{:.center}
![bla bla](/assets/pull_request.svg)

#### The repositories in a pull request
Pull requests on github normalliy happen with interaction between three repositories. Now you dont necessarily need three different repositories, but this is the most common version as i see it (you can make pull requests from a shared feature-branch on a repo, to another branch on the repo). 

<b>Central<b>
: The central repository is where you want the commits to end up. In the classical versioning workflow, you would simply have access to write commits directly to this repository, from your local machine.
<br /><br />
<b>Private<b>
: This is the repository where the branch with the new changes are. This repository is where you have full access to write commits to, and it is from this repository you create a pull request to the Central repository. On Github, this is a _forked_ repository you own, but in other setups this can be a specific branch on the central repository.
<br /><br />
<b>Local</b>
: The local repository, is the place on your local development machine, where you work and create your commits. In git this is also a repository.

#### Pull request steps
The process of working with pull requests is by the following steps.

1. <b>Pull changes</b><br />
To begin working on, you do what you always do when working in versioning - you update your local working repository so that your working on the latest version. This is typically a `git pull <central>` in git.

2. <b>Do Work</b><br />
Now its the time to start doing some development. Remind you that it is always best to do work on a separate branch. Remember that a branch is just a label to a certain commit in history, so its easy to change.


3. <b>Push new changes</b><br />
When your happy with the code, you `git push <new_branch> <private>` it, _not_ to the central repository (if you were to have access to it), but to your private fork. And again push it to a separate branch, just as on your local repository.

4. <b>Update/create pull request</b><br />
Once you have your changes in the private repo, you can create the pull request, via the interface of your hoster (as Github). A pull request will apear on the page of the Central repository, and the maintainers or whomever has access to it, will have a chance to review the change, give feedback and possibly merge it if everything checks out. If you already created the pull request, pushing additional changes to the private repo will automatically update the pull request. Every time you change the commits in the pull request, a new build will be started to verify the code (if setup on the target repository).

Now this cycle can repeat it self a few times, according to how many iterations is needed before the pull request is merged into the target branch in the central repository.

#### Dancing with branches
The benefits of pull request is that the changes get naturally boundled in a package, which is naturally 'gated' before it gets through to the target repository. In this process you get tooling support for reviewing and commenting on the code, and you get the possiblity to run all sorts of checkers on the code, before its let loose into the target repo.

Using pull requests, does though force users into using branching. Git advocates would say that its almost imposible to use git without using branches anyway, but I have found that pull request workflows work better with developers that are at least a bit aquainted to git and "dancing with branches".

