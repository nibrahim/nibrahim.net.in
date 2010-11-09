--- 
layout: post
title: Transparent editing of pickled files
wordpress_id: 252
wordpress_url: http://nibrahim.net.in/journal/?p=252
---
Emacs' jka-compr mode which is used to take care of transparently editing compressed files is quite generic. A few tiny helper scripts and one bit of lisp and it works for editing python pickled files too. 
I have two scripts
<pre>
#!/bin/bash
# pickle.sh
python -c "import pickle ; import sys ; print pickle.dumps(sys.stdin.read())"
</pre>
and
<pre>
#!/bin/bash
# unpickle.sh
python -c "import pickle ; import sys ; print pickle.load(sys.stdin)"
</pre>

Then I added this
<pre>
["\\.path\\'" "Pickling" "/home/noufal/local-lisp/ex-utils/pickle.sh" nil "Unpickling" "/home/noufal/local-lisp/ex-utils/unpickle.sh" nil nil nil ""]
</pre>
to my jka-compr-compression-info-list using the customize-group command and now when I open a .path file, it automatically shows me the unpickled version. When I edit and save it, it pickles in back. Very nice. 

My <a href="http://github.com/nibrahim/Xenon-Retroblast/tree/master">game</a> keeps a lot of the persistent data as files which are pickled so I do this a lot. 
