---
layout: post
title: "Raspberry pi watchdog"
date: 2024-04-06
featured_image: hardware_watchdog.jpg
tags: [raspberry pi, linux, dotfile, configuration]
---

Having run a raspberry pi as part of my custom weather station set up for many years, I got sick of having to manually restart the pi when it fritzed out, which seemed to happen every few weeks of so. 

I did some googling and found out that I wasn't the only one who was annoyed by this and found that hardware solution had been developed. 

## Hardware solution

I found the PiWatcher by a company called Omzlo - https://www.omzlo.com/articles/the-piwatcher 

I used this without issue for a good few years and it seemed to work with no problems at all. I then when looking for something unrelated came across an article that discussed a secret built in hardware watchdog that all the pi's had! 

## Inbuilt software solution 

To set up the hardware watchdog, follow these steps. This assumes you are using Raspberry pi OS.

#### OS set up

{% highlight bash %}
sudo nano /etc/systemd/system.conf

# add these to the end of the file
RuntimeWatchdogSec=15
RebootWatchdogSec=2min

# Reboot after saving the edits
{% endhighlight %}

#### Install and configure the application

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

#### Enable and startup the service

{% highlight bash %}
sudo systemctl enable watchdog
sudo systemctl start watchdog
{% endhighlight %}

