---
title: Include vs Extend. What's the difference?
date: '2011-08-26'
description:
permalink: 'include-vs-extend-whats-the-difference'
tags: []
---

So, I was wondering the other day what the hell the difference between `include` and `extend` in Ruby are and which one you'd want to use under what circumstances. After a bit of digging around on the net and playing with some code, it turns out that the answer is really quite simple. I'm sure for all Ruby experts this is a no brainer but since I am early on in my [path to enlightenment][1], I wanted to document it so I can cement the concept in my mind. 

Firstly, simply put, both include and extend are Ruby's keywords to allow a class to inherit methods from a module. The difference between the two is thus:

> `include` allows you to inherit methods as Instance Methods.  
> `extend` allows you to inherit methods as Class Methods. 

And the way I've been remembering this is that **IN**clude adds **IN**stance methods.

This is not unlike Java's `extends` keyword, however, the idea of include/extend in Ruby I find is slightly different. In Java, to extend something means that the extending class IS-A subclass of the super class. In Ruby this doesn't seem to be the way to think about it. It would seem that to include/extend a module in Ruby simply means that the including class wants to take on the methods of the Module. It can be totally unrelated in terms of an object hierarchy.

Anyway, let's have a little look at some code to explain the difference between include and extend.

```
module Tweet
  def save
    puts 'Tweet saved'
  end
end

class Foo
  include Tweet # Add methods from Tweet as Instances Methods
end

class Bar
  extend Tweet # Add methods from Tweet as Class Methods
end
Foo.save # => undefined method `save' for Foo:Class (NoMethodError)
Foo.new.save # => Tweet saved
Bar.save # => Tweet saved
Bar.new.save # => undefined method `save' for #<Bar:0x007f813104e0e8> (NoMethodError)
```

A small shout out to [John Nunemaker][2] who's post I found that explain's this concept nicely.

[1]: enlighten-yourself-with-ruby-koans "Ruby Koans"
[2]: http://railstips.org/blog/archives/2009/05/15/include-vs-extend-in-ruby/ "Rails Tips"