---
layout: post
title: "My org mode PIM setup"
---

<link type="text/css" rel="stylesheet" href="/css/org-highlight.css"/>

This post describes the way in which I've set up
[org-mode](http://orgmode.org) to track my work and life in
general. It's broken down into a couple of "sections" and I've linked
to the relevant parts of my config on my github account. I've tried to
be a little verbose to explain how things work so that you can try
this out if you're inclined. If you hit any snags, mail me and I'll
try to help you out.

All this is stuff that I use on a daily basis. I'm not as organised as
I like to be but I mostly practice what I'm about to preach in this
post. This covers only the PIM part of my setup. I also use org for
some authoring which I haven't discussed here.

Also, my emacs configuration files are in need of cleanup. They're
functional as far as config files go thinking of them as "programs"
makes me retch. 

History
-------

I've messed with quite a few ways of keeping myself "organised". I
took a course on the Franklin Covey system and for a while, kept my
weekly and monthly sheets up to date. I tried a bunch of programs that
helped me track time, keep notes and organise myself in general. None
of them really worked and in retrospect, there were a few reasons

1. They were hard to "Get back into" - Everyone makes mistakes and
stumbles when trying out a new system. If it's hard to reset and
get back into the flow when you mess up, the inertia is hard to
beat and you end up ditching the system altogether.
2. They were hard to tie up into other spheres of my life - Like
most people, I get todo items, information etc. from various source
(e.g. email, phone calls, personal conversations etc.). While your
system needn't be omniscient enough to track it *all* in a single
place, it should be powerful enough to do *most* of what you need
so that your mental strain is reduced. A smaller variant of this
problem is the inability of most software systems to integrate with
the rest of your work. 
3. I'm lazy, disorganised and a disgrace to humanity - No point
denying this. But once I realised and accepted it, it's given me a
set of constraints to work under.

I tried GTD but the "process" doesn't work for me (although I will
admit however that it was better than the Franklin Covey
strategies). It did however solidify my ideas of what I wanted out of
a task tracking system. I'll try to summarise below

1. One place for to look for things. I don't want to have to wade
through sheets of paper and jump through programs just to figure
out what I have to do. Did I pay this months rent? Have I paid my
utility bills? What work do I have to do today? What was that
website with the colour pickers that I used last week? All should
be in a single place.
2. Should be quick. If someone tells me something that's I have to
work on (later or immediately), I need a way of grabbing it
immediately before my attention deficit addled brain discards
it. If software, I want the kind of thing where I can develop
muscle memory and forget about the mechanics of doing things. I
especially value this given my email reading experiences with mutt
which is still in my opinion the fastest way to read and reply to
emails.
3. Should never get lost. I don't want to end up doing the "Did you
ask me to do that? Damn. I was sure that I had starred that email"
dance.
4. Should be "fun". Being a tinkerer, I like stuff that's fun to do
and with a few knobs and wheels. If they're missing, I'll probably
get bored and give up.
5. Flexible. If I want to, from tomorrow say, start tracking
habits, my system should be flexible enough to accommodate that.
   
So I flirted with the idea of writing some command line scripts to do
this kind of thing and found [todo.txt](http://www.todotxt.com/) which
"worked" but didn't really cut it for me. 

My suite of "productivity tools" consisted of something called
"gtimer" (I think) which was a simply app to keep track of time spent
on projects,
[EmacsWiki](http://mwolson.org/static/doc/emacs-wiki.html) (the
precursor to [Emacs muse](http://mwolson.org/projects/EmacsMuse.html))
and [TomBoy](http://projects.gnome.org/tomboy/) to keep notes (thereby
violating the one place only rule), a notebook and pen to track todo
items and a couple of ad hoc thing like starred emails etc. to keep
track of "projects". No, it didn't work. Yes, I wanted something
better.

I had used the
[Emacs outline mode](http://www.emacswiki.org/emacs/OutlineMode) and
hated it but then came across
[Carsten Dominiks excellent Google tech talk](http://www.youtube.com/watch?v=oJTwQvgfgMM)
on org mode. I tried it and it was love at first sight. Never looked
back since. 

Now, let's get onto the meat of the matter.

Current setup
-------------

My org mode files are stored in a git repository that I keep on my
private account. I don't put it up on github since it contains a lot
of personal information and I don't want anyone to clone it and send
me a pull request. 

The layout of the files is as follows

    .
    |-- non-org
    |   |-- bank-statements
    |   `-- invoices
    |
    |-- official
    |
    |-- pycon
    |   |-- 2009
    |   |-- 2010
    |   |   |-- IPSS
    |   |   |-- logos
    |   |   |-- presentations
    |   |   `-- pycon docs
    |   `-- 2011
    |-- specs
    |-- training
    |   |-- business
    |   |-- materials
    |    `-- outlines
    |
    |-- donotdo.org
    |-- habits.org
    |-- ideas.org
    |-- projects.org
    |-- projects.org_archive
    |-- reference.org
    |-- refile.org
    |-- scratch.org
    `-- toread.org
    
There are a few parts that are not heavily accessed or modified and
they have accumulated cruft.    
    
The `non-org` directory contains stuff like exported PDFs, `.odt`
files and stuff which contain invoices etc. Mostly binaries. The
`official` directory contains agreements and other formal
materials. It should probably be a sub directory of the `non-org`
one. `pycon` is a top level directory mainly because it was one of my
"big" projects and I was experimenting with org-mode features. `specs`
contain org-mode files which are outlines of projects I'm thinking
about. It serves as my notebook and doesn't have anything in that I
track. A digital pocketbook of sorts. `training` contains all the
stuff that I use while taking and planning trainings. Course materials
(e.g. [this](http://nibrahim.net.in/self-defence/) was written as an
org file), outlines which businesses want, different versions of my
training profile etc. `donotdo.org` is a file that contains a list of
harsh lessons that I've learnt over time. It might seem silly to put
them in a text file and forget them but I find the act of writing it
down useful. `habits.org` is a file I'm trying to use to track a few
habits I'm working on. It's not yet "production" ready so you can
ignore it. `ideas.org` is a bucket where I dump semi-detailed ideas
which occur to me. Sort of a cold freezer to search for things to do
when I have time. Not necessarily computer related. `projects.org` is
my *main* file around which my life revolves. Most of this article
will be discussing that. There's an `_archive` version of the same
file which I use to keep older subtrees. `reference.org` contains
phone numbers, links, addresses, things which I would forget if I
didn't write them down. `refile.org` is my default capture target
(I'll discuss this later). `scratch.org` is a sandbox file I use to
pimp features to people when I'm in front of them and for writing out
small things which I can export to different formats. `toread.org` is
a file that stores URLs, names of books, links to videos, movies
etc. which I plan to look at. It should really become a section in
projects but like I said, the layout needs work.


The first thing I'll discuss is the `refile.org` file which is where
all tasks land up before they are moved off into the right places. The
process of putting them there is something worth mentioning and it
addresses point 2 in my list of requirements. 


Data sources
------------
Things come into my life via. email, real life (phone + meeting
people), the web and while coding (Ah! I should do this, fix this
later). This might seem like an oversimplification but it works.

The intention here is to capture things that come in via. these
channels into our PIM. Once that's done, it becomes a regular "item"
that can be managed using our regular workflow. The capturing process
has to be quick and non-intrusive. 

Capturing
---------

Org has a somewhat new system for "capturing" things into
org-files. The idea here is to get things into your system quickly
rather than think about where to file stuff. 

The idea is quite simple. You define a bunch of
[templates](https://github.com/nibrahim/Config-files/blob/master/emacs/local-lisp/major-modes/nkv-major-modes.el#L28)
and then globally assign a
[key](https://github.com/nibrahim/Config-files/blob/master/emacs/local-lisp/major-modes/nkv-major-modes.el#L26)
which you can hit anywhere to "capture" something. 

Let's take email as an example. Suppose I get an email that asks me to
"do something", I hit `C-c r` and my org-capture buffer pops up

![Org capture interface](/images/screenshots/org-capture.png)

This allows me to capture the email using any of these templates. If
(for example), it's not a TODO but just something I need to keep track
of (e.g. a phone number or an address), I'd hit `r`. It would allow me
to fill some stuff extra information in, annotate it with a link back
to the email and add an entry to my `reference.org` file. Now, it's
safe. 

If it was an email asking me to do something (e.g. fix this bug), I'd
hit `t` and it would open a buffer asking for information. Usually, at
this time, if I can quickly think of when I want to get to that, I
schedule it using `C-c C-s` and put a date in there so that I don't
have to worry about that later. This would get dropped into
`refile.org`.

If I'm working on the openlibrary code base and see a little bit of
code that I need to fix but don't want to interrupt my current flow,
hit `C-c r` and capture it using `o`. It captures it as a TODO and
drops it into `refile.org`. This works as a trackable `/*TBD*/` in
your code. I keep this separate from the regular TODO because it's
work related and large enough to be treated separately.

The `j` is used when I want to write a diary entry. I'm probably going
to get rid of this since I can't really track or mine journal entries
and it's more fun writing them on paper anyway.

So, at the end of the day, the `refile.org` file might look something
like this

<pre class="org">
<span class="org-level-1">* </span><span class="org-todo">TODO</span><span class="org-level-1"> Fix default assigneed for support cases    </span>
  <span class="org-special-keyword">SCHEDULED:</span> <span class="org-date">&lt;2011-07-19 Tue&gt;</span>
  <span class="org-link">Email from George Oates: Fwd: Re: Default assignee</span>
<span class="org-level-1">* </span><span class="org-todo">TODO</span><span class="org-level-1"> This task needs more logging</span>
  <span class="org-special-keyword">SCHEDULED:</span> <span class="org-date">&lt;2011-07-19 Tue&gt;</span>
  <span class="org-date">[2011-07-17 Sun 14:33]</span>
  <span class="org-link">file:~/github/nibrahim/openlibrary/openlibrary/tasks.py::@oltask</span>
</pre>

For real life things which occur while I'm at the computer (e.g. phone
calls etc.), I just fire a capture and record it as a TODO. For things
that happen while I'm away from my computer, I use a stack of paper
pieces I carry with me where ever I go and write down what I need to
do. One task per piece of paper. I have a little ritual of putting
these tasks into my system every night before I hit the sack which
I'll come to later.

At the end of this process, everything is either in the `refile.org`
file or the `references.org` file. i.e. within my control. The next
thing to manage them properly.

Refiling
--------

At the end of every day, before I crash, I have a little refiling
ritual. The bottom line is to empty the `refile.org` file. If this
file has entries in it that are more than a day old, you know I've
been procrastinating.

`org-refile` is bound to `C-c C-w` by default and can be configured to
present you with a list of possible trees into which you want to move
an entry. My `projects.org` file used to be a directory of multiple
files each dedicated to a different area of my life
(e.g. `personal.org`, `work.org` etc.) but I moved them all into a
single file under different headings.

You can see my refile configuration here. It uses `ido` to make the
refiling process smoother and more "Emacs like". Here's what a
refiling looks like

![Org capture refile](/images/screenshots/org-refile.png)

It's asking me where I want to move this item to and offering me the
four headings in my `projects.org` file. I select one and it gets
"refiled".


Scheduling and agendas
----------------------

Org-mode has an extremely powerful scheduling and agenda system. The
basic idea is that you schedule your tasks for a specific day and/or set
deadlines for them and then ask org mode to generate a view that
prints out what you should be doing today, this week, this month or
this year. The scheduling system is flexible enough to handle
repeating tasks (e.g. pay rent on 10th of each month), effort
estimates, clocking etc. 

I personally use just the basic scheduling capability. 

So, when I refile, I try to schedule stuff for certain dates. There
are some tasks which need to be done "at some time" for which I
randomly schedule them a few weeks from now and forget about them till
they pop up on the agenda again. Some things are already scheduled
while capturing so that's cool. Some, I know when I will have time to
do them so I schedule them appropriately. In any case, the process of
refiling makes sure that scheduling is done. The dates needn't be
accurate but tasks should have *some* date when you'll look at them.

Once that's done, `C-a a` lists out my agenda for the current week
(focussing on the current day) and I can get to work. Here's a sample
of my agenda from last month (the capture templates were different
back then so it's slightly different from what you'd see currently).

<pre class="org">
<span class="org-agenda-date">Monday     20 June 2011 W25</span>
<span class="org-agenda-date">Tuesday    21 June 2011</span>
<span class="org-agenda-date">Wednesday  22 June 2011</span>
<span class="org-agenda-done">  projects:   Scheduled:  </span><span class="org-done">DONE</span><span class="org-agenda-done"> </span><span class="org-link">Email from George Oates: Support System - bugs/tweaks l</span><span class="org-agenda-done"> [5/5]</span> <span class="org-agenda-done"><span class="org-tag">:tbd:</span></span>
<span class="org-agenda-done">  projects:   Scheduled:  </span><span class="org-done">DONE</span><span class="org-agenda-done"> </span><span class="org-link">Email from George Oates: Re: Email setup</span>    <span class="org-agenda-done"><span class="org-tag">:tbd:</span></span>
<span class="org-agenda-done">  projects:   Scheduled:  </span><span class="org-done">DONE</span><span class="org-agenda-done"> Implement email system for support [5/5]</span>
<span class="org-agenda-done">  projects:   Deadline:   </span><span class="org-done">DONE</span><span class="org-agenda-done"> Implement email system for support [5/5]</span>
<span class="org-agenda-date">Thursday   23 June 2011</span>
<span class="org-agenda-date">Friday     24 June 2011</span>
<span class="org-agenda-done">  projects:   Scheduled:  </span><span class="org-done">DONE</span><span class="org-agenda-done"> </span><span class="org-link">Email from Anand Chitipothu: Account validation glitches</span> <span class="org-agenda-done"><span class="org-tag">:tbd:</span></span>
<span class="org-agenda-done">  projects:   Scheduled:  </span><span class="org-done">DONE</span><span class="org-agenda-done"> Fix account validation problems </span>
<span class="org-agenda-done">  projects:   Scheduled:  </span><span class="org-done">DONE</span><span class="org-agenda-done"> Mail Vikas about Org-mode information he wanted</span>
<span class="org-agenda-done">  projects:   Deadline:   </span><span class="org-done">DONE</span><span class="org-agenda-done"> </span><span class="org-link">Email from George Oates: Support System - bugs/tweaks l</span><span class="org-agenda-done"> [5/5]</span> <span class="org-agenda-done"><span class="org-tag">:tbd:</span></span>
<span class="org-agenda-done">  projects:   Deadline:   </span><span class="org-done">DONE</span><span class="org-agenda-done"> </span><span class="org-link">Email from George Oates: Re: Email setup</span>    <span class="org-agenda-done"><span class="org-tag">:tbd:</span></span>
<span class="org-agenda-done">  projects:   Deadline:   </span><span class="org-done">DONE</span><span class="org-agenda-done"> Vitalise Reassign/close case in support system</span>
<span class="org-agenda-done">  projects:   Deadline:   </span><span class="org-done">DONE</span><span class="org-agenda-done"> Change =/task= to =tasks=</span>
<span class="org-agenda-date-weekend">Saturday   25 June 2011</span>
<span class="org-agenda-done">  projects:   Scheduled:  </span><span class="org-done">DONE</span><span class="org-agenda-done"> Integrate habits with capture</span>
<span class="org-agenda-date-weekend">Sunday     26 June 2011</span>
</pre>

Now, you just pick out things to do, do them and hit `C-c t` to mark
them as done so that they move off your agenda. 

Things I don't do
-----------------

1. I'm not a clocking junkie who has to know exactly how much time I
spend on tasks. I tried using
[arbtt](http://darcs.nomeata.de/arbtt/doc/users_guide/) to do that for
a while and it works very well but I don't really need or use it. 
2. Custom agenda views. The default view looks fine to me. I haven't
really felt the need to change it yet.


Things to do
------------
Other things which I want to include in my system are 

1. Capturing directly from my browser using `org-protocol` and maybe a
custome extension. Bookmark and schedule for reading rather than dive
into that hacker news article in the middle of work.
2. Some tiny xmonad customisations to capture while outside Emacs. A
popup Emacs capture buffer using emacsclient to take something down
and close it.
3. Integrate skype chats with Emacs using bitlbee and erc so that
I can "capture" skype conversations for reference or as tasks.
4. Try some effort estimation to practice my estimation skills. 

Summary
-------

So, the bottom line is that you simply capture stuff from various
parts of your life into org, refile them appropriately once a day,
schedule them and get them done. No more missing tasks or items. 

I welcome your comments and suggestions. My Config files are on github
and you're more than welcome to cannibalise them. 

If there's sufficient interest in a formal Emacs lisp tutorial, I can
take a conventional class sometime later this year in Bangalore. Let
me know. 

At PyCon 2011
-------------
I've submitted a talk for PyCon India 2011 which will discuss my
Python development settings for Emacs. It will discuss mostly
programming tools but I do plan to spend a little time discussing
tracking stuff with org etc. so if you're interested, you should [sign
up](http://pyconindia2011.doattend.com/).






