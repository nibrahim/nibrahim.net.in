--- 
layout: post
title: Keyboard mappings on Linux
wordpress_id: 216
wordpress_url: http://nibrahim.net.in/journal/?p=216
---
My Thinkpad doesn't have a 'windows' key and I'm an Emacs user. Conventionally, I've always used the left ALT key as Meta (and ALT). All Emacs (and other application) functions rely on that button. The right ALT key, I map to a different modifier (like Hyper or Super) and use it for window management operations. This has been going on fine till my upgrade to Intrepid Ibex which messed things up. Usually, it's easy to fix this using jwz's xkeycaps but apparently, that's not working as well. It detects my right Alt as a numeric keypad enter. I somehow fixed it but then because of a package upgrade, messed up all my settings. I managed to do it again but want to document what I've done so that if I mess up again, I'll know what to do.

All the files you need to modify are in /usr/share/X11/xkb.
<ol>
<li> find all the files (XML and others) which contain the string hyper_win starting from this directory </li>
<li> To all the .lst files under rules and similar files, I added this
<pre>  altwin:ralt_super    Super is mapped to the right Alt key</pre> just after the entry for hyper_win</li>
<li> To the XML files, I added another option node like so just after the node for hyper_win
<pre>
     &lt;option&gt;
        &lt;configItem&gt;
          &lt;name&gt;altwin:ralt_super&lt;/name&gt;
          &lt;description&gt;Super is mapped to the right alt key.&lt;/description&gt;
        &lt;/configItem&gt;
      &lt;/option&gt;
</pre>
</li>
<li>
Now add the following line to the symbols.dir file in the top level directory
<pre>
--p----- -m------ altwin(ralt_super)
</pre>
</li>
<li>
Now add this to the symbols/altwin file
<pre>
partial modifier_keys
xkb_symbols "ralt_super" {
    key &lt;ralt&gt; {        [       Super_R                 ]       };
    modifier_map Mod4   { Super_R };
};
</pre>
just below the entry for hyper_win
</li>
<li>Copy over the rules/base.xml file to the /etc/X11/xkb directory.
</li>
<li>
Go to the System/Preferences/Keyboard menu from your panel and select the layouts tab. Click the 'Other options' button and under the Alt/Win key behaviour tree, you'll have the new option we added. Select that and your right alt will work as a Super key. You can verify this using xev. 
</li>
<li> Use the keyboard bindings application and change the keyboard shortcuts for window operations to use the right key. </li>
<li> Done </li>
</ol>
Disclaimer : I have no idea how xkb works or whether what I've done above is acceptable in any sense. It's working fine for me now and this is more of a 'note to self' rather than a tutorial of any kind. 
