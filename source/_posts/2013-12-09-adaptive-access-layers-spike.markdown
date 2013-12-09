---
layout: post
title: "Adaptive Access Layers spike"
comments: true
---

After reading Ulrik Born's MSDN article on [Adaptive Access Layers](http://msdn.microsoft.com/en-us/magazine/dn451442.aspx) (AAL) and hearing him present it at my company, I thought it would be a nice little exercise to start on a spike of an AAL framework.

The AAL spike code can be found on: [github.com/TheoAndersen/AdaptiveAccessLayerSpike](http://www.github.com/TheoAndersen/AdaptiveAccessLayerSpike)

The general idea is that every external dependency of the system as ex. database access, logging etc. is handled by an AAL-framework which dynamically creates the implementation of the interfaces you provide from your business logic, to use the external dependencies. So if you need a logging mechanism, you would create an interface with the right attributes to denote that its an interface providing access to the logger. The framework would then create an implementation for the interface and wire it up to the logging part of the framwork, without the code using the interface knowing about this.

{% img center /assets/aal-class-diagram.png %}

This class diagram shows the structure I've used in the spike. The ITestLogWriter represents an interface created by some business logic. The implementation is dynamically created by the AAL code, which wires it up to the LogContract derived AAL class, which handles the actual logging.

``` csharp
[LogContract]
public interface ITestLogWriter
{
   [LogEntryContract]
   string Log(string message, int second);
}
```

This is how an interface could look like. The implementation that the AAL framework would create to hook this interface would then look like this.

``` csharp
public class TestLogWriterImpl : LogContract, ITestLogWriter
{
    public string Log(string message, int second)
    {
        object[] parameters = new object[2];
        parameters[0] = message;
        parameters[1] = second;
        return (string)base.ExecuteImpl(MethodBase.GetCurrentMethod(), parameters);
    }
}
```
Now this would be generated directly in MSIL code. As can be seen from the class diagram as well, the AdaptiveLayerBase implements an ExecuteImpl method which fits together with the standard IL code. This fits together with an abstract Execute method, which is what the specific AAL layers must implement to act on any message executed on an AAL interface.

Now i wont post any crazy IL generating code here, you can look at the github repo yourself for the part i've tried out, but it doesn't feel to me like very sturdy code. The method i used to find out what IL to write, was that i created the implementation in c# first and then used the "ildasm" tool to disassemble the dll and watch what IL code it compiled into, so that i could use that as a recipe for my implementation. Still its poor error handling you get when the generated assembly just doesn't work, which is also a good reason to keep the generated IL part as small and simple as possible, and keep the normal c# code doing the lifting.

Good reasoning behind an advanced implementation
------------------------------------------------

Now I like the overall reasoning behind this approach, but I'm not sure I like the actual solution. The positives are that you enforce seperation of the external dependencies behind interfaces, to make sure that the business logic isn't tangled to much along with other concerns. The interface without a compile time implementation is though a little special. It makes the wiring of all business logic with external dependencies dynamic, so that every request will go through several 'magic' holes in the code all the time. Debugging will be kind of odd as the current position will disappear and (hopefully) appear the right place on the other side.

I do think that the reasoning behind AAL of separating and generalizing/encapsulating external dependencies behind abstractions as interfaces are important in enterprise applications and that the referred anti-patterns are very real. Even though, it is though possible to achieve this if you continuously refactor and improve the code your working on. Much development seems opposed to continuous refactoring and generalization. Developers often just continue in the existing track, and dosen't think to refactor or change what theyr externding to make it better, making it necessary to come up with an architecture/framework like this up front. I think its impressive that Ulriks team has created this architecture and have extended it as much to really improve the productivity of theyr team, but i would optimally like a team to gradually mold a and continuously refactor a system to keep it easy to use and test, without having to create a framework with restrictions on how the developers have to work (even though its an internally developed framework). If we dont get developers to think critically on the code and refactor it, we will just have to create a new framwork to the business logic as well at some time, to keep them from messing that up, and so further.

#### Sources:
- [Adaptive Access Layers + Dependency Injection = Productivity on MSDN by Ulrik Born](http://msdn.microsoft.com/en-us/magazine/dn451442.aspx)
- [Adaptive Access Layer Spike on Github](http://www.github.com/TheoAndersen/AdaptiveAccessLayerSpike)
- [Simple Adaptive Access Layer Helloworld sample (using Roslyn)](http://gertgregers.wordpress.com/2013/12/05/simple-adaptive-access-layer-helloworld-sample/)
