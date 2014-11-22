---
layout: post
title: Who needs caps lock anyway (part 2)
time: 2014-11-22 15:45:00 +00:00
---

In my previous post, I described how I used a combination of modifying my CAPSLOCK behaviour + a utility called AutoKey to remap a bunch of keys for easier keyboard arrow navigation. This solution was not quite perfect due to the AutoKey layer in the middle which meant that the key presses didn't always register correctly in all situations. Today I had a little downtime from coding and I spent the time figuring out a better solution.

<!--more-->

## Create a custom keyboard layout

The solution was to create a custom keyboard layout that creates a secondary *group* of symbols that your key presses will evaluate to. The layout file looks like this, and for me, lived in `/usr/share/X11/xkb/symbols/gb_dom`:

{% highlight text %}
default  partial alphanumeric_keys
xkb_symbols "alpha_arrows" {
    include "gb"
    
    key <AC07>  { symbols[Group2]=[ Left,      Left,      Left,      Left      ] };
    key <AC08>  { symbols[Group2]=[ Down,      Down,      Down,      Down      ] };
    key <AC09>  { symbols[Group2]=[ Right,     Right,     Right,     Right     ] };
    key <AD08>  { symbols[Group2]=[ Up,        Up,        Up,        Up        ] };
    key <AD07>  { symbols[Group2]=[ Home,      Home,      Home,      Home      ] };
    key <AD09>  { symbols[Group2]=[ End,       End,       End,       End       ] };
    key <AD10>  { symbols[Group2]=[ Delete,    Delete,    Delete,    Delete    ] };
    key <AD06>  { symbols[Group2]=[ BackSpace, BackSpace, BackSpace, BackSpace ] };

};
{% endhighlight %}

What this does is to make the J, K, L, I, O, U, P and Y keys map to arrow keys, home, end, delete and backspace when the keyboard layout is switched into "group 2".

The next step enables us to pick the new layout in Ubuntu/Mint's keyboard settings. Simply add the following XML to the `/usr/share/X11/xkb/rules/evdev.xml` file:

{% highlight xml %}
<layout>
  <configItem>
    <name>gb_dom</name>
    
    <shortDescription>endom</shortDescription>
    <description>Dom's l33t layout</description>
    <languageList>
      <iso639Id>eng</iso639Id>
    </languageList>
  </configItem>
  <variantList />
</layout>
{% endhighlight %}

Done. We can now go into `preferences > keyboard` and pick the newly created **Dom's l33t layout** keyboard layout. For me, I wanted to make this the *only* layout.

## Mapping keys to toggle the layout group

Still in the keyboard setting UI, go to **Keyboard layouts -> Options** and find the **Switching to another layout** section. Here you will find a list of key combinations that will toggle either multiple keyboard layouts or groups within a single layout (which is what we have and want). 

I chose **Caps Lock (while pressed)**, this means I can use a combination like **Caps Lock + J** for a left arrow. I also checked **Right Alt**; this means I can press right alt to *toggle* the layout so that I just need to press the single keys for the arrows (and then more easily combine them with other modifiers for various operations).

This whole setup works really smoothly and without any of the problems that I encountered when using AutoKey (which is not a problem with AutoKey, just what I was trying to do). Programs just receive the mapped keys without any other noise.

## Further reading

I don't fully grok some of the concepts here. For example, I have `include "gb"` in my layout file and I *believe* that this includes the regular GB layout which is the one I normally use. There may well be a more succinct way to achieve this without creating an entirely new layout.

The following UNIX Stackexchange question and answer really nailed it for me and might be useful if you're wanting to play around with this stuff:

[http://unix.stackexchange.com/questions/153841/how-to-make-altgri-j-k-l-work-properly-as-cursor-keys](http://unix.stackexchange.com/questions/153841/how-to-make-altgri-j-k-l-work-properly-as-cursor-keys)

Finally, this guide on the Ubuntu community wiki got me started and has more in depth information on creating custom layouts:

[https://help.ubuntu.com/community/Custom keyboard layout definitions?action=show&redirect=Howto%3A+Custom+keyboard+layout+definitions](https://help.ubuntu.com/community/Custom keyboard layout definitions?action=show&redirect=Howto%3A+Custom+keyboard+layout+definitions)






