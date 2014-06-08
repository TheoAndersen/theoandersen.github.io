---
layout: page
title: Projects
footer: false
---

This page lists the home development projects I'm currently (or currently not) working on or following. This is not the projects I'm working on proffessionally at the time, but a listing of some of the ideas and home projects I'm hacking on to try and learn more. I'm listing them here both because it would be fun to hear comments on them, and because it makes it more likely that i will actually finish them, as it's a bit emberassing to have them listed here for too long :).

The listed projects are commented with (last updated, status, intended outcome).

## ~~Log site~~ Log parser

(June 2014, In active development, TDD experience + productivity gain at work)

In the current project at work we have a large amount of logging which is used extensively when investigating faults. The procedure is currently that we fetch the logs from the given system at the given date and then search through the files to find the information we need.

The idea for this project was to create a Lucene implementation which contians a scheduled job that fetches the logs and indexes them to a Lucene database, which is used by a website to offer better, more business focues services for searching fast in the log-files.

This would both be cool and would give me practical lucene knowledge. The only Lucene knowledge i have is from a thesis i did on RavenDB, which is a document database which uses Lucene behind the covers.

Status of this right now is that i never could get the Lucene bit to perform. I stopped the lucene approach finding out how to rewrite the indexes to sort them the way i wanted to hopefully improve performance. Now i know that RavenDB does this exact thing with Lucene and they get it to perform nicely, but i never got that far with it. Had it not been because RavenDB isn't free i would propably just have used that for it.

The direction this project has taken is that it's now become an offline tool, to parse logfile's with given filters, to produce a simple html-file with details of the wanted log nicely formated in entries for each entry to the system. The first versions of this is already used on the teams on my project and we are finding it very usefull and timesaving compared to searching the logs manually.

I'm currently extending this tool to make it possible to parse logs from several log files in one go (multiselect rightclick from windows explorer) among other things as CSV renderings etc.


## Let's Code - Test-Driven Javascript

(June 2014, failing to keep up/catch up, learning TDD/node.js and rigorous js development)

Now this is _not_ my project, but James Shore's great screencast series on rigorous, professional JavaScript development. I'm listing it here because this has so far been a great resource of learning proffessional development practices which can be applied to any project.

I'm currently struggling to keep up with James' too see all the material he produces - right now I'm at episode 70 and he's winning at the 160ies... (.. edit: he's much farther ahead now)

[www.letscodejavascript.com](http://www.letscodejavascript.com)

## Golf scorecard

(June 2014, halted, get something on App store)

I'm play golf in my spare time, or, at least i used to have time to play golf. Even though I've always thought it cumbersome to update the little scorecard remembering how many extra strokes i have on every hole, and calculating how I'm playing. Therefore I'm working on a small iOS app to support basic scorecard keeping, which calculates the points etc automatically. This isnt a learning app for me because I've worked with iOS before, but this is an attemt to see if how much it takes to create something finished and possibly interesting and published on Apple's App store.

This is a closed source project, which will first be released in Denmark and will cost as little as i can keep it at.

Right now this has not been in development for some time, as me and my team-mates haven't been able to find time for it, and i've prioritized other stuff. Now with Apple's now Swift language coming out im considering rewriting it in Swift, to use it as a learning exersice as well (Hey this is hobby projects remember? i would 'propably' never rewrite anything at work just because a new exiting language was coming out :D)

## Autobackup

(June 2014, not being developed, learning node.js)

Seeing Amazons Glacier cloud service, which provides low-cost cloud storage, I thought it would be a perfect solution for cloud backups. The trick with glacie is that keeping data stored is low cost and the downside is that it can take a while to be able to fetch your backed up data out of Glacier, which shouldn't matter if its backup data.

To utilize this I've started a little autobackup project in node.js, which is ment to be a service installed on my NAS which would archive and upload my important data (as wedding pictures etc) to Glacier automaticcaly when updated. A nice addition would be a small locally hosted status-website.

So this is my first project to learn node.js and i'm finding a bigger job than i expected to implement enough of the integration with Glacier to make the basic autobackup features work. When i get the first marketable feature of a simple node.js program that will watch and autobackup a folder structure to work, i will upload it to github.

Right now this project has been parked and I'm not developing on it at the moment. This is partly because other projects seems more interesting than this at the moment and because ive come to a halt finding out how to handle intermidient interruptions in the backup.
