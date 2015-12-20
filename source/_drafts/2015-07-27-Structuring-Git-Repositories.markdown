---
layout: post
title: "Things to consider when structuring git repositories"
comments: true
---
As of now, the world has pretty much moved to git. This especially in everything open source and the rest is following along. At my work, we're still a bit behind and are still using mostly SubVersion or TFS, though some are considering moving to git. The project I'm currently working on is one of these and at this point, we have had a few introductory meeting about finding out what it would take, to convert our pretty big TFS setup to git. Being that i have worked a fair bit with git on beside work for some years now, I feel i have a good idea of how git works and inputs to how this conversion should be done. And to start with, you simply cant structure a git project like you would a centralized VCS like TFS or SubVersion.

This post contains a few pointers on what to consider when structuring git repositories and how the structure of a git repo differs form the typical *old-school* centralized repository (centralized repositories or CVCS in this post meaning Svn or TFS).

Branches aren't in seperate folders
---
Centralised VCS models a projects history linearly. This means that branches (or tags as well in svn) are stored as *'copied'* folders of the original content.

A typical Svn folder structure could look something like this:

``` text
Project
├── Tags
│   ├── Release 1.0
│   └── Release 1.1
├── Trunk
└── Branches
    ├── Feature X
    └── Spike y
```

Notice that branches are structured in seperate folders (tags are also a kind of branch).

Git dosen't represent a branch with a copy of the folder-structure, but represents a branch internally. This means that a git repository will only have the `project` folder along with the code files under this folder and when you checkout different branches, git will change the files and folders in this folder to represent the given branch.

The branches shown in the svn example would in git show it self in the commit history - which could look like this:

``` text
*  (HEAD -> master)   Another change
| *  (Spike-Y)        Started a spike
|/
| *  (Feature-X)      Specific feature x change
|/
*  (tag: Release-1.1) Updated the code
|
*  (tag: Release-1.0) Initial commit
```

This shows that the project has had two releases (tagged as 1.0 and 1.1). On the current master (or trunk if you will) there is also a spike and a feature branch. But master is one commit ahead of both of these.

So understand that in git branches and tags arent modelled in seperate folders and you should also consider revising your branching strategy as branching simply works better in git than most CVCS. (except long lived branches, which no tooling will ever make less painfull - integrate often instead!)

Seperate projects in seperate repositories
---
A typical pattern used in CVCS is that you keep everyting in one large repo and divide subproject, tools etc in seperate subfolders.

This could could look something like this:

``` text
MyProject
├── Server
├── OldClient
├── NewClient
├── OldSubprojectYouForgotWhatWasAbout
└── Tools
    ├── Tool A
    └── Tool B
```
This is one of the typical old style 'contains-all' repo's which contains 'everything' (ex. that old application-server that came from the previous supplier which nobody knows about or uses etc.). My point is that these repos become really big and unwieldy - remember that the whole structure (or parts of it) is also copied out in branches or tags, which means that the whole structure is copied under the subproject folders for each branch.

A typical beginner-error new developers do in these projects, is that they start out by fetching everything in the repo (get-latest from *'~/root'*). Then when asking how big the repo is (because the fetch is taking forever), the experienced developer laughs a bit and responds that you almost never need anything else that just this sub-sub-folderstructure here in this branch and this branch, pointing out the 2% of the repo that the new developer should focus on.

Now in git you really don't want to to keep everything in one repo like that. The notition of a git repository is different than its centralised counterparts. You can think of git as a more efficient but much more focused VCS. A git repository is ment to hold the versioning of one project. This means that every commit is a snapshot of the entire repository, and can't be specific to only a folder as is possible in TFS or subversion. A branch is also a branch of the entire repository.

This means that you want to think a bit more about the relationship between the subprojects (folders) you have in the CVCS in regards to how to want to structure them i a git repository (or several git repositories.

If you put everything in one big git repository you would most likely get a large slow git repository, in which a commit to a little tool would be indistinguishable to a change to the server part (its in the same repository).
Where we normally keep everything in the same repo in CVCS, we seperate what can b seperated using git. This is where a repository manager normally comes into play (like Github, Atlassian Stash and now also Visual studio online) keeping all the project related git repositories in one place and adding more functionality on top of the bare versioning (git) as Configuration Management fx.

##### pro's / cons to seperate repositories

- If the two repositories have dependencies to each other you have to do work to find versions of each repo that work together
+ Changes in one repository dosen't directly affect the other repository
+ Smaller/faster repositories

... example of git reposotries from the current example

... suggestion from submodule to subtree.. it seems


References
----------
<ul>
  <li><a href="http://programmers.stackexchange.com/a/178121">Jan Hudecs answer on stackexchange</a></li>
</ul>
