--- 
layout: post
title: Bootable Jaunty USB stick
wordpress_id: 259
wordpress_url: http://nibrahim.net.in/journal/?p=259
---
While using ubuntu's usb-creator to create a live CD for Ubuntu 9.04 (Jaunty I think), the image is incomplete for some reason and the thing fails to boot with this message
<pre>
Could not find kernel image: vesamenu.c32
</pre>

Copying over the syslinux directory manually to the usb stick from the iso image seems to fix the problem. The thing booted. Now let's see if we get anywhere.

