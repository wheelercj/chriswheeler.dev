+++
title = 'Getting started with Git and GitHub'
date = 2021-09-07T14:42:16-08:00
lastmod = 2024-02-01T17:29:27-08:00
tags = []
+++

Here are some great resources for learning how to use Git and GitHub. There's more here than any one person will need, so just learn what will be helpful to you in the very near future.

1. Watch these short videos: [Version control with Git and GitHub](https://www.youtube.com/playlist?list=PLWKjhJtqVAbkFiqHnNaxpOPhh9tSWMXIF) by freeCodeCamp.
2. Check out Better Explained's guides to [version control](https://betterexplained.com/articles/a-visual-guide-to-version-control/) and [Git](https://betterexplained.com/articles/aha-moments-when-learning-git/), or the [_Learn X in Y minutes_ guide to Git](https://learnxinyminutes.com/docs/git/).
3. Consider using [Sublime Merge](https://www.sublimemerge.com/), a Git client with a graphical interface, because it makes the most commonly used features of Git easier to use without preventing you from learning more about Git. However, if your computer crashes a lot, Sublime Merge has a chance of corrupting Git repos.
4. Read some of this free online book, but only the parts that you will use very soon: [_Pro Git_](https://git-scm.com/book/en/v2) by Scott Chacon and Ben Straub.
5. Bookmark [_Dangit, Git!?!_](https://dangitgit.com/en) for when you make mistakes with Git (everyone does).
6. [_How to Write a Git Commit Message_](https://cbea.ms/git-commit/) by cbeams.
7. If you're working on a big project, you may want to know about [_A successful Git branching model_](https://nvie.com/posts/a-successful-git-branching-model/) by Vincent Driessen, which is an article about workflows with Git (sometimes called "Git-flows").

## collaborating

When you are working with others on a project and you're about to start work on a new feature or other change, here's one of several ways to prevent changes from being lost:

1. Use `git pull`.
2. Create a new branch with a descriptive name and checkout the branch.
3. Make the commit(s) on the branch you created.
4. When the commit(s) on your branch are ready to be combined with another branch, use `git fetch` and then `git status` to make sure you have the latest commits. If not, checkout the main branch, pull, checkout your branch, and either rebase or merge the main branch into your branch (merge if commit order matters, rebase otherwise).
5. Push the branch with the [`-u` option](https://git-scm.com/docs/git-push#Documentation/git-push.txt--u), e.g. `git push -u origin my-feature`.
6. Make and accept a pull request on GitHub between the two branches.
7. Delete the newer branch on GitHub (and optionally the local newer branch as well).

## further reading

* you can create custom Git commands, such as [this one](https://stackoverflow.com/questions/1838873/visualizing-branch-topology-in-git/34467298#34467298) for viewing commit history as a compact graph
* for solo projects, [how to remove an accidentally pushed file from a git repo history](https://dev.to/moshe/remove-accidentally-pushed-file-from-a-git-repository-history-in-4-simple-steps-18cg) by Moshe Zada
* [Commit Often, Perfect Later, Publish Once: Git Best Practices](https://sethrobertson.github.io/GitBestPractices/) by Seth Robertson
* [My favourite Git commit](https://dhwthompson.com/2019/my-favourite-git-commit) by David Thompson.
* version numbers
    * Semantic versioning is commonly used and is covered in [Versioning](https://datasift.github.io/gitflow/Versioning.html) (by datasift) and by [semver.org](https://semver.org/)
    * [CalVer](https://calver.org/) is another versioning convention based on the calendar
* tagging
    * [Lets start tagging!](https://medium.com/@keshshen/lets-start-tagging-88c299b6b331) by Nageswaran introduces tagging with Git
    * [Proper use of Git tags](https://blog.aloni.org/posts/proper-use-of-git-tags/) by Dan Aloni briefly covers how to avoid some common mistakes and challenges
    * [Git Basics - Tagging](https://git-scm.com/book/en/v2/Git-Basics-Tagging) (part of _Pro Git_ by Scott Shacon and Ben Straub) goes into detail

## alternatives to GitHub

* [GitLab](https://about.gitlab.com/)
* [Bitbucket](https://bitbucket.org/product/)
* [Codeberg](https://codeberg.org/)
* [Gitea](https://gitea.io/en-us/)
* [search for more](https://duckduckgo.com/?t=ffab&q=github+alternatives&atb=v305-1&ia=web)
