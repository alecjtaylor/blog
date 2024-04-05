---
layout: post
title: "Neofetch set up"
date: 2024-03-27
featured_image: mac_neofetch.jpg
tags: [neofetch, linux. dotfiles, configuration]
---
  
Neofetch is a handy, slightly cheesy utility that gives some useful summary info about any *nix system from one command in the terminal. Many people, including myself like to have this pop up each time they launch a terminal instance. This behaviour is handy if you have multiple systems that you are accessing via SSH and want to be reminded of where you are in the world!
  
On modern, powerful hardware, neofetch pops up in a fraction of a second with its handy little summary, however, on lower powered hardware (raspberry pi's etc) you may find yourself waiting several seconds while it churns away to find out the relevant information. Obviously, this is an unacceptable state of affairs and needs resolving.
  
There are many ways that we can get round this issue, but the one I have settled on is to run an instance of neofetch every few minutes to update a cached file that is cat'd on start up of a terminal instance. This results in an instant display on any system I have tried so far.
  
To follow the steps below, you'll need to have neofetch installed, it is in most repos and can be installed via homebrew on a mac.

## First Job
### Script to create cache of neofetch output.
Create a hidden shell script in your home directory. Enter the following commands intoa terminal.

{% highlight bash %}
touch .nf.sh
chmod +x .nf.sh
nano .nf.sh
{% endhighlight %}

now paste the following into your newly created shell script (swapping %USER with your username)

{% highlight bash %}
#!/bin/bash
/usr/bin/neofetch >| /home/$USER/.nf
{% endhighlight %}

or if you are on a mac
{% highlight bash %}
#!/bin/bash
/usr/local/bin/neofetch >| /Users/$USER/.nf
{% endhighlight %}

## Second Job
### Set up a cron entry to run every 'X' minutes to run the above script
You will need to run this command: 
{% highlight bash %} 'crontab -e' {% endhighlight %} 
So that you enter the edit mode of your cron config files so that we can add the text below. If given the option, select the nano option to edit the file, the world doesn't need more VIM evaglesits.

{% highlight bash %}
* * * * * '/home/$USER/.nf.sh'
@reboot '/home/$USER/.nf.sh'
{% endhighlight %}

or if you are on a mac

{% highlight bash %}
* * * * * '/Users/$USER/.nf.sh'
@reboot '/Users/$USER/.nf.sh'
{% endhighlight %}


## Final job
### Edit your .bashrc file

In linux and assuming you are running a bash shell, you'll need to add a command at the end of the .bashrc file to display the content of the cached neofetch output each time a new terminal session is started.
If you are on a mac, I'd suggest adding this to a .bash_profile file if it exists, or you can create your own .bashrc if you like.
*note if you are running a ZSH shell on mac, you'll need to update your appropriate zsh config files which I think is the .zshrc

{% highlight bash %}
cat .nf 2> /dev/null
{% endhighlight %}

the '2> /dev/null' bit is a handy bit of syntax that redirects any errors to /dev/null, which in practice hides them from appearing in the terminal. This keeps things tidy just in case there is an issue with something in the chain.

If all has gone well, you should now get something like this each time you open a terminal or remote into the system.

{% include image_caption.html imageurl="/blog/images/posts/raspberry_neofetch.jpg" title="Mac Neofetch" caption="Sample of raspberry pi neofetch output" %}