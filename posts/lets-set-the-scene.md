---
title: Let's set the scene
date: '2011-01-26'
description:
tags: []
---

So, [Liquid Armour][1] and I are are working on a little web/mobile app that we think is much needed in and around London at the current time. I don't want to reveal too much about the app itself at this stage but I thought this a great place to record issues, fixes, lessons and ideas that we come across throughout the course of this little project. I feel that it might be nice to record the journey from a concept to a realized web application out in the wild. Only future me knows where this will end up, but at least, by the time we get there, we should have a pretty decent tome describing the trials and tribulations that got us there.

Liquid Armour and I used to work together in a small web development company in Melbourne, Australia. It was there that I first got my real taste of much of the JBoss stack, primarily using JBoss Seam.

Over here in the UK, Seam doesn't seem to be very popular. I think I've seen it mentioned in only a handful of job adverstisements since I got here. For these reasons, we have decided to build our app using JBoss' implementation of CDI which is the new specification for Java Contexts and Dependency Injection. Jboss have written the reference implementation of CDI, known as Weld, and this is what the upcoming Seam 3 is based on. As we both like playing around with new technologies, CDI seemed like the perfect technology to use for our new project.

As is the case when using early versions of software and frameworks, you are bound to come across bugs and issues. We are definitely coming up against these since the get go so this project should be a great source of inspiration for blog posts.

Anyway, our project at this point in time is going to be using the following technologies:

-	CDI/Weld
-	JSF 2
-	JPA/Hibernate

And as a bit of a learning exercise, we're going to use Git for our source control. Time to see what the fuss is all about.

[1]: http://liquidarmour.biz/ "Liquid Armour"