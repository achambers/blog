---
title: unless - so simple, so sexy
date: '2011-07-27'
description:
tags: []
---

So, I'm spending a couple of hours on an unattended test for a contract that I'm thinking of going for and I have to write a bit of Java code that simulates some ATM software. While churning out a couple of methods I had my very first 'Man, this would be so much easier in Ruby' moment and it put a smile on my face to know that this is the first of many more to come. I know the Ruby syntax is a lot less verbose than Java and has a lot of niceties that Java doesn't but it's so nice to be in the real world and just see it in action.

So, I needed to write a method that would throw an exception if an account had insufficient funds to make a withdrawal. It looked something like this:

```java
public Account withdraw(long accountNumber, BigDecimal withdrawalAmount) {
  Account account = accountDao.get(accountNumber);
 
  if(!account.hasSufficientFunds(withdrawalAmount)) {
    throw new InsufficientFundsException();
  }
 
  //some code here
 
  return account;
}
```

And it just got me thinking how it's so crap that I need to negate the hasSufficientFunds method. You spend all this time making easy to read method names, making sure they are written in the affirmative and convey what they do and then you have to go and negate it by adding a damn question mark to it to confuse everything. It's just putting extra pressure on your brain to parse everything. Java, it doesn't need to be that hard....if only you had syntax like Ruby. I found myself wishing I could write the method like this:

```ruby
def withdraw account_number, withdrawal_amount do
  account = Account.find account_number
 
  unless account.sufficient_funds? withdrawal_amount do
    raise "Insufficient funds"
  end    
 
  #...more code...
end
```

Sooo much nicer....Soooo much sexier

