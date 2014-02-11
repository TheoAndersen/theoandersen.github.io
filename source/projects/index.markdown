---
layout: page
title: Projects
footer: false
---

This page lists the home development projects I'm currently (or currently not) working on or following. This is not the projects I'm working on proffessionally at the time, but a listing of some of the ideas and home projects I'm hacking on to try and learn more. I'm listing them here both because it would be fun to hear comments on them, and because it makes it more likely that i will actually finish them, as it's a bit emberassing to have them listed here for too long :).

The listed projects are commented with (last updated, status, intended outcome).

## Autobackup

(december 2013, in initial development, learning node.js)

Seeing Amazons Glacier cloud service, which provides low-cost cloud storage, I thought it would be a perfect solution for cloud backups. The trick with glacie is that keeping data stored is low cost and the downside is that it can take a while to be able to fetch your backed up data out of Glacier, which shouldn't matter if its backup data.

To utilize this I've started a little autobackup project in node.js, which is ment to be a service installed on my NAS which would archive and upload my important data (as wedding pictures etc) to Glacier automaticcaly when updated. A nice addition would be a small locally hosted status-website.

So this is my first project to learn node.js and i'm finding a bigger job than i expected to implement enough of the integration with Glacier to make the basic autobackup features work. When i get the first marketable feature of a simple node.js program that will watch and autobackup a folder structure to work, i will upload it to github.

## Let's Code - Test-Driven Javascript

(december 2013, trying to keep up/catch up, learning node.js and rigorous js development)

Now this is _not_ my project, but James Shore's great screencast series on rigorous, professional JavaScript development. I'm listing it here because this has so far been a great resource of learning proffessional development practices which can be applied to any project.

I'm currently struggling to keep up with James' too see all the material he produces - right now I'm at episode 70 and he's winning at the 160ies...

[www.letscodejavascript.com](http://www.letscodejavascript.com)

## Log site

(december 2013, not started, learning Lucene)

In the current project at work we have a large amount of logging which is used extensively when investigating faults. The procedure is currently that we fetch the logs from the given system at the given date and then search through the files to find the information we need.

The idea for this project was to create a Lucene implementation which contians a scheduled job that fetches the logs and indexes them to a Lucene database, which is used by a website to offer better, more business focues services for searching fast in the log-files.

This would both be cool and would give me practical lucene knowledge. The only Lucene knowledge i have is from a thesis i did on RavenDB, which is a document database which uses Lucene behind the covers.

## Golf scorecard

(december 2013, working on first feature for way too long, get something on App store)

I'm play golf in my spare time, or, at least i used to have time to play golf. Even though I've always thought it cumbersome to update the little scorecard remembering how many extra strokes i have on every hole, and calculating how I'm playing. Therefore I'm working on a small iOS app to support basic scorecard keeping, which calculates the points etc automatically. This isnt a learning app for me because I've worked with iOS before, but this is an attemt to see if how much it takes to create something finished and possibly interesting and published on Apple's App store.

This is a closed source project, which will first be released in Denmark and will cost as little as i can keep it at.
