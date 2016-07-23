---
layout: post
title: "Refactoring your versioning history"
comments: true
published: true
image: /assets/rewriting_history_rebase.png
---
In my professional (read "closed-source") life, the versioning history of any project is a byproduct the coding of the project. What I mean is, there's not much thought about how the history reads or is structured.

This makes it hard to read and understand the versioning history, which is an important log of whats happened to the code. The following example is [XKCD's](https://xkcd.com/1296/) brilliant rendition of this.

{:.center}
![xkcd on commits](http://imgs.xkcd.com/comics/git_commit.png){:height="200px"}

The problem is that you can't use this kind of history. You can't look back at the history and understand whats going on in the system, and having an unclear picture of what has happened with the software, makes it harder to move forward in certain situations.

Beside work I always work on my own little projects. Depending on the family-life with the kids at home, it differs how much I get time to work on the projects. Some times I haven't touched a project in a week or two. Now here where the state of the versioning history begins to really matter. If I can't look at the history and get an understanding of where I'm heading with the project, I'll have a slim chance of understanding the current capabilities of software, or where it was heading when i left it. I've often found that i needed to look beyond the commit messages, and look at what really happened from the code changes - and thats annoying and very time consuming.. Most of the time I should be able to get that picture easily from just looking at the history, without having to look detailed at the code of each commit. 

The solution for this is to take more care of the versioning history, and write decent descriptive commit messages. But it's not quite as easy in my experience. Many times you're not quite sure where your going when you code it, or you find some clever other approach or get sidetracked as you work. Your versioning tool should allow you to both get sidetracked and still let you version your work as you go, and it should allow you to end up with a versioning history that makes sense for others, without all the little mishaps and stumbles on the way.

<fieldset class="bytheway">
    <legend class="bytheway">Microcommitting</legend>
    Due to the ability to rewrite commits in git, gitters commit more often than in other versioning systems. In central versioning systems, you'll normally either work in a long time on something before committing to the repo, or you'll commit a lot of meaningless commits with all your errors, stumbles and fixes to the repository. In git you typically commit very often, and then use its history rewriting features to clean up the mess before pushing the commits.
</fieldset>

The solution to this is using versioning tools that allow rewriting your history. It sounds dangerous (and it can be), but its really a powerful and cool tool, which has become really popular in distributed versioning software like git and similar.

The rest of this post will explain rewriting and rebasing history, with the context of what git's able to do. Remember to always only rewrite commits that you _own_. A rewrite (rebase) in any kind in git, will create new commits and point the history to use them instead of the old. So anybody else working further from your old commits, will be sidetracked. But if you only rewrite commits you've created yourself and not pushed to be used by others, you should be safe.

# Rewriting history to make more sense

Lets say you've coded for a while, and used your microcommitting techniques to "save" every few moments, so now you've got a history that looks like the left part of the diagram below.

{:.center}
![Rewriting a branch](/assets/rewriting_history_rewrite.svg){:height="200px"}

Now this history will not make any sense for others, and you can be sure that you wont remember the context a week from now, so what you do is you refactor the commits, before pushing them. Rewriting in git is done by using the [interactive rebase](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History) option ```rebase -i <target-branch-or-commit>```.

The first four commits (blue) except the styling one (yellow) are related to the creating of a chat page in your project. Lets rewrite these to a single commit called "Create chat page". The styling commit is isolated enough, but lets improve the commit message to "Add styling to chat page". The last two commits are centered about creating the configuration page, so lets squash these two with a better commit message.

Now didn't this make the history much simpler and cleaner? 

You decide how far you want to take this - too few commits and they get really big and incorporate alot of code changes - too few and the history gets harder to read.

I've found that I really like this way of working, where I can start out by concentrating of getting the coding part done, committing often so I can get back to a previous state if I mess up. But still having the option to make the resulting history neat, so I can understand the history more easily afterwards.


# Rebasing history for fewer branches

Using a git type versioning workflow, you typically end up with lots of branches, where everybody works in their own little branch, because branching is cheap in git. This will make the history more complex with a lot of loops and branches. 

Most gitters therefore typically rebase their work to look linear when their finished with it. 

This is actually the same git operation as we used for squashing and renaming commits, and used like this it will take a range of commits and rewrite them from another point in history. Like in the following diagram.

{:.center}
![Rebasing a branch](/assets/rewriting_history_rebase.svg){:height="200px"}

Here the normal merge, will create a merge commit, and merge the branch with the other work. Using a _rebase_ instead of a _merge_, the commits will be replayed (copied) on top of the existing, resulting in a linear more easily understood history.

# Refining your versioning history

If you use a versioning tool like git that has these options, but haven't yet really gotten into them, i suggest reading up and trying them. 

Chances are you'll have to know these techniques soon anyway, because most git projects begin to require a clean history of some sort, when people start to get more familiar with git. Also many open source maintainers will want you to keep your [pull request](http://www.nettreo.com/2016/06/17/pull-requests-are-a-best-practice.html), in the form of one single commit, because it makes the history clear and easier to understand. Github has also recently added more options to their Pull request functionality, to allow a bit of rebasing when merging a pull request.

These techniques along with microcommiting also goes along really nice with [Test Driven Development](http://www.nettreo.com/2016/06/14/TDD-Is-Just-A-Feedback-Technique.html), because it allows you to just think of solving the problem to begin with, and then refactoring it later (now also including cleaning up the versioning history).


