---
layout: post
title: "Conky set up"
date: 2024-03-29
featured_image: conky.jpg
tags: [neofetch, linux. dotfiles, configuration]
---

Conky has been around on the linux desktop (and ports to mac) for 15+ years now. A handy bit of screen candy that fills a similar job to neofetch but on the desktop envrionement. Some people spend a lot of time ricing tweaking the configuration to match with the rest of their set up to be 'just so'. My config is neither pretty or perfect, but it is functional for me and shows me what I need to know on my system. 

As is that tradition, Conkyrc files are often shared and modified online - this is my verison - with a few changes to meet my requirements. 

This version of configuration is run on my laptop that is either: 
1. Docked to my main display, power, ethernet and other accessories via usb C.  
2. On battery power away from a charger

Main change is a collection of if statement to show certain bits of data depending on situation 

## Ethernet
This bit of code detects if there is an active ethernet category and if there is, displays the elements within the if statement. 
{% highlight bash %}

${if_existing  /sys/class/net/enx3448ed762883/address} 
${color0}Wired (${addr enx3448ed762883})
${color0}Ethernet MAC Address: $color${execi 99999 cat /sys/class/net/enx3448ed762883/address }
${color0}Down:$color ${downspeed enx3448ed762883}/s ${alignr}${color0}Up:$color ${upspeed enx3448ed762883}/s
${color0}Total:$color ${totaldown enx3448ed762883} ${alignr}${color0}Total: $color${totalup enx3448ed762883}
${color0}${downspeedgraph enx3448ed762883 25,120 000000 00ff00} ${alignr}${upspeedgraph enx3448ed762883 25,120 000000 ff0000}$color
${stippled_hr 2}$endif

{% endhighlight %}

## Battery Bar colour
A long nested if to show a different colour of battery bar based on the percentage of the battery, colours will need to be set to what is appropriate for your own set up. 

{% highlight bash %}
${if_match ${battery_percent} <= 20}${if_match ${battery_percent} >=1}${color1}${battery_bar}$color${endif}${endif}\
${if_match ${battery_percent} <= 40}${if_match ${battery_percent} >=20}${color2}${battery_bar}$color${endif}${endif}\
${if_match ${battery_percent} <= 70}${if_match ${battery_percent} >=40}${color2}${battery_bar}$color${endif}${endif}\
${if_match ${battery_percent} <= 80}${if_match ${battery_percent} >=70}${color2}${battery_bar}$color${endif}${endif}\
${if_match ${battery_percent} <= 100}${if_match ${battery_percent} >=80}${color7}${battery_bar}$color${endif}${endif}
{% endhighlight %}




{% highlight bash %}
##################### references #######################
### - https://conky.sourceforge.net/variables.html - ###
######### - https://www.mankier.com/1/conky - ##########
########################################################

#### ToDo #####

# Spotify link
# https://github.com/Madh93/conky-spotify

# -------------------- Conky's Run Time Parameters -------------------- #
update_interval 1                       # Conky update interval in seconds
total_run_times 0                       # Number of updates before quitting.  Set to zero to run forever.
no_buffers yes                          # Subtract file system buffers from used memory?
cpu_avg_samples 5                       # Number of cpu samples to average. Set to 1 to disable averaging
net_avg_samples 5                       # Number of net samples to average. Set to 1 to disable averaging

# -------------------- Conky's General Look &amp; Feel -------------------- #
# --- defualt values --- #
default_color grey                      # Default color and border color
default_bar_size 0 6                    # Specify a default width and height for bars.
default_gauge_size 25 25                # Specify a default width and height for gauges.
default_graph_size 0 25                 # Specify a default width and height for graphs.
default_outline_color green             # Default border and text outline color
default_shade_color yellow              # Default border and text shading color

# --- predefined colors - http://www.kgym.jp/freesoft/xrgb.html --- #
color0 FFFFFF                           # white
color1 FFA500                           # orange
color2 B22222                           # firebrick
color3 696969                           # dim gray
color4 D3D3D3                           # light gray
color5 2F4F4F                           # dark slate gray
color6 FFEC8B                           # light golden rod
color7 54FF9F                           # sea green
color8 FF8C69                           # salmon
color9 FFE7BA                           # wheat

# --- window layout &amp; options --- #
own_window true                          # Conky creates its own window instead of using desktop
own_window_type desktop                  # If own_window is yes, use type normal, desktop, or override
own_window_transparent false             # Use pseudo transparency with own_window?
own_window_colour black                  # If own_window_transparent is no, set the background colour
own_window_argb_visual true
own_window_argb_value 100
double_buffer yes                       # Use double buffering (reduces flicker)
use_spacer right                        # Adds spaces to stop object from moving
maximum_width 600                       # Maximum width of window in pixels
own_window_hints undecorated,below,sticky,skip_taskbar,skip_pager

# --- window placment --- #
alignment top_right

# --- borders, margins, and outlines --- #
draw_graph_borders yes                  # Do you want to draw borders around graphs
border_inner_margin 9                   # Window's inner border margin (in pixels)
border_outer_margin 5                   # Window's outer border margin (in pixels)
gap_x 10                                # Gap between borders of screen and text (on x-axis)
gap_y 40                                # Gap between borders of screen and text (on y-axis)
border_width 10                         # Window's border width (in pixels)

# --- Text --- #
draw_outline no                         # Do you want ot draw outlines
draw_shades no                          # Do you want to draw shades
draw_borders no                         # Do you want to draw borders around text
uppercase no                            # set to yes if you want all text to be in uppercase
use_xft yes                             # use the X FreeType interface library (anti-aliased font)
xftfont Hack:size=10.5:weight=bold    # Xft font to be used

# -------------------- Conky's Displayed System Monitoring Parameters ------------------- #
TEXT
# Title / Banner message
${color4} 
${alignc}${font Hack:size=30}${time %H:%M}${font}
${alignc}${time %A} ${time %B} ${time %d},${time %Y}
$color 
# General system information
${color1}SYSTEM INFORMATION ${hr 2}$color
${color0}Host: ${alignr}$color$nodename
${color0}User: ${alignr}$color${execi 999 whoami}${color}
${color0}Uptime: ${alignr}$color$uptime
${color0}Distro: ${alignr}$color${execi 999 lsb_release -ds}$color 
${color0}Kernel: ${alignr}$color$kernel
${color0}Arch: ${alignr}$color$machine 

${color1}BATTERY INFORMATION ${hr 2}$color
${color0}Battery charge: ${battery_percent}% $color
${color0}AC adapter: $color$acpiacadapter
${if_match ${battery_percent} <= 20}${if_match ${battery_percent} >=1}${color1}${battery_bar}$color${endif}${endif}\
${if_match ${battery_percent} <= 40}${if_match ${battery_percent} >=20}${color2}${battery_bar}$color${endif}${endif}\
${if_match ${battery_percent} <= 70}${if_match ${battery_percent} >=40}${color2}${battery_bar}$color${endif}${endif}\
${if_match ${battery_percent} <= 80}${if_match ${battery_percent} >=70}${color2}${battery_bar}$color${endif}${endif}\
${if_match ${battery_percent} <= 100}${if_match ${battery_percent} >=80}${color7}${battery_bar}$color${endif}${endif}

# CPU information
${color1}CPU ${hr 2}$color
${color7}${cpugraph 000000 00ff00}$color
${color0}CPU Model: $color${execi 999 cat /proc/cpuinfo | grep 'model name' | awk '{ print $4 " " $6 " " $8 " " $9 }' | head -n1}
#${color0}Avg. Load: $color $loadavg
${color0}Frequency: $color$freq MHz
${color0}CPU Temperature: ${execi 5 sensors | grep "Package" | awk '{ print $4}'}
${color0}CPU Usage:$color $cpu% ${color7}${cpubar}

# Top running processes
${color1}TOP 5 PROCESSES ${hr 2}$color
${color0} NAME              PID      CPU %  MEM$color
 ${top name 1} ${top pid 1} ${top cpu 1} ${top mem 1}
 ${top name 2} ${top pid 2} ${top cpu 2} ${top mem 2}
 ${top name 3} ${top pid 3} ${top cpu 3} ${top mem 3}
 ${top name 4} ${top pid 4} ${top cpu 4} ${top mem 4}
 ${top name 5} ${top pid 5} ${top cpu 5} ${top mem 5}

# Memory and swap space untilization
${color1}MEMORY & SWAP ${hr 2}$color
${memgraph 0000ff 00ff00}$color
${color0}RAM Usage: ${color}$mem / $memmax
$memperc% ${color6}${membar}$color
#${stippled_hr 2}
${color0}Swap Usage: ${color}$swap / $swapmax
$swapperc% ${color6}${swapbar}$color

# File System utilization
${color1}FILE SYSTEM ${hr 2}$color
${color0}NVME 256:$color ${fs_used /} / ${fs_size /}
${fs_used_perc /}% ${color8}${fs_bar /}$color

# Ethernet utilization
${color1}NETWORKING ${hr 2}$color ${if_existing  /sys/class/net/enx3448ed762883/address} 
${color0}Wired (${addr enx3448ed762883})
${color0}Ethernet MAC Address: $color${execi 99999 cat /sys/class/net/enx3448ed762883/address }
${color0}Down:$color ${downspeed enx3448ed762883}/s ${alignr}${color0}Up:$color ${upspeed enx3448ed762883}/s
${color0}Total:$color ${totaldown enx3448ed762883} ${alignr}${color0}Total: $color${totalup enx3448ed762883}
${color0}${downspeedgraph enx3448ed762883 25,120 000000 00ff00} ${alignr}${upspeedgraph enx3448ed762883 25,120 000000 ff0000}$color
${stippled_hr 2}$endif
# Wireless networking
${color0}Wireless (${addr wlo1})
${color0}WiFi MAC Address: $color${execi 99999 cat /sys/class/net/wlo1/address }
${color0}Down:$color ${downspeed wlo1}/s ${alignr}${color0}Up:$color ${upspeed wlo1}/s
${color0}Total:$color ${totaldown wlo1} ${alignr}${color0}Total: $color${totalup wlo1}
${color0}${downspeedgraph wlo1 25,120 000000 00ff00} ${alignr}${upspeedgraph wlo1 25,120 000000 ff0000}$color
{% endhighlight %}
