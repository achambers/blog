---
title: "WELD - ClassFormatError: Illegal class name"
date: '2011-07-15'
description:
tags: []
---

It's been a few weeks since I've committed any code to CoffeeSnobs. I have been spending my time working on the design, look and feel mostly, as well as working on another project.

Yesterday, when I went to run CoffeeSnobs locally to implement the new look and feel, for some reason I got the following exception when starting the server:

```java
Caused by: java.lang.ClassFormatError: Illegal class name "org/jboss/seam/security/management/action/org$jboss$weld$bean-jboss$classloader:id="vfs:$$$Applications$servers$jboss-6$0$0$Final$server$default$deploy$coffee-snobs-jee6$war"-ManagedBean-org$jboss$seam$security$management$action$GroupAction[@javax$enterprise$context$ConversationScoped()@javax$inject$Named(value=)]{org$jboss$seam$security$management$action$GroupAction$conversation[@javax$inject$Inject()]$org$jboss$seam$security$management$action$GroupAction$identitySession[@javax$inject$Inject()]$org$jboss$seam$security$management$action$GroupAction$deleteGroup[@org$jboss$seam$transaction$Transactional(value=REQUIRED)@org$jboss$seam$transaction$TransactionalInterceptorBinding()](java$lang$String,java$lang$String)$org$jboss$seam$security$management$action$GroupAction$save[@org$jboss$seam$transaction$Transactional(value=REQUIRED)@org$jboss$seam$transaction$TransactionalInterceptorBinding()]()$}_$$_WeldSubclass" in class file org/jboss/seam/security/management/action/org$jboss$weld$bean-jboss     
at java.lang.ClassLoader.defineClass1(Native Method) [:1.6.0_26]     
at java.lang.ClassLoader.defineClassCond(ClassLoader.java:631) [:1.6.0_26]     
at java.lang.ClassLoader.defineClass(ClassLoader.java:615) [:1.6.0_26]     
at java.lang.ClassLoader.defineClass(ClassLoader.java:465) [:1.6.0_26]     
at sun.reflect.GeneratedMethodAccessor129.invoke(Unknown Source) [:1.6.0_26]     
at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25) [:1.6.0_26]
at java.lang.reflect.Method.invoke(Method.java:597) [:1.6.0_26]     
at org.jboss.weld.util.bytecode.ClassFileUtils.toClass2(ClassFileUtils.java:143) [:6.0.0.Final]     
at org.jboss.weld.util.bytecode.ClassFileUtils.toClass(ClassFileUtils.java:109) [:6.0.0.Final]
... 67 more
```

Now, the weird thing here that I have not made any changes since the last time I ran the project and committed code. And it worked then. 

So I did a little bit of searching and found the following forum post which seems to have fixed the problem.

[http://seamframework.org/Community/IllegalClassNameException][1]

The problem seems to be in regards to the version of the Weld jar in the Jboss AS server. Although, I have no idea why this used to work and then all of a sudden stopped. Anyway, the solution is as follows (by the way, I have Jboss AS 6.0.0Final installed on Mac OSX Snow Leopard):

Go to `$JBOSS_HOME/server/<whatever>/deployers/weld.deployer/` and delete the file called `weld-core-no-jsf.jar`.

Then:

Download `https://repository.jboss.org/nexus/content/groups/public-jboss/org/jboss/weld...` and copy it into the above directory

And there you have it. All fixed.

[1]: http://seamframework.org/Community/IllegalClassNameException "Forum Post"