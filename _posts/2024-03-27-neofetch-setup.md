---
layout: post
title:  "Neofetch set up"
date:   2024-03-27
featured_image: Neofetch.jpg
tags: [Neofetch, Linux. dotfiles, configuration]
---

Neofetch is a handy (if a little cheesy) utility that gives some useful summary info about a *nix system from one command in the terminal. Many people, including myself like to have this pop up each time they launch a terminal instance. This behvaiour is handy if you have mutliple systems that you are accessing via SSH and want to be reminded of where you are in the world! 

*note if you are running a Zsh shell on mac, this wont work! 

On modern, powerful hardware, neofetch pops up in a fraction of a second with its handy little summary, however, on lower powered hardware (raspberry pi's etc) you may find yourself waiting several seconds while it churns away to find out the relavent information. 

There are many ways that we can get round this issue, but the one I have settled on is to run an instance of neofetch every few minutes to update a cached file that is cat'd on start up of a terminal instance. This results in an instant display on any system I have tried so far. 

First Job - script to create cache of neofetch output.

{% highlight bash %}
#!/bin/bash 
/usr/bin/neofetch >| /home/alecjtaylor/.nf
{% endhighlight %}

or if you are on a mac

{% highlight bash %}
#!/bin/bash
/usr/local/bin/neofetch >| /Users/alecjtaylor/.nf
{% endhighlight %}

Second Job - set up a cron entry to run every 'X' minutes to run the above script
It also runs on start up so you don't have to wait when you first boot up your machine. 

{% highlight bash %}
* * * * * 'home/alecjtaylor/.nf.sh'
@reboot 'home/alecjtaylor/.nf.sh'
{% endhighlight %}

Final job - edit your .bashrc file

In linux and assuming you are running a bash shell, you'll need to add a command at the end of the .bashrc file to display the content of the cached neofetch output each time a new terminal session is started. 
If you are on a mac, I'd suggest adding this to a .bash_profile file if it exists, or you can create your own .bashrc if you like.

{% highlight bash %}
cat .nf 2> /dev/null
{% endhighlight %}

the '2> /dev/null' bit is a handy bash command that redirects any errors to /dev/null, which in practice hides them. this keeps things tidy just in case there is an issue with something in the chain. 



<!--more-->

### Regular Image


{% highlight bash %}
'#!/bin/bash 
/usr/bin/neofetch >| /home/alecjtaylor/.nf'
{% endhighlight %}

or if you are on a mac

{% highlight bash %}
#!/bin/bash
/usr/local/bin/neofetch >| /Users/alecjtaylor/.nf
{% endhighlight %}

{% include image_caption.html imageurl="blog/images/posts/mac_neofetch.jpg" title="Mac Neofetch" caption="Sample of mac neofetch output" %}


<!--more-->
