# Workshop: Band-App

Workshop to build an app to help bands

## Prerequisites

- [Node.js](https://nodejs.org/en/) (includes npm)
- [LoopBack](http://loopback.io/)

## What are we building?

From your host, [Joe Sepi](https://github.com/joesepi):

>"I have been in bands since I was young and am always wishing I had some app to help me and my band in some way. I'm going to use all of those itches to drive a workshop building out a full-featured app to help today's DIY bands."

## Overview

In this workshop, we will go through the steps to build an app (and eventually a platform) that satisfies some of the needs of DIY band---cataloging shows (past and present), cataloging albums, managing press kit materials, and more---but it could be any app as these are pretty basic relational data needs.

For example, we will start out cataloging shows/performances. We then abstract out the other artists (on the performance bill) and the venues into their own models and create relationships between them. We will go through devops related tasks such as deployment, linking services (DB) and creating a toolchain for continuous delivery/integration. We will build an admin interface for our data and eventually we open it up to other bands as a platform. You can imagine the same sort of development flow for other things -- from a technology stand point (developing code) as well as from a product standpoint (developing ideas).

## Get started

Begin with the [Getting Started](workshop/00-getting-started.md) page to learn more about LoopBack and how to start the workshop.

**Code repository:** [https://github.com/StrongLoop-Evangelists/band-app](https://github.com/StrongLoop-Evangelists/band-app)

**The Band App on Bluemix:** [http://band-app.mybluemix.net/](http://band-app.mybluemix.net/)
