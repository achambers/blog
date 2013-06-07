---
title: GIT rid of SVN
date: '2011-02-02'
description:
tags: []
---

So, after many years of using SVN, and a few less years of hearing about distributed version control, I decided to try out Git on our new project. It's a bit of trial and error and I'm still learning the differences and nuances between SVN and Git but I've got to say that I'm loving it. Below are a few thoughts about Git and things I'm learning along the way.

Firstly, to date, my SVN experience has been 100% through a GUI, whether that be TortoiseSVN or Subclipse/Subversive. While plugins often make learning a tool easier, I believe that they really do abstract you away from the inner workings of a tool and save/stop you from understanding what is really going on under the covers. I hate not knowing what is going on behind the scenes so it is for this reason that I have decided to learn Git from the command line. While it can be a bit confusing and tiresome, and feel downright slow trying to understand a diff on the command line as opposed to two nice side by side windows, I think learning Git this way will definitely cement in my mind how it hangs together.

So, as we all know, SVN is a remote source control server that each developer pulls changes down from and pushes changes up to. There are a number of things that are frustrating about this method, some of which include:

- Pulling a mass of changed files that are all related to different code changes when working with multiple developers
- The frequency of pulling in code that breaks your build if not tested properly
- Slow speed to pull down large groups of changes, comparing files with previous versions, switching branches etc
- The frequency of merges going wrong, especially when merging lots of code between branches.

Git, however, has a different model. Being distributed, there is no one master repository where the source code lives. Every developer has a local copy of the repository that contains every single commit since the repository was created. This allows the developer to commit code, create/switch branches, diff files, all within the local repository, without the need to be connected to a network. Once a developer is happy with their code, they are able to push their commits to a remote repository, be it a common remote repository such as one hosted on Github or another developer's local machine repository.

From what I've been reading, the merging capabilities of Git far out weigh that of SVN as well. From what I can gather the difference between the two is quite large. My understanding is that SVN basically looks at the current revision of a file and the revision of the file to be merged and just merges the two lots of file contents. As you can imagine, this can leave you in a world of pain if multiple lines of the same code in both revisions have been changed by different people in different ways. Not to mention if code was deleted. This is often referred to as 'Merge Hell'.

Git, on the other hand, tracks the actual changes that are made to a file from the very first point it was created. Each commit of a file knows about the parent commit. Therefore, Git can order a number of different commits and apply them to a file in sequential order massively reducing the merge conflicts experienced. At least, that's my basic understanding of it anyway.

So I'm really liking the feel of Git, compared to SVN. Here are some of the commands that I use most frequently. I'm recoring them here mainly so that I can reference them in the future, but also in case anyone out there is starting out with Git and wanting a nudge forward.

### Create a repository

The first thing you might want to do is create a local Git repository for a project you are working on. To do this, at a terminal prompt, move to the base directory of the project and run:

```bash
$ git init 
```

This will initialise the git repo for you. Exciting eh?  Or if you wanted to create a repo from a remote one, maybe on Github, you need to clone it like so:

```bash
$ git clone https://github.com/someuser/somerepo.git
```

This will pull down everything within the repo specified, including the history of every commit ever made since the project was first created. It will also update your git project config so that it knows that your local repo is linked to that remote repo, which Git will call `origin`.

### Committing files

Once you have new/modified files in your Git repo, whether it be the initial commit, or a bug fix to code on a shared project you cloned, you're going to want to commit them to your repo.

From here you can run the following command to see the status of your repo, showing which files you have modified or added to the index:

```bash
$ git status 
```

This will print out any files that are currently untracked by Git, and any files that have been modified since the last commit. In the case of the newly create Git repo, everything will be untracked. To add everything to the index, run the following:

```bash
$ git add .
```

Running the `git status` command now will show you that Git is now tracking your project files but they are uncommitted. To commit the files, just use the following command:

```bash
$ git commit -m 'Enter commit comment here'
```

It just keeps getting easier doesn't it?  This command is obviously going to commit your files to your local repo. The -m flag allows you to add a comment. Missing this flag, Git would prompt you for a commit comment.

It is really important, at this stage, to understand that at this point you have only commited changes to your local repo. If you have cloned a remote repo, you have not yet made any changes to that repo. They are all local to your environment and no one can get to them. This is the beauty of a distributed version control system. You can commit files until your heart is content and you do not need to be connected to any network and will not affect the code that anyone else is working on. Not until you are ready to push your changes to a remote repo.

### Pushing files

Speaking of pushing to a remote repo, maybe we should do that. First we want to ensure that we have the most up to date code from that remote repo. This is easily done by running the following commands:

```bash
$ git fetch origin
```

This will fetch any changes from the remote repo. Before merging them into our repo, we should probably diff them to have a look at what has changed

```bash
$ git diff origin/master
```

Once happy with changes that we will be merging into our repo, we do the following:

```bash
$ git merge origin/master
```

Now that we have the latest code, it's time to push our changes up to the remote repo. Simply do the following:

```bash
$ git push origin master
```

It's as easy as that.

### Branching

One thing I love about Git is how easy and quick branching is. For some reason, I hardly do it in SVN because it just feels clunky and slow. And switching code branches takes time and, for some reason, I am never confident that Eclipse switched branches properly and that there aren't files lying around from the previous branch. Anyway, with Git, I never have these worries. Switching branches is so easy and so damn quick.

To create a branch, it really is as easy as:

```bash
$ git branch newbranchname
```

To switch to that branch, just type:

```bash
$ git checkout newbranchname
```

And finally to delete a branch:

```bash
$ git branch -d newbranchname
```

And as with merging changes from a remote repository, we can merge changes in from a local branch to master, by typing the following:

```bash
$ git checkout master
$ git merge newbranchname
```

So, there you have it. My first, simple post on some of the basic Git commands that I use from day to day. Now lets all try and GIT rid of SVN.