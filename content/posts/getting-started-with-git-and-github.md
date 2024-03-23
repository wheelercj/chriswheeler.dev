+++
title = 'Getting started with Git and GitHub'
date = 2021-09-07T14:42:16-08:00
lastmod = 2024-02-25T01:07:32-08:00
tags = []
+++

Here are some great resources for learning how to use [Git](https://git-scm.com/) and [GitHub](https://github.com/). There's more here than any one person will need, so just learn what will be helpful to you in the very near future.

1. Watch these short videos: [Version control with Git and GitHub](https://www.youtube.com/playlist?list=PLWKjhJtqVAbkFiqHnNaxpOPhh9tSWMXIF) by freeCodeCamp.
2. Check out Better Explained's guides to [version control](https://betterexplained.com/articles/a-visual-guide-to-version-control/) and [Git](https://betterexplained.com/articles/aha-moments-when-learning-git/), or the [_Learn X in Y minutes_ guide to Git](https://learnxinyminutes.com/docs/git/).
3. Consider starting with a Git client with a graphical interface, such as [GitHub Desktop](https://docs.github.com/en/desktop), [GitButler](https://gitbutler.com/), or [Sublime Merge](https://www.sublimemerge.com/). They make the most commonly used features of Git easier to use without preventing you from learning more about Git.
4. Read some of this free online book, but only the parts that you will use very soon: [_Pro Git_](https://git-scm.com/book/en/v2) by Scott Chacon and Ben Straub.
5. Bookmark [Dangit, Git!?!](https://dangitgit.com/en) for when you make mistakes with Git.
6. [How to Write a Git Commit Message](https://cbea.ms/git-commit/) by cbeams.

## Git workflows

There are many different workflows that can be used with Git. A great example is [GitHub flow](https://docs.github.com/en/get-started/using-github/github-flow), by GitHub. A workflow I sometimes use is very similar:

1. Use `git pull` to make sure your local copy of the code is up-to-date.
2. Create a new branch with a descriptive name and `git switch` to the branch.
3. Make commits on your branch.
4. When your commits are ready to be merged into the main branch, use `git fetch` and then `git status` to make sure your local copy of the code is up-to-date. If not, switch to the main branch, pull, switch to your branch, and either rebase or merge the main branch into your branch (merge if commit order matters, rebase otherwise).
5. Push your branch.
6. Make and accept a pull request on GitHub between the two branches.
7. Delete your branch both locally and on GitHub.

When you're waiting for a pull request to be approved but have more work to do, create a new branch off the unmerged branch, not main.

Another example of a Git workflow with a different use-case is covered in [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/) by Vincent Driessen.

## further reading

* you can create custom Git subcommands, such as [this one](https://stackoverflow.com/questions/1838873/visualizing-branch-topology-in-git/34467298#34467298) for viewing commit history as a compact graph
* [So You Think You Know Git](https://www.youtube.com/watch?v=aolI_Rz0ZqY) by Scott Chacon
* for solo projects, [how to remove an accidentally pushed file from a git repo history](https://dev.to/moshe/remove-accidentally-pushed-file-from-a-git-repository-history-in-4-simple-steps-18cg) by Moshe Zada
* [Commit Often, Perfect Later, Publish Once: Git Best Practices](https://sethrobertson.github.io/GitBestPractices/) by Seth Robertson
* [My favourite Git commit](https://dhwthompson.com/2019/my-favourite-git-commit) by David Thompson.
* [Git Pathspecs and How to Use Them](https://css-tricks.com/git-pathspecs-and-how-to-use-them/) by Adam Giese.
* version numbers
    * Semantic versioning is commonly used and is covered in [Versioning](https://datasift.github.io/gitflow/Versioning.html) (by datasift) and by [semver.org](https://semver.org/)
    * [CalVer](https://calver.org/) is another versioning convention based on the calendar
* tagging
    * [Lets start tagging!](https://medium.com/@keshshen/lets-start-tagging-88c299b6b331) by Nageswaran introduces tagging with Git
    * [Proper use of Git tags](https://blog.aloni.org/posts/proper-use-of-git-tags/) by Dan Aloni briefly covers how to avoid some common mistakes and challenges
    * [Git Basics - Tagging](https://git-scm.com/book/en/v2/Git-Basics-Tagging) (part of _Pro Git_ by Scott Shacon and Ben Straub) goes into detail
* [How HEAD works in git](https://jvns.ca/blog/2024/03/08/how-head-works-in-git/) by Julia Evans
* [Creating a GPG key for signing Git commits](https://til.chriswheeler.dev/creating-a-gpg-key-for-signing-git-commits/)
* [git-branchless: High-velocity, monorepo-scale workflow for Git](https://github.com/arxanas/git-branchless)

## alternatives to GitHub

* [GitLab](https://about.gitlab.com/)
* [Bitbucket](https://bitbucket.org/product/)
* [Codeberg](https://codeberg.org/)
* [Gitea](https://gitea.io/en-us/)
* [search for more](https://duckduckgo.com/?t=ffab&q=github+alternatives&atb=v305-1&ia=web)

## alternatives to Git

* [Fossil](https://fossil-scm.org/home/doc/trunk/www/index.wiki)
* [Mercurial](https://www.mercurial-scm.org/)
* [search for more](https://en.wikipedia.org/wiki/Comparison_of_version-control_software)
