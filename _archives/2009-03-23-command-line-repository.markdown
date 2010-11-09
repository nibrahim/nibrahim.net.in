--- 
layout: post
title: Command line repository
wordpress_id: 235
wordpress_url: http://nibrahim.net.in/journal/?p=235
---
<a href="http://www.commandlinefu.com/">Commandlinefu.com</a> is an awesome site which serves as a public repository for (mostly UNIX) command line spells. I learnt quite a few things from there and I just found it today. Here are some useful examples. 
<ol>
<li>Redirect a file from a currently running process
<pre>yes 'Y'|gdb -ex 'p close(1)' -ex 'p creat("/tmp/output.txt",0600)' -ex 'q' -p pid</pre>
</li>
<li>Display a graph of apache2's dependencies. 
<pre>apt-cache dotty apache2 | dot -T png | display</pre>
</li>
<li>Capture a video of the desktop session
<pre>
ffmpeg -f x11grab -s wxga -r 25 -i :0.0 -sameq /tmp/out.mpg</pre>
</li>
</ol>

