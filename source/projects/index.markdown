---
layout: page
title: Projects
footer: false
---

This page lists the home development projects I'm currently (or currently not) working on or following. This is not the projects I'm working on proffessionally at the time, but a listing of some of the ideas and home projects I'm hacking on to try and learn more. I'm listing them here both because it would be fun to hear comments on them and because it makes it more likely that i will actually finish them, as it's a bit emberassing to have them listed here for too long :).

The listed projects are commented with (last updated, status, intended outcome).

## Log parser

(Juli 2015, Finished, TDD experience + productivity gain at work)

In the current project at work we have a large amount of logging (+10gb per day) which is used extensively when investigating faults or inquiries. The procedure is currently that we fetch the logs from the given system at the given date and then search through the files manually to find the information we need.

The first idea for this project was to create a Lucene implementation which contained a scheduled job to fetches the logs and index them to a Lucene database. This would be used by a website to offer better, more business focused services for searching fast in the log-files. This would both be cool and would give me practical lucene knowledge. The only Lucene knowledge i have is from a thesis i did on RavenDB, which is a document database which uses Lucene behind the covers.

Status of this right now is that i never could get the Lucene bit to perform. I stopped the Lucene approach finding out how to rewrite the indexes to sort them the way i wanted to hopefully improve performance. Now i know that RavenDB does this exact thing with Lucene and they get it to perform nicely on the same sort of data, but i never got that far with it. Had it not been because RavenDB isn't free i would propably just have used that for it.

The direction this project has taken is that it's now become an offline tool, to parse logfile's with given filters which produces a simple html-file with details of the wanted log nicely formated in entries for each entry to the system. And this is working very well. This tool is now a given help for investigating logs in the system, and we are actively finding more uses in which to extend the logparser for other queries.

## Live mashup

(Juli 2015, Just started, Socket Io / Nodejs experience + something i would like myself)

Some times big events go really viral on the social media, which can be very interesting to follow when its happening, if you have the time. Examples could be natural distasters or terrorist attacks or more positive events as a festival or concert.

An example was a few years ago when Coldplay last held a concert here in Denmark, one local radiostation streamed the whole thing live. Listening to this and looking at Twitter i noticed that a large portion of the audience was tweeting and posting pictures live from the concert on Twitter, Instagram etc.. This was the concert where everybody in the audience got a special wristband when they arrived, which lighted up in an amazing light-show in specific parts of the show, so the concert was visually fantastic as well. What i would love was a streaming site where i could just keep automatically streaming the posts and images from this concert while i was hearing it.

I've never really been able to find a site thats able to stream live trends on social media well for occations like this. What i would love was a site where you can stream posts and images about a topic live, in an animated nice graphical form (HTML5 and CSS3 is pretty nice these days). An extension to this could be to analyse whats trending a bit further. Twitter shows what it thinks is trending, but its not necessarily trending right now, and when you want something animated and streaming you need a topic which is really being tweetet about right now.

The status of this project is that i have most of the server part for the first minimal marketable feature almost done, and need to start working on the front-end part.

## Let's Code - Test-Driven Javascript

(Juli 2015, failing to keep up/catch up, learning TDD/node.js and rigorous js development)

Now this is _not_ my project, but James Shore's great screencast series on rigorous, professional JavaScript development. I'm listing it here because this continues to be a great resource for learning proffessional development practices which can be applied to any project.

I'm currently struggling to keep up with James' too see all the material he produces - right now I'm at episode 134 and he's winning far far ahead of me.

[www.letscodejavascript.com](http://www.letscodejavascript.com)

## Golf scorecard

(June 2015, halted, get something on App store)

I play golf in my spare time, or at least i used to have time to play golf (2 kids now). I've always thought it cumbersome to update the little scorecard remembering how many extra strokes i have on every hole, and calculating how I'm playing. Therefore I'm working on a small iOS app to support basic scorecard keeping, which calculates the points etc automatically. This isnt a learning app for me because I've worked with iOS before, but this is an attempt to see, how much it takes to create something finished and possibly interesting and published on Apple's App store.

This is a closed source project, which will first be released in Denmark and will cost as little as i can keep it at. 

Right now this has not been in development for some time, as me and my team-mates haven't been able to find time for it along with me prioritized other projects. Now with Apple's now Swift language coming out im considering rewriting it in Swift, to use it as a learning exersice as well (Hey, this is hobby projects remember? I would never rewrite anything at work just because a new exiting language was coming out, unless it was a big benifit for the project :D)

## Autobackup

(June 2015, not being developed, learning node.js)

Seeing Amazons Glacier cloud service, which provides low-cost cloud storage, I thought it would be a perfect solution for cloud backups. The trick with Glacier is that it keeps data stored low cost with the downside that it can take a while to be able to fetch your backed up data out of Glacier (which shouldn't really matter if its backup data that you rarely want to fetch).

To utilize this I've started a little autobackup project in node.js, which is ment to be a service installed on my NAS which would archive and upload my important data (as wedding pictures etc) to Glacier automaticcaly when updated. A nice addition would be a small locally hosted status-website.

So this is my first project to learn node.js and i'm finding it a much bigger job than i expected to implement. When i get the first marketable feature of a simple node.js program that will watch and autobackup a folder structure to work, i will upload it to github.

Right now this project has been parked and I'm not developing on it at the moment. This is partly because other projects seems more interesting than this at the moment and because ive come to a halt finding out how to handle intermidient interruptions in the backup.
