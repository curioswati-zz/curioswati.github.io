---
title:  "Getting started with i3: The Tiling Window Manager"
date:   2017-06-24 16:05:00
tags: ['index', 'tools', 'tutorial', 'dev-hacks', 'ubuntu', 'i3']
categories:  tools i3
permalink: /:categories/:title
---
Being a keyboard girl, I always look for things that takes the touchpad out of the picture and now I have found one more.

I recently was introduced to [i3](http://i3wm.org/) from one of my colleagues at [DoSelect](https://doselect.com).
Before that I have been long wondering about setting up arch and go all non-GUI but it was just a someday thing that seemed not to be happening anyday.
But this time seeing him using, I was a bit encouraged to try it and over a weekend I finally did.
Setting it up and getting things into normal flow took some efforts but after some Googling, I was able to get into my normal flow with **i3**.

Summarizing the process here, for my own future reference and others who are dwelling in the someday thing ;-).

<p class='note'>Note: I use ubuntu 16.04. For installation relatd stuff, do check the <a href="http://i3wm.org/">official website</a>.</p>

<br>

### 1. Get it!
Just a command as usual:

    sudo apt-get install i3 i3-wm

<br>
### 2. Start it!
Now logout and while on the login screen, you will find an option for changing the desktop environment.
It should be there on the top-right corner of the login form. Just select **i3** from there and login.

You will be welcomed by a blank screen. Yes, for the first time, it runs a configuration manager,
so it will ask you some questions. 

**First** - To create a default configuration file, **Accept** it.  
**Second** - Which key you want to use as the **Meta** key. Now it is a special key.
Every control command will be bound with this key later on, so select one that comes in more handy.
Well, mine is `Mod1` -> `Alt`.

<br>
### 3. Some tools in the box!
Extra packages for some handful functions that you are going to need:

- **i3status** - The status bar for everything that you want to keep monitoring.
- **i3lock** - Screen locker.
- **dmenu** - The application menu.
- **feh** - For desktop wallpaper (Yeah! **i3** does not handle that for you)
- **dunst** - Well! notifications are important.
- **scrot** - Sometimes you need screenshots.
- **NetworkManager-gnome** - For managing network connections from the status bar.

<br>
### 4. Configure it! (Of course)
Now comes the most important part, the configuration file.
On the first interaction, the file was created in `/etc`, you need to create one in your `home` directory to alter it.
you can either create `~/.i3/config` or `~/.config/i3/config` as per your choice. Just copy the file from `/etc/i3/config` in the location.
Also make yourself the owner of the file.

For the aliens, here are the commands:

    mkdir -p ~/.config/i3
    sudo cp /etc/i3/config ~/.config/i3/config
    sudo chown $USER:$USER ~/.config/i3/config

This file gives you complete control over your **i3** environment.

**i3** comes with a default configuration, I have tweaked the file for my needs and the changes look like following:

    bindsym Ctrl+Mod1+t exec konsole
    bindsym Mod1+F4 kill
    bindsym Mod4+b exec google-chrome
    bindsym Mod4+e exec dolphin

These are some of the keyboard shortcuts. For every other shortcut, you just need to `bindsym` the key to `exec` the application or command.

<br>
Here are few function key bindings, that took me time to find:

##### **Volume control with function keys:**

    bindsym XF86AudioRaiseVolume exec --no-startup-id pactl set-sink-volume 1 +2%
    bindsym XF86AudioLowerVolume exec --no-startup-id pactl set-sink-volume 1 -2%
    bindsym XF86AudioMute exec --no-startup-id pactl set-sink-mute 1 toggle

<br>
##### **Capture screenshots with `<prt sc>`:**

    bindsym Print exec "scrot -e 'mv $f ~/Pictures'"

<p class='note'>Note: It will capture entire screen, look in the <code class='note'>man</code> page for <code class='note'>scrot</code> for customizations.</p>

<br>
##### **Lock screen with `<Ctrl+Alt+l>`:**

    bindsym Ctrl+Mod1+l exec --no-startup-id i3lock -i Pictures/wallpaper.jpeg

<br>
##### **Startup Applications:**

- auto lock after 5 minutes of inactivity

      exec --no-startup-id xautolock -time 5 -locker "i3lock -i Pictures/wallpaper.png"

- remember desktop wallpaper

      exec --no-startup-id ~/.fehbg

    <p class='note'>Note: For setting the wallpaper first time, use <code class='note'>feh --bg-scale /path/to/wallpaper</code>.
Now that <code class='note'>scale</code> is variable thing, you can choose from other options to decide how your wallpaper should render on screen.
See <code class='note'>man feh</code> for details.</p>

- network manager

      exec --no-startup-id nm-applet

- bluetooth devices

      exec --no-startup-id blueman-applet

<br>
#### Status Bar
okay, so we setup some applications. Now let's configure the status bar, that is the only thing you can see on your screen when it's all black.

In the **i3** config file, you will find a block called "bar". It by default has one command that decides which application to use for status bar.
By default it uses, **i3status**, you can change it to your preference (that you will need to hunt, I am okay with the default till now).

You can specify some more commands to tweak the display of the status bar.

- position

      position top

There are many more (I'm still to explore).

Now what can you show in the status bar.

Create a file `~/.config/i3status/config` for the status bar configuration.
See mine [here](https://gist.github.com/curioswati/a482f56cbdd3929e43db6bdc84dfb4b7). You can copy it to the above location,
and reload **i3** (Mod1+Shift+r).

<br>
### 5. Go!
Yay! We have installed and configured **i3**.
Now let's learn some keys to use it.

Press `Meta+Enter` if you want to open a terminal, later on set a keybinding for that too.
You can use `Meta+d` for opening any application, it will run `dmenu` that will show application names at the top of your screen.
<br>
#### **Navigation**

So, you can open applications by keybindings. But how to navigate among them?
Well, there are many hotkeys for different things. They are set by default in the config file, you can tweak them there.
I'll tell you about the default ones here and only a few of them,
rest you can find out [here](http://i3wm.org/docs/userguide.html#_opening_terminals_and_moving_around).

- To switch between vertically split windows, you can use `Meta+j` and `Meta+;`.
- To switch between horizontally split windows, use `Meta+k` or `Meta+l`.  
**(Recall Vim)**.

Windows are put inside a container in **i3**. There are two ways, 
in which the containers can be arranged- **horinzontally** and **vertically**.

When you want your windows to be split horizontally on opening, first press `Meta+h` and then open next application.
You will see them one above other and now use the above mentioned key bindings to navigate.
When you want your windows to be split vertically on opening, first press `Meta+v` and then open next application.
You will see them arranged side by side and now use the above mentioned key bindings to navigate.

#### **Window Management**
Here are a few shortcuts for managing windows.
- To close a window, `Meta+Shift+q` or `Ctrl+w`.
- To toggle fullscreen mode for a window, `Meta+f`.

**i3** has a concept of workspaces. It has 10 workspaces, each workspace is a virtual screen that can again be divided into containers.
The windows can be moved among workspaces, use `Meta+Shift+<num>`
where `num` is the number label of workspace in which you want to send the window. 

#### **Workspaces**

As I said above, you have 10 workspaces, you can navigate among them with `Ctrl+<num>`,
where `num` is the number label of workspace to which you want to navigate to.

<br>
### And...
So this is it! Not actually, because there is a lot that can be customized and can be learned.
I am also starting with things, so put an introduction forward. The [docs][1] are a great place to understand things and play around with them.
I will also update the post with whatever experiments I will be doing and the learnings from them.

<br>
### References
[1]: http://i3wm.org/docs/userguide.html
- [i3 docs](http://i3wm.org/docs/userguide.html), they are awesome!
- [hands-on-with-the-i3-window-manager-installing-configuring-and-using](http://www.zdnet.com/article/hands-on-with-the-i3-window-manager-installing-configuring-and-using/)
- [Install and Use i3 Window Manager on Ubuntu](https://www.maketecheasier.com/install-use-i3-window-manager-ubuntu/)
