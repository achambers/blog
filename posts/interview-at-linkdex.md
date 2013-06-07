---
title: Interview @ Linkdex
date: '2011-07-12'
description:
tags: []
---

So, I've just finished my last contract and am about to start looking into getting another, so I decided it might be a good idea to blog about the interview processes that I come across and the sorts of questions that I am given. This is mainly for my own record so that I can keep track of the sorts of questions that are being asked and the things I should keep fresh in my mind when going for future interviews, but also for anyone else who might be going for a job at one of these companies. So, without further ado, the latest interview I went for...

### The Company

The company are a startup that's been around for a couple of years. Their product is basically a web based management and workflow tool for managing links to your website. It analyses your website and that of your competitors and allows you to see stats on your current and potential links. It also wraps this with a workflow engine to allow you to track tasks based around building new, and maintaining existing, links.

### Interview #1 - The telephone interview (David)

One of the easiest telephone interviews I've ever had. There was basically zero technical questions and it was more of an informal chat. David and I chatted for about 45 minutes, discussing mainly the technology stack that the company use. David asked me only one question about my CV which was when he asked for some background on the project I undertook to implement Cassandra at my last place of work. He wanted to know a little about the drivers for the work and the outcomes. Other than that, we basically just had a chat about different technologies and chewed the fat about the company.

### Interview #2 - The face to face (David, Martin)

The first hour and a half of the interview was more of a technical nature with David. We sat in a room and went through a few technical type questions on a white board, followed by going into some detail about the workings of the company's system and some hypothetical situations based around how I would approach a particular piece of work.

First off David asked me to take a whiteboard marker and write a method that would reverse a String. I started out by telling him I'd use the `StringBuffer#reverse` method, but understanding that that probably defeats the purpose of the question, went on to write something along the lines of:

```
public String reverseIt(String myString) {
    StringBuffer sb = new StringBuffer(myString.length());

    for (int i = myString.length() - 1; i >= 0; i--) {
        sb.append(myString.charAt(i));
    }

    return sb.toString();
}
```

Next David, asked me to write a generic class that modelled a key/value pair like so:

```
public class Test<KEY, VALUE> {
    private KEY key;
    private VALUE value;
    public Test(KEY key, VALUE value) {
        this.key = key;
        this.value = value;
    }
    //...Getters
}
```

Then he asked me to improve that by making it a factory type class by putting a static build method on it. The point of this question was to see if I understood the concept that generic types defined at the class level cannot be seen by the static method. Hence:

```
public class Test<KEY, VALUE> {
	private KEY key;
	private VALUE value;

	private Test (KEY key, VALUE value) {
		this.key = key;
		this.value = value;
	}

	public static <KEY, VALUE> Test build(KEY key, VALUE value) {
		return new Test(key, value);   
	}   

	//... Getters
}
```

Next we did some SQL. David drew two tables on the board, the first, `Person (id, name, age, gender, address_id)` and the second, `Address (id, address)`. I had to write up a few select queries basied on the schema. The first one I think was along the lines of select all addresses along with the county of people at each one. The last I think was something like select all addresses along with the count of males and the count of females without doing any sub selects. Something along those lines anyway.

The final white board question was one of my favourites. It was simply to get an idea of whether I thought logically and how I went about problem solving. Basically, he drew an analogue clock face on the board without hands. He asked me to calculate the number of degrees of the smaller angle between the hour and minute hands when the clock shows the time as 4:40. The solution to this was pretty straight forward. A clock, 360 degrees, is made up of 12 segments, which are 30 degrees each. At 4:40, the minute hand is at 8 and the hour hand is 2/3 of the way between 4 and 5. This means that there is 1/3 of 30 degrees between the hour hand and 5 o'clock which equals 10 degress. And there is 90 degrees between the 5 and 8. Therefore, there is 100 degress between the hour and minute hands. Easy peasy.

So, after some chatting about the Linkdex technology and other stuff, my interview with David was concluded and Martin came in to meet me.

Martin really just went through the business side of what Linkdex do and how they operate. He asked me a few questions about things on my CV such as which project would be most similar to how the company operate. He also asked which project I would have done differently and why. Finally Martin asked me a question that I wasn't expecting: **If I have £100 at the start of the first year and £200 at the end of the second year, what is the annual interest rate?**

Now, this question kind of threw me off guard as I wasn't expecting it and I haven't thought too much about my interest calculation maths since high school. I also didn't think to ask probably the most important question, 'Is that simple or compound interest?'. I was trying to work out the compound interest and got myself tied in a little knot. But I think Martin was actually after the simple interest which I could have told him in 2 secs. I guess I'll never know.

Anyway, that's about the extent of the interview. Overall not all that technical but there were a couple of nice logic questions in there.