--- 
layout: post
title: Coding for testing
wordpress_id: 99
wordpress_url: http://nibrahim.net.in/journal/?p=99
---
While refactoring one of my recent (python) programs at my workplace, I came across a need to run a large number of tests on some of the methods that my library provides.

The application is fairly simple. It'a wrapper for an internal parallel build system which customises it for our team. It provides some simple ways of customising the build sesion per site (eg. license servers in the India network are different form those in the German network etc.) and provides lots of convenient shortcuts for different types of builds. It does some checks to see if the requested build is supported or not, logs the amount of time each stage of the build takes, runs some smoke tests once the build is over and saves debug information which can be used for post mortem analysis of mistakes in the build rules.

It started off as a 50 line script which simply parsed some arguments and called the underlying build tool with them. This grew and now it's considerably larger.  The whole thing was a single file which has two or three classes and lots of separate functions. When the need came to put in the unittests using the (rather unique) <a href="http://docs.python.org/lib/module-doctest.html">doctest</a> module. I found lots of limitations in the way I wrote the functions. This is a note to self of sorts of little things to remember while coding. I'd appreciate comments by people who have more experience with this unit test driven style of programming.
<ol>
	<li><em>Raise exceptions instead of exiting when termination conditions are met</em>. - In my program, I had to set an ARCHITECTURE variable based on where the build was taking place and some other things like that. If this could not be set, the build could not take place so I abort with an error message. However, while testing, I purposely wanted to make it get corrupt input from the system to see if it would handle the situation properly. It did and it quit the program (and my test suite). Definitely not the way to go. It would have been better if it just raised a custom exception rather than quit.</li>
	<li><em>Make functions receive all parameters from the outside</em>- The harness looks for some information dumped by the version control system to figure out which version is going to be built. In a test scenario however, the location where the functions are called will not be where the build is taking place. eg. In this snippet
        <pre>
def isSupported(mactype, mode):
    version = open(".version").read().strip()
    # Check if version is supported on mactype for mode
    # return True or False 
</pre>
I need the file ".version" to be there when I test this function. This can't always be done. It would be much more convenient to read the version from this file outside the function and pass it as a parameter to this one. That way, I can test it for a large number or versions quite easily. 
</li>
<li><em>Keep testing in mind (modularise)</em> - I wrote all my code with some considerations (flexibility, ease of making changes for new versions, platforms etc., error handling etc.). Testing the code at a functional level was not one of them. If I had considered that, questions like, "when I test this, can I pass this? Oops no. Needs some changing." come up. Something like
<pre>
def runMake():
    for i in targets:
        try:
            os.system("make %s"%i)
        #Exception handling
</pre>
is pretty much untestable. It's a black box which does some magic when you call it and returns.  On the other hand, if this were more modular (a function to generate the targets, a function to generate the make command line, a function to actually run a command line watching for errors etc.), you could test each one of them. 
</li>
<li><em>No globals (except when absolutely necessary)</em> - Python allows dictionaries of globals to be passed to functions so you can simulate your actual program environment but as a general rule, it's quite hard to manage these things. I think it's useful to keep magic numbers, constants and data/action tables in a separate file which is loaded by the the main program and functions get parameters from this file. That way, your test scripts can do the same thing.
</li>
</ol>
I'll probably update this post as I move along. 
