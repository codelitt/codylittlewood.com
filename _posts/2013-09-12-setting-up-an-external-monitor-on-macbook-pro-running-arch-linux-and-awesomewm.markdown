---
layout: post
title: "Setting up an  external monitor on Macbook Pro running Arch Linux and AwesomeWM"
date: 2013-09-12 22:35
comments: true
categories:
- arch linux
- macbookpro
- linux
- code
- apple running linux
- external devices
- apple cinema display
- apple usb keyboard
- apple usb mouse 
---

I really don't need any more power than my [Macbook Pro](http://www.amazon.com/gp/product/B0074703CM/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B0074703CM&linkCode=as2&tag=codelitt-20) running Arch Linux. The distro is tiny and extremely powerful so with an SSD upgrade, it is more than enough. I do, however, hate spending much time in front of a 13 inch screen. I use my laptop as my work station, but plug in an Apple Cinema Display (already had it, but it is beautiful and huge) and an Apple USB keyboard and mouse. Apple Cinema is a little bit of an overkill really, but I'm a huge fan of their keyboards. They have the perfect amount of give for me.

Here's the product list:

* [Macbook Pro](http://www.amazon.com/gp/product/B0074703CM/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B0074703CM&linkCode=as2&tag=codelitt-20) outfitted with a [Samsung 840 Series SSD](http://www.amazon.com/gp/product/B009NB8WRU/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B009NB8WRU&linkCode=as2&tag=codelitt-20) Harddrive type should not change any of these instructions though. 
* [Apple Cinema Display](http://www.amazon.com/gp/product/B0002ILKMW/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B0002ILKMW&linkCode=as2&tag=codelitt-20) but any external monitor will work.
* [Apple USB Keyboard](http://www.amazon.com/gp/product/B0002ILKMW/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B0002ILKMW&linkCode=as2&tag=codelitt-20), but again, any keyboard will work.
* Apple USB Mouse, but any mouse will work. (Sorry. No link for that guy. It's discontinued)

*If you're wanting the Bluetooth setup, for now you'll have to check out the wiki.* 

### Dual Monitors

Getting Dual monitors setup with Mini DisplayPort is pretty easy with Arch Linux running AwesomeWM.

AwesomeWM comes with a wicked litle tool called XRandR that handles all of your dual monitor settings. I found it much easier than dealing with Xorg or X11 conf files. 

To query your hardware you need to run:

`xrandr -q`

Your output will list devices availble and what's connected to what port:
{% codeblock [bash output] %}
LVDS-0 connected primary 1280x800+0+0 (normal left inverted right x axis y axis) 286mm x 179mm
   1280x800       60.2+
DP-0 disconnected (normal left inverted right x axis y axis)
DP-1 disconnected (normal left inverted right x axis y axis)
   2560x1440      60.0+
   1280x720       59.9 
{% endcodeblock %} 

When you check this output, you'll notice that your LVDS-0 is connected. Your monitor that is plugged in to the DisplayPort is disconnected and lists available display size options. Too connect your monitor, you'll tell XRandR to output to this screen as well and specify your resolution desired. You can also decide where the monitor will be located in relation to your primary screen. There is more information for alternative settings in the [AwesomeWM Wiki.](http://awesome.naquadah.org/wiki/Using_Multiple_Screens). I ran:

`xrandr --output DP-0 --mode 2560x1440 --right-of LVDS-0`

You should see your monitor fire up without any issues. 

### Apple USB Keyboard
A USB keyboard is easy. It should work as soon as you plug it in either to the back of your monitor or directly to the laptop. 

### Apple USB Mouse

Same thing. Plug and play. I prefer to plug both the keyboard and mouse into the monitor. When I'm arriving or leaving work, I only have to plug/unplug in the power, MiniDisplay and one USB cable to the laptop. 

If you run into any problems, head to the comments, the Arch Wiki, or ping me on Twitter [@codelitt](http://twitter.com/codelitt)
