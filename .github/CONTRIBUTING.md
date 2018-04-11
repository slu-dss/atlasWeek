# Contributing to this Lesson

## Before Opening an Issue
Before opening a new issue, be sure to [search issues and pull requests](https://github.com/slu-dss/lesson-template/issues)
to make sure the bug or typo hasn't been reported and/or already fixed.
Similarly, search to make sure someone has not already made a suggestion. If
there is a suggestion and you want to express support for it, please comment
in the existing issue.

By default, the search will be pre-populated with `is:issue is:open`. You can
[edit the qualifiers](https://help.github.com/articles/searching-issues-and-pull-requests/)
(e.g. `is:pr`, `is:closed`) as needed. For example, you'd simply
remove `is:open` to search _all_ issues in the repo, open or closed.

## PR process

### Fork, clone, branch
The first thing you'll need to do is to [fork](https://help.github.com/articles/fork-a-repo/)
[this lesson](https://github.com/slu-dss/lesson-template), and then clone it locally.
We recommend that you create a branch for each PR.

### Style
Match the existing code or documentation style. Be careful to only make style changes
to the code you are contributing.

### Document
Make sure to update the [lesson resources](../resources/) if your changes impact
any of those documents. If you add a new package dependency, please also update
the `DESCRIPTION` file so that the dependency is part of our continuous integration
testing.

### Check
Once you are done, make sure that all changed `.Rmd` documents knit without error.

### Commit
When you've made your changes, write a clear commit message describing what
you've done. If you've fixed or closed an issue, make sure to include keywords
(e.g. `fixes #101`) at the end of your commit message (not in its
title) to automatically close the issue when the PR is merged.

### Push and pull
Once you've pushed your commit(s) to a branch in _your_ fork, you're ready to
make the pull request. Pull requests should have descriptive titles to remind
reviewers/maintainers what the PR is about. You can easily view what exact
changes you are proposing using either the [Git diff](http://r-pkgs.had.co.nz/git.html#git-status)
view in RStudio, or the [branch comparison view](https://help.github.com/articles/creating-a-pull-request/)
you'll be taken to when you go to create a new PR. If the PR is related to an
issue, provide the issue number and slug in the _description_ using
auto-linking syntax (e.g. `#15`).

## Code of Conduct
Please note that this project is released with a [Contributor Code of
Conduct](CODE_OF_CONDUCT.md). By participating in this project you agree to
abide by its terms.
