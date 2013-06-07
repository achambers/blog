---
title: Dood, where's my transaction?
date: '2011-01-27'
description:
tags: []
---

Seam 3 has been written in a very modularized way. It is basically CDI at it's core with a bunch of modules that give support for things like Transactions, Facelets, GWT, JBPM that can be tacked on to CDI as the developer sees fit. This is great as the Seam 2 stack contained all this stuff, a lot of which was not needed all the time. Now, with Seam 3, we can just pick and choose which modules we need.

Out of the box, CDI does not come with any container managed transaction (CMT) support. However, using the Seam 3 Persistence module you can add CMT support to your application.

This allows us to use annotations in our code to signify where transactions should begin.

The Seam Persistence module documentation can be found [here][1].

The following is how I plugged the module in and a couple of gotchas I came up against.

Firstly, I needed to add the Seam Persistence jars to our project. I'm using Maven 3, so the following are the dependency definitions 

```xml
<dependency>
	<groupId>org.jboss.seam.persistence</groupId>
	<artifactId>seam-persistence-api</artifactId>
	<version>${seam.persistence.version}</version>
</dependency>
<dependency>
	<groupId>org.jboss.seam.persistence</groupId>
	<artifactId>seam-persistence-impl</artifactId>
	<version>${seam.persistence.version}</version>
</dependency>
<dependency>
	<groupId>org.jboss.seam.solder</groupId>
	<artifactId>seam-solder</artifactId>
	<version>${seam.solder.version}</version>
</dependency>
```

Next, enable declarative transaction management in your beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans 	xmlns="http://java.sun.com/xml/ns/javaee" 
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
		xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
		http://docs.jboss.org/cdi/beans_1_0.xsd">
	<interceptors>
		<class>org.jboss.seam.persistence.transaction.TransactionInterceptor</class>
	</interceptors> 
</beans>
```

Now, according to the documentation, you should be able to use the EJB3 annotation `@TransactionAttribute` to specify transaction boundaries. I couldn't actually get this to work for reasons unknown. Instead, however, you can use the Seam `@Transactional` annotation instead. And walah, there you have it. Container Managed Transactions in CDI with a little help from the Seam 3 Persistence module.

[1]: http://docs.jboss.org/seam/3/persistence/latest/reference/en-US/html/persistence.html 'Seam Persistence documentation'