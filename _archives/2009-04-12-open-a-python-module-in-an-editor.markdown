--- 
layout: post
title: Open a python module in an editor
wordpress_id: 239
wordpress_url: http://nibrahim.net.in/journal/?p=239
---
A simple trick which <a href="http://anandology.com/">Anand</a> showed me. A shell function that finds that file component of the python  module given to it and then opens it in Emacs. 

<pre>
epy() {
    cmd="import $1 as a ; print a.__file__.endswith('.pyc') and a.__file__[:-1] or a.__file__"
    file=$(/usr/bin/env python -c $cmd)
    echo $file
    emacsclient --no-wait $file
    }
</pre>

So that you can type
<pre> 
epy urllib
</pre>
to see the urllib module rather than figure out it's location manually (/usr/lib/python2.5/urllib.py) and then spawn off your editor with it. 

<em>Update:</em> <a href="http://github.com/anandology/hacks/blob/2710ffb78a29a230cef453118f6b3097a59ae8b0/pyvi">He's done it</a> rather differently though. :)
