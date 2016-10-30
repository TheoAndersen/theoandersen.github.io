---
layout: post
title: "You dont need to see my nulls"
comments: true
published: true
---
In most programming languages, _null_ is the most common thing in the world, and every programmer knows that an object can be 'empty', or in other words have the value _null_, _nil_ or _undefined_ depending on the language.

In this post, I'll try to show you, why a language should'nt need _null_ in the way that most programming languages use it today.

{:.center}
![You dont need to see my nulls](/assets/you_dont_need_to_see_my_nulls.png)

The first person to invent the concept of _null_ was Tony Hoare, a British computer scientist, which has sais the following about his _null_ invention.

> I call it my billion-dollar mistake. It was the invention of the null reference in 1965. At that time, I was designing the first comprehensive type system for references in an object oriented language (ALGOL W). My goal was to ensure that all use of references should be absolutely safe, with checking performed automatically by the compiler. But I couldn't resist the temptation to put in a null reference, simply because it was so easy to implement. This has led to innumerable errors, vulnerabilities, and system crashes, which have probably caused a billion dollars of pain and damage in the last forty years.
> - Tony Hoare

Why does he say this? It seems natural that an object could be _null_, otherwise it will just have some default value like an _int_ defaulting to 0 right?

Actually null is totaly avoidable, which is what Tony.. err.. Sir Tony Hoare was talking about. Lets look at why.

# _Object reference not set to an instance of an object_
_Null_ is the cause of the most common exception in the world. 

{:.center}
![Object reference exception](/assets/object_reference_exception.png){:height="150px"}

Because objects can implicitly always be _null_ in languages like Java or C#, these languages are prone to the common "Object reference" error, when you try to access the properties of an object which was not there.

I'm not even going to show you a code example, because I know you have seen this error plenty of times.

There is several strategies for avoiding it.

# Returning _null_ instead of failing
Usually you will have to foresee when a use of an object can give this error, and guard for it. In newer C# versions, a new operator has been added which makes it possible to just return null, if the object use is null.

```csharp
public Customer fetchCustomer()
{
    return null;
}

public string DoSomething(Customer customer)
{
    Customer customer = fetchCustomer();
    
    // old version which fails..
    return customer.Name;
    
    // newer one.. which returns null
    return customer?.Name;
}
```

Now this is at least a shorter version, than using a guard clause. But even if we use this, it will be difficult to figure out where the null was actually returned, and where the problem was.

# Null object design pattern
In object oriented languages, there's a design pattern which can be used to avoid _null_ problems - at least in the core of the application. An its something like this.

<p class="center">
<img src="http://yuml.me/diagram/scruffy/class/[Customer{bg:yellow}]^-[NoCustomer]" >
</p>

When you initialize your object from a database or similar, you use a subclass null object, instead of the real one to represent the non existing object. In this case the core logic of the application, which works on Customer objects, will instead work on a subclass of Customer called _NoCustomer_, which returns relevant data for when there really is no customer.

In this way the core logic wont need to have all the if statements, for guarding against null - and can pretend like this really cant happen. But it doesn't really remove the issue, but forces it to the boundaries of the application. You still have to remember to use the Null object.

# Making _null_ errors impossible all-together
What I think sir Hoare was really inferring to, was that its not necessary to, and indeed dangerous, to have the implicit option of a null in your code. 

In fact some languages, dosen't allow nulls at all.

Then how to we model _null_? - we model it explicitly instead. This is also much better, as it makes it impossible to forget about the possibility of something not being there.

Lets look at an example.

One language that disallows null, is Elm. 

The same example as before, will look like this in Elm.

```haskell
import Html exposing (..)

type alias Customer =
  { name: String }

fetchCustomer : Customer
fetchCustomer =
  (Customer "Theo")

main =
  text fetchCustomer.name
```

Now this example can never return null, because the type of fetchCustomer states it should always return a ```Customer```. In Elm there are no types which can be null or empty, without you explicitly stating that it can, and the code using that type, explicitly handing every option. The compiler will simply fail to build your code if you don't do this.

A way to add the possibility for fetchCustomer to not being able to find a ```Customer```can be added by changing the type to.

```elm
fetchCustomer : Maybe Customer
```

```Maybe``` is a built in type, which wraps around another type, to specify that the type might not be there. It does this by being either just the value ```Just Customer``` or simply ```Nothing```.

So when we change fetchCustomer to a ```Maybe``` type as shown and recompile, the compiler gives us this straight away.

{:.center}
![Null compile error in elm](/assets/null_error_in_elm.png){:height="250px"}

This states that now ```fetchCustomer``` returns a ```Maybe Customer``` where the Customer type may not be there. The rest of the code has take this into account.

Sweet. 

The compiler refuses to let us write code that will fail because of null at runtime.

The solution could be to change main to this.

```haskell
main =
  text (case fetchCustomer of
          Just customer -> customer.name
          Nothing -> "no customer found :(")
```

So.. no more code coincidentally failing because we did not foresee that an object could be null.

You really don't need to see my nulls. 




