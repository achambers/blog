---
title: RVM
date: '2011-05-24'
description:
tags: []
---

I had a 'light bulb' moment the other day when I actually understood the value of [RVM][1]. Before then I was using it, but didn't really understand why and what value it was giving me. The reason for this was because I didn't really understand what it does and what it's trying to solve. I am no expert in RVM by any means but this is my understanding of RVM and why and how I use it.

### What is RVM?

RVM stands for Ruby Version Manager. It basically allows you to manage multiple versions of Ruby on your system and more importantly, I think, it allows you to sandbox Ruby versions and gem sets so that each project can have it's own encapsulated environment with only the gems that are required. This will ultimately save you from dealing with sneaky versioning issues between gems and also allows you run apps against new version of Ruby and gems without upgrading your system version.

### How RVM works

When you install Ruby to your system normally it is installed to a system directory like /user/bin/ruby for instance (on Mac OS X). When you install gems using the system version of Ruby/Rubygems they are all installed into the same system directory. This means that different versions of the same gem are installed into the same place and it also means that gems from across all projects that use that version of Ruby are installed into the same place. This can cause multiple hassles from gem version collisions to inconsistent environment set ups between development and staging/production.

RVM however, allows you to run multiple versions of Ruby with multiple gem sets all sandboxed in their own environment specific directory within RVM. For starters, RVM installs itself in the `~/.rvm` directory. This means that you no longer need to use `sudo` to install gems. This also means that from now on, any Ruby versions or gems you install are installed into the RVM directory and kept completely separate from your system installation of Ruby.

When you use RVM to switch to a particular version of Ruby and/or a particular gemset it essentially switches the PATH variables that point to the Ruby version and gems directory. From that point onwards, you use Ruby exactly as you normally would. It's a pretty simple concept when you think about it. But it is oh so powerful.

### How to install a Ruby version

Once you have RVM installed (follow the instructions to do this on the website above), the first thing you will want to do is install a version of Ruby. First lets see which version of Ruby are available to us:

```bash
$ rvm list known
```

In this instance we'll install version 1.9.2-head by typing the following:

```bash
$ rvm install ruby-1.9.2-head
```

RVM has now installed our required version of Ruby and set it to be the current one. We can check this:

```bash
$ rvm current
#=> ruby-1.9.2-head
```

We can also see which other version of Ruby we have installed in RVM:

```bash
$ rvm list
#=> ruby-1.9.2-p180 [ x86_64 ] 
#=> ruby-1.9.2-head [ x86_64 ]
```

To switch to the other version of Ruby we would simply type the following:

```bash
$ rvm use ruby-1.9.2-p180
```

So what has actually happened when we installed this version of Ruby with RVM?  To get a better idea, let's have a look at the rvm info page:

```bash
$ rvm info
```

From the output we can see a few important things:

- RVM is using the version of Ruby, `ruby-1.9.2-p180`, which is installed in `[user home]/.rvm/rubies/ruby-1.9.2-p180`
- The directory where gems are installed is `[user home]/.rvm/gems/ruby-1.9.2-p180`
- RVM has modified the PATH variable to include the path to the bin directory of the version of Ruby, the gem directory and also the global gem directory (which I will explain in more detail further down)

As you can see, everything to do with this version of Ruby is contained into it's own directory specific to that version of Ruby. Awesome!

### Gems and Gemsets

RVM has the concept of Gemsets which is essentially a directory where gems are installed. This means for each version of Ruby we can create different gem sets. It wasn't until I started using Ruby across multiple projects that I understood the value of this. Gemsets essentially mean that for each project I have that is using Ruby version 1.9.2-head I can have a separate set of gems that are specific to each project. This means that there are going to be no collisions between gem versions used across different projects and there are also not going to be redundent gems used in one project but not in another. This is a great thing which also makes is super easy to have a sandboxed environment that can be easily replicated across different servers from development to staging/production.

Before looking closer at Gemsets it's important to note that there are 3 scopes of Gemset. There is the default gemset, global gemset and, for lack of a better word, specific gemset. 

The default gemset is the gemset used when RVM is set to use a specific version of Ruby but no specific gemset. This gemset lives at `[rvm home]/gems/ruby-1.9.2-p180`.

The specific gemset is the gemset that I would create for each project, for instance My Project might have a gemset called my_project. This will hold all gems specific to the project that will use it. This gemset lives at `[rvm home]/gems/ruby-1.9.2-p180@my_project`.

Finally, the global gemset is a gemset that is available to all other gemsets within a ruby version. So any gems I install in the ruby-1.9.2-p180 global gemset will be available to all other specific gemsets as well as the default gemset. This gemset lives at `[rvm home]/gems/ruby-1.9.2-p180@global`.

So lets create our first gemset for our head version of Ruby:

```bash
$ rvm use ruby-1.9.2-head 
$ rvm gemset create my_project 
$ rvm use ruby-1.9.2-head@my_project 
```

We could also create and use that gemset in the same command:

```bash
$ rvm use --create ruby-1.9.2-head@my_project
```

If we now run the `rvm info` command you will see that the gem directory is now set to `[rvm home]/gems/ruby-1.9.2-head@my_project`. Any gems we install from now on will be installed into this directory, and as long as we switch to this ruby version and gemset when running our project, that is the version of gems that will be used.

### Automcatically switching to a gemset

Knowing which gemset a project is using and having to switch to it each time you go to the directory can become very tedious. To help with this you can create a file in the root of your project directory called `.rvmrc` and add the above command to use/create the required gemset. This file will be executed everytime you cd into the project directory, switching the gemset for you. This is very handy.

So, that's my high level understanding and explanation of what RVM is, how it works, and why it should be a definite tool in your Ruby toolkit.

[1]: http://rvm.io "RVM"