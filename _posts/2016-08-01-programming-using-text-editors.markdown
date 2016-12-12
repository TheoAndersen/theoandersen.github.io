---
layout: post
title: "Why program in a text editor"
comments: true
published: true
image: /assets/emacs-open-project.gif
---

I've always lived in Microsofts big IDE Visual Studio, for all my programming
purposes, because I developed .NET. Visual Studio is a monster of an
application - it has its own quircks, but is generally regarded as a pretty good
development tool. Of course all .NET developers swear to Visual Studio, because
they really haven't got anywhere else to go - leaving Visual Studio would sound
like leaving .NET all together. 
And actually the experience in Visual Studio isn't all that bad. Its got good
code suggestions, great debugging facilities - but 90% of its features I either
don't know about, or selectively dont use. 


I on purpose said that I "develop .NET software", but not that I'm a ".NET
developer" - because i want to emphasise that .NET isn't the only programming
platform that I'm interested in. I've spent many hours ín my own time, fiddling
with other programming languages and runtimes, and I've found that the big IDE's
like Visual Studio isn't always the best choice. 



Tools for the polyglot programmer
==

I see my self more and more as a polyglot programmer (*achievement unlocked -
hipster programming word 'polygot' used*). Meaning that I want to know about
more than one programming platform and programming language. I believe that no
one language and platform has got it all right, and learning multiple languages
will make you a better programmer overall, even though you don't use all these
languages professionally. So I've looked into Nodejs and Ruby, and later more
functional languages as Erlang, Elixir and Elm. 


But looking into those languages, I couldn't use Visual Studio - so i kindof had
to jump ship to another thing.

Somehow I ended up on Emacs. I think it was because I saw a developer at a
conference work through a test driven javascript example - and i just couldn't
believe the editing speed of this guy. Everything on the page was changing a lot
faster than I was used to, and i really hate waiting on the editor. It turns out
that these text-editors, has a lot of tricks that Visual Studio didn't - and
that they're much easier to extend and customize, if they don't have what you
need.

Text searching and code navigation
==
The old big IDEs as Visual Studio relies a lot on mouse navigation, and most
developers always have the source explorer open, so that they can select the
right files. Visual Studio do have text search capabilities and code navigation,
but they fall far short of what decent text-editors have to offer.

In my emacs-setup i have a few text-search options that are always available no
matter what programming language, because they're only text based. First of i
can search through the files in a project, getting fuzzy matched suggestions
thats updated as i type, by pressing a simple short cut.

The gif below shows how I can open projects and select a file in that project.
First press a short-cut and type to search for the project, when selected i do
the same for selecting a file in the project.

{:.center}
![Open a project in emacs](/assets/emacs-open-project.gif){:height="300px"}

Next below I have coupled emacs with silver-searcher, (grep on steroids) which
is a pretty fast text-searcher. Which allows me to search in all text in the
project getting the same fast suggestions.

{:.center}
![search in text](/assets/emacs-search-text-in-project.gif){:height="300px"}

It may seem simple, but this fast text-searching, is often times quicker, than
using a search that knows about the code, and supports regex if you need to
match something more complex.

Project support
==
Most IDEs of course include some kind of project support. But these are
typically based on a specific project-implementation as used in a particular
language/runtime. As an example Visual Studio has its solution files and project
files.

A text-editor as emacs isn't related to a specific programming language, and
therefore typically has basic project support in a way that can work in many
language. The typical module thats used for projects in emacs, detects a project
based on your source versioning as defined in a git repository or other VCS.
This is easy and simple, and works rather well.

The trend is turning towards text editors again
==
Wiki describes an IDE as an integrated programming environment - and normally
consisting of a source code editor, build automation tools and a debugger. I
remember making fun of those who programmed html in plain notepad on windows in
the old days. But using a text editor isn't at all like that - you can have
almost everything an IDE has with your text-editor now a days - but you'll need
to tailor it your self.

A text-editor is good at... you guessed it.. editing text. But where an IDE
usually comes preequiped with everything you need to program. An IDE can be
setup to have everything you need - if you set it up - and maybe code a bit too.

Pros and cons of text-editors
==
So IDEs are typically tailormade all-included to support a single programming
language or platform. This means that many of the language specific features
will be included, as refactoring-support and inbuilt debugging without
configuring anything. 

But if you need to program anything else, it probably wont be supported, and
you'l need to learn another environment.

Using text-editors, you typically get better text-editing and searching
capabilities. You will need to be a bit more geeky, as you will need to
customize it and maybe program it a bit your self (a lot if you choose emacs).
And the more advanced language refactorings and features wont supported as well
as the IDE.

I do find that I am more productive in a well customized text-editor, even if
its with only the most basic language features. 

I do all my javascript/typescript/html editing in Emacs, but C# programming is
still Visual studio, as its still better integrated to this style of programming
than the plugins supported for emacs. But there are people working on this, and
one day i might be convinced to jump, and then I'll be able to use all the other
text-editing tricks in my C# programming, that i haven't gotten now in Visual
Studio.

Are you annoyed with your slow IDE? Try a text-editor, but give it some time and
customize it to your liking before you decide. And if want to try something even
more powerful and you really like programming - try emacs. :)
