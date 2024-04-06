---
layout: post
title: "Raspberry pi watchdog"
date: 2024-04-06
featured_image: .jpg
tags: [raspberry pi, linux, dotfile, configuration]
---

Having run a raspberry pi as part of my custom weather station set up for many years, I got sick of having to manually restart the pi when it fritzed out every few weeks. 

I did some googling and found out that I wasn't the only one who was annoyed by this and found that hardware solution had been developed. 

## Hardware solution

I found the PiWatcher by a company called Omzlo - https://www.omzlo.com/articles/the-piwatcher 

## Inbuilt software solution 

{% highlight bash %}
sudo nano /etc/systemd/system.conf

# add these to the end of the file
RuntimeWatchdogSec=15
RebootWatchdogSec=2min
{% endhighlight %}




{% highlight bash %}
sudo apt install watchdog
{% endhighlight %}

{% highlight bash %}
sudo nano /etc/watchdog.conf

# clear all the entires in the file and list these

realtime                = yes
priority                = 1
watchdog-device         = /dev/watchdog
watchdog-timeout        = 15
max-load-1              = 24
# if your device isn't going to be connected to a network wirelessly - I'd not include this one. 
interface               = wlan0

# Save changes and exit nano
{% endhighlight %}



{% highlight bash %}
sudo systemctl enable watchdog
sudo systemctl start watchdog
{% endhighlight %}

