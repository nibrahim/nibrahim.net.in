---
layout: post
title: "Using buildout"
---

I'm trying to use [Buildout](http://www.buildout.org/) to automate
setting up my development environment. The Buildout documentation
could use some work. I can't really find a clean tutorial
anywhere. This blog post aims to be something of the kind. 

Introduction
------------
Buildout is a system used to *ready* (meaning download, build,
conifgure etc.) software "parts" which are necessary to get a complex
application to run. 

For example, You might have a web based application that requires a
full text indexer, a web server, a database server and some other
components. You can get buildout to set this up for you. Better yet,
you can ship a configuration file to your users who can run it with
buildout to replicate your environment. 


Further references
------------------

