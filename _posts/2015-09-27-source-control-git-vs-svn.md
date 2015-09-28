---
layout: post
title: source control - git vs svn.
comments: true
---

First of all, it has definitely been a long time coming for a blog post. It is creeping up on almost a year since the last post. However, it is better late than never.

In the past year, I've had the opportunity to work closely with subversion. At first, I knew that I could use some of my git background with subversion by utilizing the [git-svn](http://git-scm.com/docs/git-svn) bash commands. The git-svn commands were great when the workflow for the repository was linear and simple. However, when it came time to branch and merge with the remote repository, the git-svn commands "lost their luster." To be fair, the git documentation completely discourages branching/merging using git-svn for a branching workflow. The reason being the subversion server will combine all commits to one commit, will not notice any branching/merging done locally, and sometimes the wrong branch would be committed (depending on the branches on the remote server). After experiencing this one time, I decided that the git-svn commands did not grant me full control of my subversion workflow. Plus, in order to become a viable source control resource to my team, it is beneficial to use the same tools (aka svn and not git-svn).

With the time invested in both git and subversion, I decided this would be a great opportunity to analyze which one I believe grants the better source control experience.

-----

# the problem.
A development team has tasked themselves with migrating from an older source control system.
The older source control system has been deemed too cumbersome to learn and requires too much time to interface with.
The development team has ruled out TFS (Team Foundation Server) due to costs. They have boiled it down to subversion or git.

The development team is looking for a source control system that will have a rich server UI experience. The UI must enable code reviews, notify developers of code changes, and allow certain users access to repositories or branches within the repository. The development team is also looking for fluid integration with Visual Studio. Moreover, the team is looking to work from local, non-conflicting production copies of the source code for dynamic workflows to enable quick flexibility for planned and unplanned requests.

# the solution.
In this scenario, Git is the clear choice. Subversion satisfies some of these requirements while Git satisfies all of these requirements.

Disclaimer - this will not always be the scenario when determining the source control/version system best suited for a development environment. However, after choosing a system, new source control requirements can arise, making the chosen system moot. As new requirements arise, developers notice, with research, that other systems are more suitable. This really stresses how invaluable proper research is before selecting not just a source control system but any tool or system.

Now, let's begin the comparison of Git and Subversion given these desired requirements.

## user interface.
The user interface can be pinned down to two sides: client and server. In terms of client, git and subversion provide the same tools. They both provide a command line and GUI option. Majority of users tend to use [TortoiseSvn](http://tortoisesvn.net/) or [TortoiseGit](https://tortoisegit.org/), but there are other GUI's wrapped around Git that might give Git the nod in the client side interface category (i.e. [SourceTree](https://www.sourcetreeapp.com/), [GitEye](http://www.collab.net/products/giteye), [GitExtensions](http://gitextensions.github.io/), and many more).

For server side ui, Git wins this category by a long shot. Tons of user interfaces exist, that will nicely wrap around a Git repository (i.e. GitHub, BitBucket, GitLab, and more). As for subversion, [VisualSVN](https://www.visualsvn.com/server) seems to be the only choice. Both of these user interfaces will show the repository file/folder structure, markdown files, and commit history. However, the Git user interfaces add much more with graphs (showing contributors, commit time line, amount of source changes, owners with their commits, etc), issue tracking, zip downloads, pull requests, review discussions, server side file CRUD options, project documentation sites, server side tagging, repository configuration, and more.
	
## code reviews.
As stated above, the server side interface for Git comes equipped with code reviewing functionality. Unlike subversion, Git's server side user interface allows users to comment on commits with or without a pull request.

## source code change notifications.
For subversion, source code change notifications can be implemented with hooks or communicated via e-mail or verbally. For git, this can happen with a pull request. Pull requests are best used for code reviews . The pull request notifies all or certain individuals that a branch (master or not) has been updated and is available for a "git pull". This could be for a branch that is controlled or not controlled. This is typically the time in which developers will converse, comment, and review the commit.

For me, if things can be automated, let them be automated -- allow a pull request to notify me when any source has changed. This category is a push because this is based on user preference.

## visual studio integration.
In Microsoft's Visual Studio solution, they have openly embraced git and all of its features. They enjoy git so much, they have included git as a staple in their source control solution TFS [(Team Foundation Server)](https://msdn.microsoft.com/en-us/Library/vs/alm/Code/git/overview). In order for subversion to integrate with Visual Studio, users can go the free route with [AnkhSVN](https://ankhsvn.open.collab.net/), [TortoiseSVN](http://tortoisesvn.net/visualstudio.html) or purchase a license for [VisualSVN](https://www.visualsvn.com/visualsvn/). 

It can argued that neither source control system holds the advantage in this category but with Microsoft's public approval, git should get the advantage here.

## user access.
For both systems, all user access is controlled server side. This means for both project specific and branch specific access. However, not all user interfaces come equipped with the capability to manage these permissions. The ones equipped with this capability can be managed by a development manager or team lead. The ones unequipped have to be managed by a server admin.

User interfaces for git come fully equipped with this functionality (aka [BitBucket](https://confluence.atlassian.com/bitbucket/branch-management-385912271.html) or [GitHub](https://help.github.com/articles/defining-the-mergeability-of-pull-requests/)). Whereas for subversion, only server admins can control access natively at a repository level. However, to easily manage access at a branch level, a separate [tool](http://blogs.collab.net/teamforge/wildcard-access-control-and-path-based-permissions-in-teamforge#.Vf9T_xFViko) must be installed (this will provide a Regex implementation). 

In this category, the clear "thumbs up" of approval has to go to git. 

## planned workflow. branching, tagging, and merging.
When work is planned, either system will do the job perfectly. Everything can go to the trunk or master with no problems or quarrels about functionality conflicting in a release. However, nine times out of ten that is not always the case. Both systems provide the capabilities of branching, merging, and tagging. Both systems accomplish this workflow in different ways in terms of implementation and ease of use. 

The first thing to note are branches and where they are stored. In git, branches begin as local branches, meaning the server does not know about them - this is due to git's decentralized nature. Also in git, branches are not direct copies of the master/trunk, but represent the file differences between the master and its current state. Subversion is the opposite, but does not imply this is a negative - depends on user preference. In subversion, every branch that is created has to be committed to the server. Moreover, from a client perspective, every branch that is created/pulled is a complete copy of the trunk (meaning the total size of the repository will be equal to the size of the trunk times the number of branches/tags).

Second is tags. In both git and subversion, tags represent the repository at a certain place in time. In git, this is very much like a pointer. The tag will point to a commit that represents that place in time. In subversion, [tags](http://svnbook.red-bean.com/en/1.7/svn.branchmerge.tags.html) are "no different" than branches.  When pulling tags (and branches) in subversion, each tag will download a complete copy of the repository's trunk at that "tagged" place in time.

Last is merging. Depending on how the repository is setup, merging in subversion can be done by project tree or range of revisions. In my experience, if there are repositories within repositories, merging by project tree can come with some "gotchas", i.e. if new files or folders are added to the main branch after the branch was created, those files or folders will get lost when merging the branch back into the main branch. At this point, the user has two options: merging by revisions or merging the main branch into the merging branch and then merging the branch back into the main branch. In git, there can be a learning curve in terms of merging. For example, it is best practice to do "git fetch" to obtain any commits that your local working copy may not have before merging. The reason being is, if a developer happens to forget to pull down new commits and merging locally has already started and it warns the developer that you are behind the "HEAD" you can only "git push --force", the developer has the chance of overriding the remote repository's history.

In this scenario, merging has to be used with extreme caution for both systems. However, branching and tagging should not be so expensive in terms of repository size and time to download. Although, it can be argued that some users want the remote server to know everything that is going on with repository. However, in my opinion with subversion's heavy nature and my tendency to want to branch on a whim, git gets my approval here.

## unplanned workflow.
In every development environment there will be times when unplanned work sneaks in. This tends to put the developer in a pinch when in the middle of a request. Typically, the unplanned work will take precedent. Changing directions elegantly and efficiently should not be a burden. This is where a versatile source control system can make this easy.

In subversion, this type of situation requires some finesse to save current work in progress. For unplanned work, the developer can save their work by creating a [patch](http://tortoisesvn.net/docs/nightly/TortoiseSVN_en/tsvn-dug-patch.html) or [branch](http://stackoverflow.com/questions/1554278/temporarily-put-away-uncommited-changes-in-subversion-a-la-git-stash). In git, the developer can also use either branches or [patches](http://git-scm.com/docs/git-format-patch). Again, this is entirely up to developer preference and can depend on how long the unplanned work will take. Another option in git is [stashing](https://git-scm.com/book/en/v1/Git-Tools-Stashing). Stashing provides developers a mechanism to save "the dirty state of their working directory." With stashing, the developer at any point has the option of "popping" any index off the stash on any branch. This is great for saving unfinished work to be finished later.

When it comes to unplanned work, git and subversion match up almost identical. However, with an extra tool for developers to use with stashing, git gets win.

## rewriting commit history.
Every once in awhile, there comes a time when re-writing repository history is a must. Sometimes author names and commit messages need to be edited, commits need to be removed, or commits need to be rearranged. For subversion, removing commits is easy with reverting. Now, this does not completely remove the commit from the repository history but the revert commit will denote that a certain commit's changes have been reversed. In order to edit author names or commit messages of a subversion repository, hooks have to be enabled on the server. Unfortunately, in subversion there currently isn't a mode to rearrange commits. For git, all of these scenarios can be accomplished with [git rebase](http://git-scm.com/docs/git-rebase). Git rebase allows developers the option to change commit messages, authors, and commit history (for easier git commit changing see [git commit --amend](https://www.atlassian.com/git/tutorials/rewriting-history/git-commit--amend)). Git's rebase operates differently than subversion's revert. Where subversion keeps the commit but reverses it, git completely removes the commit from the history.

Depending on the developer's preference, subversion or git can be a viable commit history writing tool. Some would say they would want to track when a commit needs to be deleted - this is where subversion is the clear choice. On the other hand, if the developer needs to change a commit author or message and they do not have access to enable subversion hooks, git is the clear choice. Then in the last scenario, if at any point the commits need to be rearranged to couple certain functionality for a release, git is the only choice. Since this category is scenario and preference specific, this is a push.

## project ignores.
When setting up a repository it is best to ignore certain files and folders. Both subversion and git provide this capability respectively with their global-ignores and .gitignore. Each system will allow you to ignore specific files by name while using or not using a regular expression wildcard. The only difference between the two comes when ignoring folders. For example, ignoring the compiled output folder of a project, say the bin folder in a .NET project, can be done by ignoring that folder in the global-ignores or .gitignore. However, if at any point another bin folder gets introduced to the repository, git will continue to ignore it (because all bin folders were specified to be ignored) but in subversion the newly introduced bin folder will have to be specified in the global-ignores. So, if for any reason this new folder gets committed with the non-versioned items, the repository will contain compiled source (which is a big no no).

Based on this occurrence, git has the edge.

## creative nature.
Not always when developers sit down and start coding will they have the luxury of network access (i.e. the internet provider is down or the vpn is unreachable). However, still in this setting coding must be done. When it comes time to commit, depending on the source control system the developer will be able to or NOT be able to commit. Due to subversion's centralized nature, a connection the remote repository is required when committing, creating branches/tags, merging, or changing repository history. With git's distributive nature, a connection is NOT required when committing, branching, tagging, merging, rewriting, etc when working on a project. When using git, every developer gets their own local copy of the project in which they can execute any commands without any communication to the server. This helps promote creative nature in terms of smaller, modular commits (and prevents large, irreversible commits) for the project repository. Without a doubt, git takes this category.

-----

Hopefully, this post will help speed up the research for any development team when selecting a new source control system.
If you have any questions, comments, or concerns, please feel free to share them below.