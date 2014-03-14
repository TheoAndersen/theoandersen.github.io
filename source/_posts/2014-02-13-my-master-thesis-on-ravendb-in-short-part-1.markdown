---
layout: post
title: "My master thesis on RavenDB in short(ish)"
comments: true
---
Last year I finished a professional master in Software Construction, in which I wrote a thesis named "A case study of the document database RavenDB". Like most enterprise developers I know, I've done pretty much all of my work using relational databases, but I continue to be intrigued by the differences of NoSQL databases compared to the well known relational and wanted to investigate these differences further, which were the prime motivation for choosing this topic. I admit that I did a crucial error by writing it in my native language danish, because it means that not many are able to read it - you're welcome to try and google translate it ;)). This is though the reasoning behind a couple of blog post I mean to write, reporting the main point of the findings on this blog.

The project was to investigate the .NET document database RavenDB on four main points, comparing it to the relational database Microsoft Sql Server. This was done by building a prototype application and testing selected view-points on both a version of the application using RavenDB and one using Microsoft SQL Server.

In short the points of investigating which the prototype was implemented based on was:

 - Modelling
 - Performance
 - Ad-hoc querying
 - Extendability

This will be a series of multiple blog posts, but the necessary place too start will be too sum up some background theory on NoSQL which I'll skip over pretty quickly because there's so much material about this everywhere on the web. But the posts that follows will hopefully be a bit more concrete. I hope that this will provide yet another little bit of info into the differences between a typical document database as RavenDB and a relational database.

NoSql - what a confusing name
-----------------------------
"NoSql" have become a widely adopted term meaning "Any database which is not the normal, big old relational databases as Sql Server, Oracle, MySql etc.". But the name is misleading as it's not at all about _not_ using SQL, but more about using a different model for storing the data than the relational model and focusing less on being fully transactional and more on performance, horizontal scaling (more servers) and easier development (as having a dynamic schema).

All NoSQL database approach and handle these aspects differently, but some categorization based on the chosen model can be made by categorizing them in; key-value store, document, graph and column stores.

I've chosen to investigate RavenDB which is a document database, because I think modeling data in documents isn't that far off from  the typical enterprise scenarios i normally work with using relational databases. And as I work professionally with .NET, RavenDB was the obvious choice for investigation because of its tight integration to .NET, through a Linq supported API.

BASE - It's anything but ACID
-------------
Where ACID is a familiar term describing transactional databases (as ex relational), its counterpart BASE describes something that's Basically available, Soft state, and Eventually Consistent. The main thing here is that you can get better performant systems, if you don't need the system to be fully transactional, consistent or available all the time. It also makes it much easier to make systems that scales better.

<p align="center">
  <img src="/assets/TeoriAcidBase.png" width="500" style="valign: center"></img>
</p>

As opposed to the relational databases, which by default tries to enforce strict transactionality on everything it stores, most NoSQL databases are known for supporting BASE and not having ACID transactions. But this does not have to be one or the other. People usually think of this as being something that's ultimately chosen by choosing a particular database, but it doesn't have to be. The architecture of a relational database makes it transactional by default but it is possible to model BASE in relational systems. Its what we do when we create performance/buffer -tables and create batched jobs that update these tables every once in a while. By default RavenDB supports simple reads and writes as ACID transactions but more complex queries are BASE.

In the next blogpost I'll describe the basic differences in the conceptual architecture differences behind the relational database and a document database as RavenDB, which will uncover this part a bit more.

#### Sources
<ul>
<li>D. Pritchett. Base: An acid alternative. Queue, 6(3):48–55, 2008.</li>
<li>N. Leavitt. Will nosql databases live up to their promise? Computer, IEEE, 43(2):12–14, 2010.</li>
<li>S. Gilbert and N. Lynch. Brewer’s conjecture and the feasibility of con- sistent, available, partition-tolerant web services. ACM SIGACT News, 33(2):51–59, 2002.</li>
</ul>
Just search for "NoSQL", "Acid" or "Base" on Google :)
