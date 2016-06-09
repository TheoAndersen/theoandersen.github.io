---
layout: post
title: "Use the source Luke"
comments: true
published: false
---
So, there's almost a new version of ASP.NET out - [ASP.NET Core](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx) as its called now. Having some vacant time at work, I decided to take a look at it. Being that ASP.NET Core isn't out yet, the documentation isn't entirely fulfilling, and I quickly came across parts that i couldn't get to do what i wanted, but then I realized; Hey - its open source now. I can just look at the implementation of the API I'm having difficulties with and figure it out myself. So i found the sources on [Github](www.github.com/aspnet) and was actually able to worked my way through the problems, using the real source - sweet!

Being able to use the real source of the libraries you use, is a luxury that .NET developers havent really gotten used to - yet. We've all tried stepping into minified JQuery code, or maybe disassembling some library to try and figure out why the library does that strange thing. But there is real value in being able to stop into the framework you're using - you don't have to consider the .NET libraries a black box anymore, and their not all that frightening to look at. Chance is you've propably said it once your self "The best documentation is the code".

So, looking at the ASP.NET Core code on Github got me through some initial problems, but it would be really neat to be able to just step my way into the framework code, instead of having to switch between code and Github in the browser. This though was a bit harder to figure out initially than expected.

### How its supposed to work

The only material I have been able to find about debugging through the source is [this old blogpost](https://blogs.msdn.microsoft.com/webdev/2015/02/06/debugging-asp-net-5-framework-code-using-visual-studio-2015/) and [this issue](https://github.com/aspnet/Home/issues/1293) i have filed on the aspnet github repo trying to get some details on the same. But the procedure, as I understand it from the documentation, should be something like this:

Add a solution, along with a `global.json` file, in which you put a `sources` property where you state the paths to your sources. if one of the sources points to a library source of which you have a dependency to in your `project.json`, it will get used instead of the library package. Oh, and it seems the `sources` property have been renamed to `projects` some time, because thats what's in the current scaffoled projects in my current Visual Studio 2015 version.

### Using the source, to figure out whats going on

The program which wires these dependencies up is the .NET Execution Environment (DNX). So we need to look in the sources of that, which can be found on github [here](www.github.com/aspnet/dnx), to figure out whats really going on.

Now we know that there should be something looking into the `global.json` file, if its there. So starting by searching for that in the sources, we find it a few places, but one class named `GlobalSetting.cs` in the Microsoft.Dnx.Runtime namespace seems to be it (look [here](https://github.com/aspnet/dnx/blob/72d8a95c584b25a4b150b735407c1e5d7605a148/src/Microsoft.Dnx.Runtime/GlobalSettings.cs) on the github repo).

Looking in the implementation we can already see that the DNX seems to accept both the property `projects` and the seemingly older name `sources`, so both of them should work.

{% highlight csharp %}
var projectSearchPaths = jobject.ValueAsStringArray("projects") ??
                         jobject.ValueAsStringArray("sources") ??
                         new string[] { };
                         
globalSettings.ProjectSearchPaths = new List<string>(projectSearchPaths);                         
{% endhighlight %}





