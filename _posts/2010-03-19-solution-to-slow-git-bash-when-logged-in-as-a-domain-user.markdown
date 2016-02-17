---
layout: post
title: "Solution to slow Git bash when logged in as a domain user"
comments: true
---
Today i found that my newly installed git on my laptop at work was working pretty slow. 0.5 ~ 1sec delay on loading the git bash, and on every command. I logged in as a local user on the computer and the delay was gone.

Searching the internet a bit, i found that git uses the default home, on my account set to be a network account. This ment that git would look in this directory all the time, causing the delay.

To fix this i created a local user environment variable, overriding the default one, and setting it to %USERPROFILE% which points at c:\users\[username].

![](/assets/slow_git_bash_1.png)

This makes git look the right place for the git.config.   <br />    <br />Next is that when you click the git bash shortcut, i opened on my default homedrive, which was a network connection.&#160; Looking at the properties for the shortcut it looks like this:

![](/assets/slow_git_bash_2.png)

Changing the "Start in" to c:\user\[user], or any other place made the shortcut open much faster for me.

Now my git bash is back and fast again :)
