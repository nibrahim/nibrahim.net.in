---
layout: post
title: "UNIX command line course"
date: "2013-08-03 19:04:06 +0530"
---

I grew up on DOS and then jumped straight to UNIX (Solaris and then
Linux) without really using Windows. I was introduced to "The UNIX
programming environment" by Kernighan and Pike by a friend and studied
the basics of using the shell from it. Most of the tricks stuck with
me since then and while I've never used shell for a production
program, it's always my go to tool for throw away scripts and glue
programs. A lot of what I've picked up over the years has evaporated
but the general feeling that anything is possible once you have shell
access has stayed and has been proven true repeatedly.

I have, however, seen ace programmers who aren't comfortable with the
command line and repeatedly resort to using the mouse or larger GUI
based programs (often less powerful that their CLI counterparts) to
get simple things done. The sharper ones write scripts in their
favourite languages to get small things done while the slower ones do
them manually by hand. Recent posts like
[this](http://www.gregreda.com/2013/07/15/unix-commands-for-data-science/)
which was popular on hacker news point to an education problem and
since I'm fond of teaching, I've decided to put together a course on
command line UNIX and shell scripting. I hope to do it in association
with [HasGeek](http://hasgeek.com) in Bangalore just like I do my git
workshops.

The plan, atleast for now, is to cover the following

1. The command line - Mostly editing, navigating, working with the
   history, prompts etc.
2. Shell as a language - Text manipulation commands (uniq, cut, paste,
   sed, awk etc.), redirection, pipes, substitutions.
3. Shell scripting proper - Writing scripts that do little
   tasks. Useful in places like cron jobs, monitoring scripts, log
   analysers and such.
4. Power tools - Tools like `netpbm`, the imagemagick suite, `netcat`,
   `ffmpeg`, `make` etc. all of which perform tasks in their own
   domain amazingly well but which are unexploited due to their
   interfaces being unfamiliar.

I don't know how long it will be. 2 or 3 days maybe. We'll see. The
primary references I have with me are the book I mentioned above,
[The advanced bash scripting guide](http://www.tldp.org/LDP/abs/html/),
The
[UNIX power tools](http://www.amazon.com/Unix-Power-Tools-Third-Edition/dp/0596003307)
book, The
[Unix guru universe](http://www.ugu.com/sui/ugu/show?I=ugu.home)
website and the [commandlinefu](http://www.commandlinefu.com/)
website.

I'll start working on this in mid August. It should be ready in a
month - that's mid September. I'll make the course available hopefully
in October. Suggestions for books and other materials are welcome as
are any other bits of feedback or criticism.
