---
title: Crunch API - My feedback, suggestions and opinions
date: '2012-01-11'
description:
tags: []
---

Hey Crunch Team, just had a bit of a squiz over [your new your API][1] and wanted to put forward some feedback, suggestions and a few opinions.

Now, everything following this is based on my understanding of the REST spec, my beliefs of how a RESTful API should be designed and any opinions I have along the way.

I basically skipped over the OAuth stuff and on to the endpoints which is what interests me the most, so let's get started.

### Naming

The way I look at a RESTful resource URI is similar to how I look at a folder structure on a file system. On my filesystem, in my `suppliers` folder, I might have a list of suppliers listed by `id`. Inside each `id` folder I have any folders and data that correspond to the specific supplier.

So, if we take a look at your services to get a list of suppliers and expenses...

> /rest/api/suppliers  
> /rest/api/expenses

...things look pretty good. To get a list of my suppliers, I look in my `suppliers` folder. Likewise, for my expenses, I look in my `expenses` folder. So far so good.

However, now let's take a look at the services behind GETting a specific supplier or expense:

> /rest/api/supplier/{id}  
> /rest/api/expense/{id}

This is where I think things start to fall down a little. What you have done here is tell me that there is a folder, or resource if you will, called supplier, at the same level as the suppliers/expenses folder. And you are also telling me that I will find the supplier/expense for a specific ID in this folder.

What I would actually expect to find is a specific supplier underneath the `suppliers` folder on my filesystem...wouldn't I? Maybe something like this is more appropriate:

> rest/api/supplier**s**/{id}

Notice the `s`? It is a very small point, and it's all semantics, however, I still think it's important. The supplier I want is in the list of suppliers that lives inside the `suppliers` folder.

Next up, lets have a quick look at:

> /rest/api/expense/expenseTypes  
> /rest/api/expense/paymentMethods

Once again, if we follow the folder theme, it doesn't, in my mind, make sense that I would look in the expense folder to find a list of possible expenseTypes or paymentMethods. To me, I would have my `expenses` folder which contained my list of expenses and then when I wanted to find the possible list of expenseTypes, I'd probably find that in it's own folder at the top level like so:

> /rest/api/expense_types

Likewise, I'd probably do the same thing with payment methods:

> /rest/api/payment_methods

The reason behind this is they are, in a way, top level resources. They don't live as subordinates of an expense, or anything else really.

Now on to accounts. Let's have a look at your current URI's:

> /rest/api/accounts/allAccounts  
> /rest/api/accounts/bankAccounts  
> /rest/api/accounts/creditCards

Thinking back to the folder analogy, on my filesystem, I don't think I'd arrange my accounts in `allAccounts`, `bankAccounts` and `creditCards` folders underneath an `accounts` folder. That doesn't really make sense to me. In my mind it would make more sense to have all my accounts sitting inside the `accounts` folder and each account would have a type. So when I want to get a list of all my accounts I'd probably go to:

> /rest/api/accounts

If I wanted only accounts that were bank accounts, I'd probably look for accounts, in the `accounts` folder that have a type `bankAccount` like so:

> /rest/api/accounts?type=bank_account

Likewise, I'd do something similar if I wanted credit card accounts:

> /rest/api/accounts?type=credit_card

As for the payload that is returned, I'd probably then expect something like so:

```xml
<accounts>
  <account  accountId="1" type="bank_account">Bank Account 1</account>
  <account  accountId="2" type="bank_account">Bank Account 2</account> 
</accounts>
```
 
and:

```xml
<accounts>
  <account  accountId="3" type="credit_card">Credit Card 1</account>
  <account  accountId="4" type="credit_card">Credit Card 2</account> 
</accounts>
```

This then allows you to return all accounts to me with:

```xml
<accounts>
  <account  accountId="1" type="bank_account">Bank Account 1</account>
  <account  accountId="2" type="bank_account">Bank Account 2</account>
  <account  accountId="3" type="credit_card">Credit Card 1</account>
  <account  accountId="4" type="credit_card">Credit Card 2</account> 
</accounts>
```

Again, these are pretty small semantic changes but I think they make for a much more readable, logical and understandable API. Obviously I don't know your data model or how these accounts relate to each other but this makes sense to me from the perspective of a consumer of your service. 

### PUT vs POST

`PUT` vs `POST` is something that confuses pretty much everyone I talk to when they first start learning about REST. And the fact is that there is nothing stopping a developer from implementing `PUT` and `POST` methods for their services incorrectly.

First let's look at the specification for `PUT` vs `POST` as per the [w3.org website][2]. 

> The POST method is used to request that the origin server accept the entity enclosed in the request
> as a new subordinate of the resource identified by the Request-URI...

What this essentially means is that when you `POST` an entity to a particular URI, the server should create a new object underneath the requested resource URI, or inside the folder requested. And the response should return the location of the newly created resource. So, as an example, when I `POST` an entity to:

> /rest/api/expenses

The server should create a new expense underneath the `expenses` resource, or folder, and return me the resource location for it, for example:

> /rest/api/expenses/1

If I were to `POST` the exact same entity to the exact same resource, a new entity would be created and the location for that would be returned, ie:

> /rest/api/expenses/2

This is what is meant by the fact that the `POST` method is not idempotent. Each time you call the method, even with the same payload, a new resource is created on the server. And when thinking of the `POST` method like this, the naming of your GET methods, like I proposed, for specific expenses and accounts starts to make sense.

And now lets look at `PUT`:

> The PUT method requests that the enclosed entity be stored under the supplied Request-URI.

What this essentially means is that when you `PUT` an entity to a particular URI, the server stores that entity at the specified URI. So if I wanted to create a new expense with a specific ID, or I wanted to update an existing expense for which I know the ID, I would `PUT` it to, for example:

> /rest/api/expenses/2

`PUT` is idempotent meaning that I can `PUT` to that URI all day and there will be no change to the server state for each subsequent call after the first. I will essentially be saving the same expense over itself for each subsequent call.

Taking all this in to account, I want to put it out there that I think your API for adding an expense and a supplier is slightly wrong. At the moment you are allowing a user to `PUT` to:

> /rest/api/supplier  
> /rest/api/expense

According to the REST spec, this would create the entity at that actual resource URI. Instead, you should, technically, only be able to `POST` to that URI. If you want to support `PUT`, that should really be to:

> /rest/api/suppliers/{id}  
> /rest/api/expenses/{id}

This would essentially be allowing a user to update a particular supplier or expense for the ID specified.

### Casing of URIs

Casing of URI's comes down to personal preference, however, it is important to note that URI's are case sensitive. Therefore, it might be worth thinking about lower casing your URI's (as I sort of snuck in in the section about expenseTypes and paymentMethods) as it stops the user having to think about whether or not a particular resource URI contains upper case characters or not. If you stick soley to lower case, then there won't be any problems with that. Again, this is down to personal preference, so what you go with is up to you.

### Typo

A small cut and paste error, I'm sure, but under the **Add Expense** section you refer to the **Add Supplier service**. Might want to fix that up ;)

### Conclusion

Just to re-iterate, the feedback and suggestions I have for you as follows:

1. Look at your naming of URI's. Think of them like a folder structure on the filesystem.
2. Understand PUT vs POST and ensure that you implement them correctly.
3. Thinking about casing of URI's
4. Fix the typo

Woah, looking back now, this post became way longer than I had imagined it would be. I hope I haven't ranted too much. As I mentioned at the beginning, all the things I raised are things that sprung to mind while reading your API doc that maybe I would have done differently if I were designing the services. Take them all with a pinch of salt but please have a think about them. In particular the `PUT` vs `POST` one. Along with that, it's true, I am only human and therefore I am only speaking based on my experiences as a developer. I have been known to be wrong more times than I could ever remember and there is every chance that I have been wrong in some points above, however, if I can do nothing more than open up some discussion and give you something to think about then I'm happy.

Good work on what you have done with the API and the Crunch app. I have nothing but great things to say about your work and your company so keep up the awesomeness.

Oh, and by the way, Helena is a freaking awesome account manager.

[1]: http://support.crunch.co.uk/attachments/token/t6tkagiuvv95esz/?name=Crunch_API.pdf "Crunch API Documentation"
[2]: http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html "Hypertext Transfer Protocol -- HTTP/1.1 Spec"