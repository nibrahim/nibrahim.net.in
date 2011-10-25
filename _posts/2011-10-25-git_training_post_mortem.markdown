---
layout: post
title: "Git training post mortem"
---

These are some thoughts on the recent git training which I conducted
here in Bangalore.

I've done several corporate trainings before and spoken at local
events but I've never conducted something like this before.

## Organisation 

I'll first talk about the organisation. The dates were bad. I didn't consider Diwali. I had other plans for the last week of October and early November and after that is [DroidCon](http://droidcon.in) so I just dropped it on October 22rd and 23rd. A lot of people went back home for the holidays and so the attendance dropped. Quite a few people told me explicitly that this was a problem.

The venue was overpriced for something like this. I don't want to give out actual numbers but this place is designed for more "corporate" style events and has "corporate" prices which someone like me can't afford if I want to keep doing this on a regular basis. However, no one really complained about my prices. I explicitly asked about this in the feedback forms and no one really complained about the prices. I asked some third parties and they generally said that the price was low. A typical open source style conference here charges between 1000 and 2000 INR. I charged 5500 for the training and with the low number of people and personal attention, I guess it was worth it for the participants. 

I created the slides using [landslide](https://github.com/adamzap/landslide) and added a macro to display command output on the slides. I kept paper notes for the complex parts like discussions of the merge algorithms and some more involved examples. This was useful. I tried to use the whiteboard which the venue manager provided but it was too unstable and I couldn't really use it.


## Content

The morning of the first day was mostly introductory stuff. While I generally feel that git is easier understood from the inside out, you've got to start somewhere. Create a few repositories, commit files, view logs, diffs etc. The usual version control stuff. After that, I spent some time talking about the index which is something that git users have to be aware of. 

### Day 1 morning

After this we dived into git objects. Blobs, trees and commits. I went over these trying to drive home the idea that git is internally organised like a file system. We used the `cat-file` command to dig into the various objects and then went into the `.git` directory to actually see what's happening. We tried running some commands to see how the repository was changing so that people actually understand what's going on when they do stuff like `git add` etc. I used a few scripts using [grit](http://grit.rubyforge.org/) which outputs files that can be understood by [dot](http://graphviz.org/). These draw the commits over time and the various heads which help people understand how commits are made. I also had some scripts that actually draw the DAG as it grows over time and pictures like [this](http://twitpic.com/72lpp7) help people understand how this takes place. We went on with this for quite some time and the morning session of the first day got over. 

### Day 1 afternoon

Then we started the business of branching and merging. I tried to emphasise the lightweigt nature of git's branches and the fact that they're private unless published. I tried to spend an equal amount of time on the actual implementation of graphs as well as their uses. We discussed merging, fast forward and true. We supplemented all this with examples and exercises but I'll come to that later. We mentioned rebasing and then stopped. 

### Day 2 morning

We spent about 45 minutes on a recap of what happened the previous day. After that, we dived into the business of rebasing and after that spent time talking about merging. We sidetracked a little talking about the diffing and merging algorithms and I tried to explain why merging works better in git than it does in subversion. We then talked a little about how git does merging (including the internal implementation). After this, we started with collaboration. We just mentioned remotes, bare repositories, and tracking branches and then stopped for lunch. 

### Day 2 afternoon

Here, we paired up and spent time actually pushing and pulling changes to a remote. I set up a bunch of bare repos on my machine and created an ad hoc wireless lan which everyone used to collaborate. I tried to cover details of different workflow styles, refspecs, an important gotcha of pushing discussed [here](http://longair.net/blog/2011/02/27/an-asymmetry-between-git-pull-and-git-push/) and when everyone was satisfied, I wrapped up by discussing a bunch of useful leftover commands like `bisect`, `archive`, `cherry-pick` etc.


## Things to be improved

I didn't like the way I created the exercises. I think they needed to be highly stuctured (no ad hoc stuff *at all*). Also, I need to seriously rethink them. I didn't have a "list of commands" section. I didn't think it was a good idea and introduced commands as we needed them but I wonder now whether that was the right way to do it. I didn't give any homework. One of the participants suggested that I should have. I didn't cover any of the bridges (git-svn etc.). 

I should have advertised a little more and given more gap between the announcement and the day of the event. I should have booked a cheaper venue. I should have chosen the dates more carefully. 


All things considered, I think it went well. If anyone here is interested in a repeat, I'd be happy to do one in December or so. Please holler. :)


