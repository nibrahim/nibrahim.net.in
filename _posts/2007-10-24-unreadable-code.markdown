--- 
layout: post
title: Unreadable code
wordpress_id: 119
wordpress_url: http://nibrahim.net.in/journal/?p=119
---
Not that we needed any confirmation about perl's terse syntax but, apparently even <em>grep</em> has a hard time with it.
<pre>
$ grep SNPS * | tail -2
Binary file ts.verify matches
$ head -1 ts.verify
#!/usr/bin/perl
$ 
</pre>
