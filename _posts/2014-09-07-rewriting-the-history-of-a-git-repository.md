---
layout: post
title: rewriting the history of a Git repository.
comments: true
---

[Git](http://git-scm.com), the free, open source version control system (or source code management system). 
All developers, young, old, new, and experienced, will cross paths with Git. 
Unfortunately, when we are learning something new, we tend to make newbie mistakes. 
One of the most common mistakes made by developers when utilizing Git is committing unnecessary files and consequently bloating the repository.

-----

# the problem.

A developer has not created a gitignore file and mistakenly commits a build file, maybe a jar file or the whole bin directory, to the remote repository.
As time elapses, the repository's size becomes larger and larger.
Since an inappropriate file was previously committed, the total size of the repository has grown from simple KBs to tens and possibly hundreds of MBs.
The size needs to be reduced because simple commands like cloning and pulling are taking too long to complete.

# the solution.

First, the old commits will need to be "cleaned" locally. 
If any of the commands below look unfamiliar, use the --help parameter to learn more about the command.

1. Obtain a fresh copy of the repository to be cleaned - `git clone <repositoryUrlHere>`
2. Remove the specified file or directory from the entire repository history - `git filter-branch --tag-name-filter cat --index-filter 'git rm -r --cached --ignore-unmatch <file or directory to remove>' --prune-empty -f -- --all`
	* `filter-branch` - this will filter the branch specified. Use --all to filter all branches.
	* `--tag-name-filter cat` - Rewrites all tags. Recreates every tag that points to a rewritten object.
	* `git rm -r --cached --ignore-unmatch <file or directory>` - Remove the specified file or directory from the repository's history.
	* `--prune-empty` - Notifies git to ignore empty commits.
3. Expire the references for the repository right now for all branches - `git reflog expire --expire=now --all`
4. Run the garbage collector to remove dangling, unreferenced objects - `git gc --prune=now --aggressive`
	* `--prune=now` - instead of using the default, which is typically two weeks, prune all loose objects older than now.

Second and lastly, the old commits now need to be "cleaned" remotely. 
The reason for this is, garbage collection has only ran on the local repository. 
Now, it needs to be done on the remote repository as well.
Primarily, git servers run `git gc` on a schedule.
So, technically this process could wait until that time has elapsed.
However, if immediate results are required, there are two ways this can be achieved.
Depending on the git server, if it is GitHub, BitBucket, or some other external source code management system, an email can be sent to the appropriate parties asking for garbage collection to be ran on a certain repository.

If the git server is an internal server, the follow commands can be executed from the repository source directory.

1. Obtain the object, size, objects in pack, the number of packs, the size of the pack, and how many objects are to be collected - `git count-objects -v`
2. Much like the last step for the local repository "clean up", run the garbage collector to remove dangling, unreferenced objects for the remote repository - `git gc --prune=now --aggressive`
3. Once the garbage collection is complete, run `git count-objects -v` again and notice the difference before and after the garbage collection occurred.

After these simple steps are completed, your repository should be less bloated and more organized for the next person.

# reference links.

Attached are some of the links I used when I first encountered this problem.
Hopefully, if my blog post does not make sense then the links will help.

* [http://git-scm.com/book/ch6-4.html](http://git-scm.com/book/ch6-4.html)
* [http://stackoverflow.com/questions/2164581/remove-file-from-git-repository-history](http://stackoverflow.com/questions/2164581/remove-file-from-git-repository-history)
* [http://stevelorek.com/how-to-shrink-a-git-repository.html](http://stevelorek.com/how-to-shrink-a-git-repository.html)

-----

Overall, I hope you find this post helpful in your next Git adventure.

If you have any questions, concerns, or suggestions, please share them below.