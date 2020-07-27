---
layout: post
title: "Git Ignore Locally"
date: 2020-07-26 00:33:00 -0500
---

The blog post is going to a short and sweet writeup about one of the git aliases that I have created in my gitconfig. I use this shortcut quite often and it could come in handy for developers using Git as version control.

`.gitconfig` is the file that stores basic setting for Git installed on your machine. It includes your commit name and email. You can also set your diff and merge tool here, among other things.

### Locations

For Windows

```
%USERPROFILE%\.gitconfig
```

For Mac

```
~/.gitconfig
```

### Problem

Imagine you are working on a project. The project requires modifications to specific files to run on your machine. You do not want to commit these changes, but Git tracks these files.

These files will clutter the commit tree in your source control manager or when you do `git status`.

Above is just one scenario. There could be any reason for you to have some local modifications to files which you should not commit and like I said you don't want to commit them.

### Solution

You can ignore these files locally without un-tracking them!

Add the following aliases to your `.gitconfig` file.

```
ignore = update-index --skip-worktree
unignore = update-index --no-skip-worktree
ignored = !git ls-files -v | grep \"^S\"
```

It gives you three Git shortcuts `ignore` (to locally ignore some files), `unignore` (to stop the local ignore on specific files) and `ignored` (to see the list of files you have ignored).

Execute any of the shortcuts, prefixed with `git`, in your terminal. Like below:

```console
> git ignored
```
