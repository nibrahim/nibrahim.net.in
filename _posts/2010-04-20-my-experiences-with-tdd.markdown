--- 
layout: post
title: My experiences with TDD
wordpress_id: 272
wordpress_url: http://nibrahim.net.in/journal/?p=272
---
I recently wrapped up a project at my workplace. It was an RPC mechanism for a usb gadget running an embedded UNIX. The framework exposed the device as a software object on which one could make regular function calls and stuff. They would be translated into an application protocol, sent to the device, executed there and the results returned. 

The framework was useful but not particularly novel. It was done mostly in Python with a few C extensions here and there to work around the limitations of the <a href="http://bugs.python.org/issue3132">struct</a> module. The interesting part was that it was the first real world non-trivial application that I did entirely using TDD (test driven development). I prepared myself with a copy of <a href="http://www.amazon.com/dp/0321146530/?tag=hashemian-20">TDD by example by Kent Beck</a> which was an excellent book. This blog post puts down in writing some of my feelings and experiences with the whole business. 

<ol>
<li> It's time consuming </li>
The red/green/refactor cycle is addictive and after a while becomes second nature but it is time consuming. I easily took around 2 or 3 times the time to develop the app that I would have taken if I were just coding and doing manual testing. Of course, there is effort to write tests and it paid off substantially. Massive changes could be easily verified, git bisect became useful and my confidence in the code and willingness to refactor it increased.
<li> Design improvements and cleaner abstractions</li>
The need to build the app piece by testable piece resulted in an architecture that was very loosely coupled and relied on some simple API conventions. Also, while the actual device and host communicated via. USB, it would be tough to test using that. To manage this, I abstracted the communication channel from the actual application and made a dummy 'short circuit' channel to do testing of the two endpoints. I don't think I'd have done this if I didn't have to test it so rigorously. 
<li> Smaller/cleaner code </li>
Functions in the application were small and clean. The interfaces were clearly specified and didn't keep any local state (since I needed to drive them from the tests). I didn't spend <em>that</em> much time working on the cleanliness of the tests themselves so that part accumulated some cruft but the app itself was quite clean and is still very readable. 
<li> Shared culture </li>
It's important to have everyone in the team work the same way. I went off for 2 weeks during the project and got back only to find most of my abstractions broken and some 30 tests out of the 42 odd ones in the suite failing. This was depressing and was a huge mental barrier against further development. It totally shattered the confidence I had so carefully built up.
<li> Nice APIs </li>
Even before you write the app, you will write the tests and there you'll be forced to write a nice API so that you can make the test readable. This forces you to design your APIs better than you would have otherwise. The main reason is that you have a use case for your APIs before you've designed them
<li> Brain in a jar testing is bad and almost useless </li>
If you mock out all dependencies a module or class has and test it in isolation, chances are that you'll get flimsy tests that don't really do anything useful. There should be a significant piece that you are testing. Too small might mean too trivial.
<li> Scope of tests </li>
If you're testing a method, it's best to by default test inputs, outputs and side effects. Anything less and your coverage is likely to suffer. 
<li> Testing is under emphasised</li>
Testing, build harnesses, infrastructure etc. are as important as if not more so than the actual application being built. People need to get this into their heads. 
<li> Don't stop at unit tests</li>
4 modules with around 60 tests were done. Every line was tested. The whole thing was put together and it crashed and burnt. Integration tests that verify end to end behaviour are a must and need to be done as part of the regular test suite. It might be hard to develop them before the app is done though. They don't form a part of the whole TDD cycle but they're necessary. 
<li> It's fun </li>
This alone should be a sufficient reason why I'm going to use this approach for my next project as well. 
</ol>

The app has been deployed and is in production. Let's see how things go. 
