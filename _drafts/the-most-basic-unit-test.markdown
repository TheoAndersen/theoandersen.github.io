---
layout: post
title: "A very basic unit test"
comments: true
published: false
draft: true
---
Today i realized what the most basic form of a unit-test is.

It's this:

{% highlight csharp %}
// the account must be reapproved
if(Account.Type == AccountType.Normal)
{
  Account.ApprovalExpiryDate = DateTime.Now;
}
{% endhighlight %}

I was looking at a similar piece of code on the project i'm currently developer on at work, and was wondering why the first line of comments were there (i hate unnecessary comments, which clutter the meaning of the code). And then i realized that even though the first line of comments is clearly repeating whats really easily read in the expression in the if statement, it is making sure that that's what should be in the if-expression. If someone were to change the expression we would still have that line of code to make sure that the expression was what was originally intended.

Normally i wouldn't write the first line of comments, but only the second as it describes why we set the accounts ApprovalExpiryDate to the current DateTime, which isn't very clear. Even better would though be to extract the line in the if into a method that expressed what we try to explain in that comment line (SetAccountToBeReapproved() or something).

Now this is a stupid example yes, but the testing mindset is there. The best solution, and next step would then be to have a unit test check this, and enforce it automatically instead.
