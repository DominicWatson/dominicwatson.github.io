--- 
name: my-cinnamon-desktop-setup
layout: post
title: My Linux Desktop (cinnamon) setup
time: 2020-09-20 15:22:00 +00:00
---

Its September 2020 and I've very recently installed Linux Mint 20, Cinnamon Edition and I'm stoked enough with it to want to share the experience (with the hope to inspire someone else to make the switch). 

I keep coming back to this distro as, for me, it has the simplest, fuss-free configuration to productivity that matches my workflow. Headlines, for me:

1. UI that gets out of the way
2. Keyboard shortcuts for everything related to the desktop
3. Attractive
4. Multi-monitor support
5. Stable

![My Cinnamon Desktop - Background image by Simon Stalenhag](/public/images/desktop2000.png)

# Desktop appearance and general theme

## Desktop

This is the first, and most simple thing. I like a single panel at the top of the desktop with minimal content:

1. Cinnamon menu launcher (rarely used, but useful in case)
2. Date/time with link to calendar
3. System tray

That just leaves choosing a desktop background. I recently discovered [Simon Stalenhag](https://simonstalenhag.se/) whose awesome work makes for a great atmospheric desktop I think.

## Theming

In the past, I've spent a fair amount of time tweaking the themes and getting just the right icon set. Today, I have a pretty straight forward setup with minimal external installs:

1. Icon theme: [Papirus](https://github.com/PapirusDevelopmentTeam/papirus-icon-theme)
2. Everything else (almost) - Mint Y (out of box theme)

![My Cinnamon theme choices](/public/images/themeconfig.png)

### Custom panel styling

I really like the transparent panel that the default theme for [Elementary OS](https://elementary.io/) use and it didn't take long to reproduce in Cinnamon. I found a few transparent panel themes through the theme installer but none of them quite matched the general theme of the desktop for me. So, a tiny little DIY was required to get the desired result:

```bash
# copy the core Mint-Y theme into my own .themes directory
mkdir -p ~/.themes
sudo cp -r /usr/share/themes/Mint-Y ~/.themes/Dom-Mint-Y
chown -R dom:dom ~/.themes

# update the theme index file to indicate modification
echo "[Desktop Entry]
Type=X-GNOME-Metatheme
Name=Transparent Mint-Y
Comment=A flat theme with transparent elements
Encoding=UTF-8

[X-GNOME-Metatheme]
GtkTheme=Transparent Mint-Y
MetacityTheme=Transparent Mint-Y
IconTheme=Transparent Mint-Y
CursorTheme=DMZ-Black
ButtonLayout=menu:minimize,maximize,close" > ~/.themes/Dom-Mint-Y/index.theme

# make the panel transparent
echo "
.panel-top, .panel-bottom, .panel-left, .panel-right {
  color: #ffffff;
  border: none;
  background-color: rgba(0, 0, 0, 0);
  font-size: 1em;
  padding: 0px; 
}" >> /.themes/Dom-Mint-Y/cinnamon/cinnamon.css
```

## No dock?

There are lots of really nice "Docks" for the Linux desktop and I _do_ enjoy their appearance. However, I find that I just don't use them and they do nothing but get in the way. So - no dock.

## Launcher

For me, the launcher has to be [Kupfer](https://kupferlauncher.github.io/):

![Kupfer launcher](/public/images/kupfer.png)

Hitting `ctrl-space` at any time brings up the launcher. Start typing and find the thing you want. This includes all your applications, SSH connections from `~/.ssh/config`, remote desktop configurations in Remmina and a host of other useful things.

## Desktops

Cinnamon has a simple multi-desktop scheme. Four horizontal desktops. I do nothing to tweak this other than give a label to each one. I _do_ love this feature and use it all the time though. I also have the `alt-tab` switcher _only_ cycle through apps in the current desktop.

![Desktop switcher](/public/images/desktopswitching
.png)

# Keyboard shortcuts

After setting up my keyboard with custom second layer for VIM like navigation (see [Who needs capslock anyway?](/2014/11/who-needs-capslock-anyway-part-2.html)), the next task is to setup core desktop navigation keyboard shortcuts. The things that _I_ care about:

1. Min/Maximize a window
2. Move a window left->right, right->left _monitor_
3. Move a window left->right, right->left desktop (I have a simple horizontal four desktop setup)
4. Switch applications (alt-tab)
5. Snap window to left/right side of current desktop
6. Take screenshots (using shutter), whole and dragged area
7. Auto type passwords with KeepassXC
8. Open KeepassXC for finding passwords
9. Switch between desktops

The Cinnamon team have done a great job with the keyboard shortcut setup and all the above is super easy out-of-the box.

# Software

Beyond the basics (and among other non-consequential things), I install:

* [Shutter](https://shutter-project.org/) (for screenshots)
* [KeePassXC](https://keepassxc.org/) (offline password manager)
* [Glimpse](https://glimpse-editor.github.io/) (image editing)
* [Sublime Text](https://www.sublimetext.com/) (coding)
* [DBeaver](https://dbeaver.io/) + [mycli]() (database management)
* [Shortwave](https://gitlab.gnome.org/World/Shortwave) - online radio
* [Insomnia](https://insomnia.rest/) - RESTful client and more
* [Spotify](https://www.spotify.com) - I have a family account, and it works perfectly in Cinnamon

# Summary

There's loads more that could be written to go into detail about why I love using Linux, and the Cinnamon desktop specifically. A lot has to do with how things just work and also get out of your way to make you really productive. 

In 2020, I don't have a single problem that prevents me from doing what I have to do. In addition, there's plenty that makes things easier than other environments. I find having a desktop environment singing how you like it, be it OSX, Windows or some Linux DE, really helps my state of mind when working and I've never been happier with my setup.

If you're curious about trying Linux, my recommendation is [Linux Mint Cinnamon](https://linuxmint.com/).