---
layout: post
title: Who needs caps lock anyway
time: 2014-09-07 08:56:00 +00:00
---

I've been obsessing over my keyboard layout recently, stumbling upon [typing.io](http://typing.io) and acquiring the awesome [Das Keyboard 4 Ultimate](http://www.daskeyboard.com/daskeyboard-4-ultimate/). One thing I've noticed that kills my typing rhythm is using the arrow keys to navigate code; in particular, having to move my right hand from its "home" typing position and back again once I've finished (something that [Vim](http://en.wikipedia.org/wiki/Vim_%28text_editor%29) artists don't suffer from).

In Windows, I had found an excellent utitlity called [TouchCursor](http://touchcursor.sourceforge.net/), it allows you to use the space bar as a modifier key and maps `spacebar+i`, `spacebar+k`, `spacebar+j` and `spacebar+l` to `up`, `down`, `left` and `right` (plus a whole bunch more). I couldn't find an equivalent utility for Linux after much searching, so set about hacking out a workable solution.

<!--more-->

## Ditching caps lock

The caps lock key is in a prime location and something that I very rarely use. It was a no-brainer to substitute it for something else after reading some blog post advocating this approach. This was thankfully straightforward in the Cinnamon Desktop Environment: **Settings** -> **Keyboard** -> **Keyboard** **layouts** -> **Options** -> **Caps lock key behaviour** gives you a list of possible substitutions for Caps lock, I opted for **Make caps lock an additional hyper**.

## Mapping keys

Once I had a new 'hyper' modifier, I then needed to map key combinations to individual keys. This was sadly less straight forward than I had hoped. There seems to be no native method to do this, using the `xmodmap` utility only allows single keys to be mapped to other single keys as far as I could make out.

My solution involved installing a utility called [AutoKey](https://code.google.com/p/autokey/). This utility allows you to define phrases that can be triggered by an arbitrary key combination. A phrase could be some lorem ipsum text, or, as wanted for our example, one or more special keys (such as modifiers and arrow keys). To map `CapsLock+J` to the left arrow key for example, I created a phrase with the content `<left>` and gave it a hotkey combination of `<hyper>+j`.

I now have all the arrow keys setup in this way, as well as `home`, `end` and `delete`. I also have `CapsLock+H` mapped to `<ctrl>+<left>` and `CapLock+;` mapped to `<ctrl>+<right>`. I will add more combinations once my fingers have fully learned this set, but this already feels like a really great improvement. 

What other uses for the CapsLock key are you using / can you think of?

