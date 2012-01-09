---
layout: post
title: "Moving topic branches in git"
---

I hit an interesting situation recent which I'll try to recreate here in the hopes that others might benefit.

Imagine that we cut a branch called `tp1` from our `master` and did some work on that branch. At the same time, some other work was done and pushed onto `master`. Our current DAG looks ike this.

![Initial scenario] (/images/git/rebase-0.png) 

Now, while on the topic branch, we do some work and commit them to create this. I've used `tb2` in the commit messages so that you can identify them easily.

![Two commits on topic branch 1] (/images/git/rebase-1.png) 

Now, we realise that these two commits should be on a different topic branch which should be based off `master`. 
We look at the logs (while on the topic branch) and see this

    (ol)[1] noufal@sanitarium% git log --pretty=oneline 
    32abaeefed66ee5165b7e7edddb5604ecdb2fa64 tb2 5
    4541e86113cb4ceaf654ed74abe85d477950ed2f tb2 4
    84d1f43da8b6d63592627aca6ecd4c7e3df29600 tb1 3
    7956bc382d183674208f1b04c3e62d12806c517c tb1 2
    a6d74c2cb1e0b7ba456626898fd63a3c0d1fc863 tb1 1
    c60c4c22f2eb8d3c7465ad3b138a7d4a3aee3c5f master 3
    0c99224a32fcc61e123f94837d971456a2659bb0 master 2
    c3522d06410e2a37444ed5759f7a6f4c7b67ad17 master 1
	
We want `8a94e9cf00aa2df580d7beed8efb52557bdfce16` to be based off the current tip of the `master` branch

    (ol)[1] noufal@sanitarium% git log --pretty=oneline master
    755b7823acfa30f60ade5ad6c5c575116c38fe92 master 5
    95da533bb52e60497ba5312dcd0540c1bd32db96 master 4
    c60c4c22f2eb8d3c7465ad3b138a7d4a3aee3c5f master 3
    0c99224a32fcc61e123f94837d971456a2659bb0 master 2
    c3522d06410e2a37444ed5759f7a6f4c7b67ad17 master 1
	
In other words, off commit `755b7823acfa30f60ade5ad6c5c575116c38fe92`. How do we do this?

We first create a the new topic branch (lets call it `tp2`). Our DAG now looks like this

![New topic branch created] (/images/git/rebase-2.png) 

And then we rebase this *onto* the `master` branch using the `84d1f43da8b6d63592627aca6ecd4c7e3df29600` as the branching point like so `git rebase --onto master 84d1f43da8b6d63592627aca6ecd4c7e3df29600 tp2`. 

    (ol)[1] noufal@sanitarium% git rebase --onto master 84d1f43da8b6d63592627aca6ecd4c7e3df29600 tp2
    First, rewinding head to replay your work on top of it...
    Applying: tb2 4
    Applying: tb2 5
	
Once this is done, we get

![Rebasing is done] (/images/git/rebase-3.png) 	

And then we switch back to the `tp1` branch and reset it to discard the two extra commits that are now tracked by `tp2`. 

    noufal@sanitarium% git co tp1
    Switched to branch 'tp1'
    noufal@sanitarium% git reset --hard 84d1f43da8b6d63592627aca6ecd4c7e3df29600
    HEAD is now at 84d1f43 tb1 3

We get this

![Final product] (/images/git/rebase-4.png) 	



















	
