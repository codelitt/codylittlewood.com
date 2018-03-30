---
layout: post
title: "PiFace Digital IO on RaspberryPi with Arch Linux ARM"
date: 2013-05-16 16:17
comments: true
categories: 
- raspberry pi
- arch linux
- piface
- code
- linux
---

I'm currently the process of building a [RaspberryPi](http://www.amazon.com/gp/product/B009SQQF9C/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B009SQQF9C&linkCode=as2&tag=codelitt-20) setup to basically control my entire home with a WebUI that I can access from anywhere. I need the capabilities to provide surveillance, control the lights, regulate temperature, and control my media centre/sound system throughout the home. Accessing these controls from any device remotely is a huge appeal; not to mention the convenience and peace of mind.

There are lots of open-source projects for XMBC which will take care of my media centre, a few surveillance projects, and a WebUI project so I figured I'd get started on the lighting and temperature. Luckily there is the [PiFace Digital IO](http://www.amazon.com/gp/product/B00BBK072Y/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B00BBK072Y&linkCode=as2&tag=codelitt-20) which offers a super simple input/output board and plugs directly into your RPi. 

If you're missing either pieces you can pick up the [RaspberryPi here](http://www.amazon.com/gp/product/B009SQQF9C/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B009SQQF9C&linkCode=as2&tag=codelitt-20) and the [PiFace Digital IO here](http://www.amazon.com/gp/product/B00BBK072Y/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B00BBK072Y&linkCode=as2&tag=codelitt-20).

{% img /images/raspberrypiface.JPG [600 [700]] ['RaspberryPi with PiFace' | 'RaspberryPi with PiFace'] %}

There was already a library out there for the board, but I use Arch Linux ARM on my Pi's. Arch doesn't come with Python3, make, or gcc preinstalled and the library depends on these. Since I already was going to fork the PiFace library, I decided you add the dependencies into the install script to make it easier for others. It uses pacman to install the dependencies so you shouldn't have any conflicts or issues. 

You can find the [PiFace library for Arch Linux ARM here.](https://github.com/codelitt/pifacedigitalioArch)

Install git with:

` pacman -S git `

Configure git.

Clone the library:

` git clone https://github.com/codelitt/pifacedigitalioArch `

Change directories:

` cd pifacedigitalioArch `

And run the install script:

` ./install.sh ` 

You may have to sudo if you are not already root. Answer "y" and hit enter when the install script prompts you if the packages are not already installed. 


