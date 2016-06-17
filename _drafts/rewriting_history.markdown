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
