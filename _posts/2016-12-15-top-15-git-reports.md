---
layout: post
title: 15 easy git reports from command line.
comments: true
---

Again, it has been too long since my last post. Life happens, priorities shift, and quite frankly I am bad at "task" management. So, in order to get this blog lively again, let's start at the beginning - a post on Git.

It is easy to say that Git is the leading source versioning software out there. Therefore, getting more in tune with the system can be of great benefit. After learning the basics of Git, such as staging and committing, another great tool to have in your toolbox is how to retrieve repository history aka logs.

Now, I know what you are thinking, GUI tools like SourceTree and GitKraken provide history tracking graphs already - Indeed they do. I would be lying to you if I said I don't use them, specifically for that reason. However, what I would like to show you is how they create those graphs and give you additional history commands that you can use hand in hand with your favorite Git GUI.

-----

# the problem.
You are using your favorite Git GUI and you are curious - what commands is this GUI running to illustrate this Git history? Moreover, you wonder - is there additional information that I can acquire that the GUI does not provide me?

# the solution.
From the git console alone, there are various logs and history that can be retrieved. Commands like "log", "reflog", and "branch" with its plethora of parameters provide such data. Listed below is a section for each command and the type of logging reports it can get.

## git reflog
The reflog, aka reference log, is arguably the most under utilized but surprisingly informative log a user can see. These records solely keep track of when the tips of branches are updated in the local repository. 

When using just "git reflog" by itself, by default it will execute "git reflog show". Oddly enough, "git reflog show" is actually an alias to "git log -g -abbrev-commit -pretty=oneline" - so a pointer to a pointer. 

Unfortunately, there is not much options to the reflog but listed below are some neat logs/reports it provides.

* git reflog - as stated before, this will show everything in regards to the tip of the local repository.
* git reflog show <reference> - will show all the reference based on the log options you input, i.e. "HEAD@{one.week.ago}", "master@{one.year.ago}", or "branch@{three.days.ago}".
* git reflog --all - will show all the references to the local repository, including others branches and the stash.

## git log
The log command, accessed by simply typing "git log", is the gateway to repository history. Out of the box by executing "git log" without any parameters, the user will see a list of recent commits and for each commit they will see the associated SHA, author (name and email), date, and commit message. However, "git log" comes equipped with a plethora of parameters and with those parameters a copious amount of combinations.

Listed below is just a small sample of what git log can do and how information can be displayed with certain parameter combinations.

* git log --oneline --decorate --graph - shows the last ~25 commits all on oneline (--oneline) with where each branch (local or remote) is pointing (--decorate) and where each branch has diverged and where it has merged into (--graph); this is honestly my favorite command to call and I always ensure this is setup as an alias.
* git log --format=short --max-count=3 --stat --unified=3 - shows the previous 3 commits (--max-count=3) with their SHA, author, and commit message (--format=short), the number of lines additions, deletions, file renames, and file deletions (--stat), and retrieves a diff for each file changed in each commit "with <n> lines of context" (--unified=3).
* git log --patch <path to the file> - shows all the commit history of a given file with each commit showing an illustration of the diffs (--patch).
* git log <branchToCompareTo> ^<branch> - shows all the commits that reside on the <branchToCompareTo>, or the left branch, that are NOT in the branch prepended with ^, or the right branch.
* git log --author=<pattern> - shows all the commits by an author that matches the pattern.
* git whatchanged - is basically git log under the covers but by default it shows "the raw format diff output and [skips] merges" or to put it shortly with the git log --raw parameter passed.

Remember, the examples above barely even scratch the surface of the data that can be retrieved and how it can illustrated from the repository history.

## git branch
The branch command operates specifically on branches - plain and simple. For logs/reporting with branches, parameters can be passed illustrate the branch information of a repository.

* git branch --list - will show all the **local** branches (which are denoted in green).
* git branch --remote - will show all the **remote** branches (which are denoted in red).
* git branch --all - will show all the branches, local and remote (green for local and red for remote).
* git branch --contains <commitSHA> - will show all the branches that contain a commit by SHA. This can be useful when determining if a branch contains a commit that was necessary for a hot fix.
* git branch --merged <branchname> - will show all the branches that have been merged into the specified branch - greatly useful when determining if a branch is eligible for deletion.
* git branch --unmerged - will show all the branches that have not been merged - just as useful as the command above when determine what needs to be merged.

# reference links.

[git reflog](https://git-scm.com/docs/git-reflog)
[git branch merging](http://stackoverflow.com/questions/226976/how-can-i-know-in-git-if-a-branch-has-been-already-merged-into-master)
[git whatchanged](https://git-scm.com/docs/git-whatchanged)
[git log](https://git-scm.com/docs/git-log)
[commit differences between two branches](http://stackoverflow.com/questions/7566416/different-commits-between-two-branches)
[git tips](https://github.com/git-tips/tips)

-----

All in all, I hope this post helps show the many possibilities that come from the command line and how some of our favorite GUIs operate. After all, the git GUIs are simply using git commands "under the hood." Like always, if you have any questions, comments, or concerns, please share them below.