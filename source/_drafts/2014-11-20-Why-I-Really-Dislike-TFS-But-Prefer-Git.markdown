---
layout: post
title: "Why i prefer git to TFS"
comments: true
---
Being a professional .NET developer i've used TFS pretty much since the SourceSafe days (along with the occasional subversion). I've though used git a lot on side projects and at home. If you're only familiar with TFS and/or subversion, you have no idea what your missing out on and you might be hurting a lot in areas as branching more than you have to.

Im currently a part of a project with many branches and many developers in TFS and am expeciencing alot of that hurt, that i would think less painfull if we were using tools as git instead. I actually think that a lot of the bad reputation branching and merging has, is partially because of the pain caused when using tools that's got it hacked in - such as TFS.



<!-------------------------------------------------------------------------------
<!-- Quick summary - Git vs TFS                                                  
<!-- --------------------------                                                  
<!-- |                 | =GIT=         | =TFS=                                 | 
<!-- |-----------------+---------------+---------------------------------------| 
<!-- | Branching       | project-based | folder-based                          | 
<!-- |-----------------+---------------+---------------------------------------| 
<!-- | local branches  | Yes           | No                                    | 
<!-- |-----------------+---------------+---------------------------------------| 
<!-- | Most operations | works locally | only when connected to the TFS server | 
<!-- |-----------------+---------------+---------------------------------------| 
 | VCS model       | Distributed   | Centralized                           | 
-------------------------------------------------------------------------------->

Git is based on a better model than TFS
-----------------------------------------------------------
Whats possible in different versioning systems and how one uses them has everything to do with the structure they use (or the model if you like) - this is especially true for their support for branching.
Git's model is based on an acyclic graph (paths can divide and converge, but not go back in circles) of changes originating from one first commit. A branch is here merely is a pointer to one place in this graph.

In TFS changes are oneway/linear. A branch in TFS is a copy onto another folder location which then gets its own linear history, but you can merge (sort of copy) content from one to another. TFS keeps track of which changes you are missing from one branch to another - Actually we have had this go wrong several times with our TFS server, where we have had to mark older changes as merged again in tfs. This shows that TFS really sees the changes as seperated in two different locations. In git brances really just pointers to locations in some code-bases history - on the same line of changes, not in separate locations.

A merge is always a squash in TFS, as opposed to git
----------------------------------------------------
(because we copy all content thats missing in one branch to the other one and put then in a new commit (marking them as Merged (meaning no change), merge edit.. etc.

In git a merge is the joining diverged paths into one - meaning thats its no copy of code from a branch to another - but the actual commit has changed content it self if there was no conflicts in the merge, why should there? All the info of the two branches is in the diverted paths before the merge.

In git a merge is never a squash per default - actually in git a merge dosen't even have to end up in a new commit. If they result of merging branc A onto branch B means that A should just contain the same exact contents as Branch B, then it just makes whats called a "fast forward" merge and changes A to point to the same commit that B does.

Points to investigate
---------------------
- A TFS Merge squashes commits to one new one created in the new branch
- Annotate/Blame
- Rollback one or multiple items
- Cherry pick commits when merging
- "Baseless" merges (is there such a thing in git at all - dosen't it just work?)
- (new) Ability to easily go forward/backwards with versioning of a file (+ keep position)

- Points that i can do in TFS, can i do them in git? how
- Points i can in git but cant in TFS
- How do i identify code versions that has been changed the most.. (files at first)
- Finding merge errors across branches is painfully difficult in TFS
- You can get Merge errors if you forget to get-latest before merging

More about Microsofts comparisons of Git vs TFS
(much of it is based on Microsofts use of git - not git it self)
http://msdn.microsoft.com/en-us/library/ms181368.aspx#tfvc_or_git_summary
