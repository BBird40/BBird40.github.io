---
layout: post
title: source control - git vs svn.
comments: true
---

First of all, it has definitely been a long time coming for a blog post. It is creeping up on almost a year since the last post. However, it is better late than never.

In the past year, I've had the opportunity to work closely with subversion. At first, I knew that I could use some of my git background with subversion by utilizing the [git-svn](http://git-scm.com/docs/git-svn) bash commands. The git-svn commands were great when the workflow for the repository was linear and simple. However, when it came time to branch and merge with the remote repository, the git-svn commands "lost their luster." To be fair, the git documentation completely discourages branching/merging using git-svn for a branching workflow, the reason being the subversion server will combine all commits to one commit, will not notice any branching/merging done locally, and sometimes the wrong branch would be committed (depending on the branches on the remote server). After experiencing this one time, I decided that the git-svn commands did not grant me full control of my subversion workflow. Plus, in order to become a viable source control resource to your team, it is beneficial to use the same tools (aka svn and not git-svn).

With the time invested in both git and svn, I decided this would be a great opportunity to analyze which one I believe grants the better source control experience.

-----

# the problem.
A development team has tasked themselves with migrating from an older source control system.
The older source control system has been deemed too cumbersome to learn and requires too much time to interface with it.
The development team has ruled out TFS (Team Foundation Server) due to costs. They have boiled it down to svn or git.

The development team is looking for a source control system that will have a rich server UI experience. The UI must enable code reviews, notify developers of code changes, and allow certain users access to repositories or branches within the repository. The development team is also looking for fluid integration with Visual Studio. Moreover, the team is looking to work from local, non-conflicting production copies of the source code for dynamic workflows to enable quick flexibility for planned and unplanned requests.

# the solution.
In this scenario, Git is the clear choice. Subversion satisifies some of these requirements while Git satisifies all of these requirements.

Disclaimer -- this will not always be the scenario when determining the source control/version system best suited for a development environment. However, after choosing a system, new source control requirements can arise, making the chosen system moot. As new requirements arise, developers notice, with research, that other systems are more suitable. This really stresses how invaluable proper research is before selecting not just a source control system but any tool/system.

Now, let's begin the comparison of Git and Subversion given these desired requirements.

## user interface.
The user interface can be pinned down to two sides: client and server. In terms of client, git and svn provide the same tools. They both provide a command line and GUI option. Majority of users tend to use [TortoiseSvn](http://tortoisesvn.net/) or [TortoiseGit](https://tortoisegit.org/), but there are other GUI's wrapped around Git that might give Git the nod in the client side interface category (i.e. [SourceTree](https://www.sourcetreeapp.com/), [GitEye](http://www.collab.net/products/giteye), [GitExtensions](http://gitextensions.github.io/), and many more).

For server side ui, Git wins this category by a longshot. Tons of user interfaces exist, that will nicely wrap around a Git repository (i.e. GitHub, BitBucket, GitLab, and more). As for subversion, [VisualSVN](https://www.visualsvn.com/server) seems to be the only choice. Both of these user interfaces will show the repository file/folder structure, markdown files, and commit history. However, the Git user interfaces add much more with graphs (showing contributors, commit timeline, amount of source changes, owners with their commits, etc), issue tracking, zip downloads, pull requests, review discussions, server side file CRUD options, project documentation sites, server side tagging, repository configuration, and more.
	
## code reviews.
As stated above, the server side interface for Git comes equipped with code reviewing funcationality. Unlike subversion, Git's server side user interface allows users to comment on commits with or without a pull request.

## source code change notifications.
For svn, source code change notifications can be implemented with hooks or communicated via e-mail or verbally. For git, this can happen with pull request. Pull requests are best used for code reviews . The pull request notifies all or certain individuals that a branch (master or not) has been updated and is available for a "git pull". This is typically the time in which developers will converse, comment, and review the commit.

For me, if things can be automated, let them be automated -- allow a pull request to notify me when source has changed.

## visual studio integration.
In Microsoft's Visual Studio solution, they have openly embraced git and all of its features. They enjoy git so much, they have included git as a staple in their source control solution TFS [(Team Foundation Server)](https://msdn.microsoft.com/en-us/Library/vs/alm/Code/git/overview). In order for svn to integrate with Visual Studio, users can go the free route with [AnkhSVN](https://ankhsvn.open.collab.net/) or [TortoiseSVN](http://tortoisesvn.net/visualstudio.html) or purchase a license for [VisualSVN](https://www.visualsvn.com/visualsvn/). 

It can argued that neither source control system holds the advantage in this category but with Microsoft's public approval git should get the advantage here.

## user access.
For both systems, all user access is controlled server side. This means for both project specific and branch specific access. However, not all user interfaces come equipped with the capability to manage these permissions. The ones equipped with this capability can be managed by a development manager or team lead. The ones unequipped have to be managed by a server admin.

User interfaces for git come fully equipped with this functionality (aka [BitBucket](https://confluence.atlassian.com/bitbucket/branch-management-385912271.html) or [GitHub](https://help.github.com/articles/defining-the-mergeability-of-pull-requests/)). Whereas for svn, a separate [tool](http://blogs.collab.net/teamforge/wildcard-access-control-and-path-based-permissions-in-teamforge#.Vf9T_xFViko) must be installed in order for the server admin to manage permissions to a particular branch/tag aka folder. 

In this category, the clear "thumbs up" of approval has to go to git. 

## planned workflow. branching, tagging, and merging.
When work is planned, either system will do the job perfectly. Everything can go to the trunk or master with no problems or quarrels about functionality conflicting in a release. However, nine times out of ten that is not always the case. Both systems provide the capabilities of branching, merging, and tagging. Both systems accomplish this workflow in different ways in terms of implementation and ease of use. 

The first thing to note are branches and where they are stored. In git, branches begin as local branches, meaning the server does not know about them - this is due to git's decentralized nature. Also in git, branches are not direct copies of the master/trunk, but represent the file differences between the master and its current state. Svn is the opposite. This does not mean this is a negative because this purely user preference. Some users might want to know about every branch that is being created by a team member (in order to create a branch in svn you have to commit the branch to the server). Some users might want a direct copy of what the trunk looked like when they branched (branches in svn copy/paste the existing state of the trunk into the branch). My preference leans towards a source control system that allows me to branch without a connection to the server and duplicate, redundant copies of my master/trunk branch - a decentralized/distributed system aka git.

Second is merging. Depending on how the repository is setup, merging in svn can be done by project tree or range of revisions. In my experience, if there are repositories within repositories merging by project tree can come with some "gotchas", i.e. if new files or folders are added to the main branch after the branch was created, those files or folders will get lost when merging the branch back into the main branch.

## unplanned workflow.
Git has stashing and patches. Subversion does not. Must rely on branching mechanism.

## rewriting commit history.

## project ignores.
When setting up a repository it is best to ignore certain files and folders. Both svn and git provide this capability with their global-ignores and .gitignore, respectively. Each system will allow you to ignore specific files by name while using or not using a regular expression wildcard. The only difference between the two comes when ignoring folders. For example, ignoring the compiled output folder of a project, say the bin folder in a .NET project, can be done by ignoring that folder in the global-ignores or .gitignore. However, if at anypoint another bin folder gets introduced to the repository, git will continue to ignore it (because all bin folders was specified to be ignored) but in svn the newly introduced bin folder will have to be specified in the global-ignores. So, if for any reason this new folder gets committed with the non-versioned items, the repository will contain compiled source (which is a big no no).

Based on this occurrence alone, git has a slight edge.

## experimental and creative nature.
Centralized vs decentralized. Network vs no network (committing and unable to commit).

-----

Hopefully, this post will help speed up the research for any development team when selecting a new source control system.
If you have any questions, comments, or concerns, please feel free to share them below.