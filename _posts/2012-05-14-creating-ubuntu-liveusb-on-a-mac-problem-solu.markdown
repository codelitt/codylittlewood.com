---
author: Cody Littlewood
comments: true
layout: post
slug: creating-ubuntu-liveusb-on-a-mac-problem-solu
title: Creating Ubuntu LiveUSB on a Mac Problem & Solution!
wordpress_id: 130760791
categories:
- linux
- ubuntu
- liveusb
---

The Ubuntu site has [excellent documentation](http://www.ubuntu.com/download/help/create-a-usb-stick-on-mac-osx), but I kept running into an error when it came time to mount the image onto the USB so that I could have a LiveUSB ready to go. This slimy error read, "The disk you inserted was not readable by this computer." It would happen everytime I tried to execute sudo dd if=/path/to/downloaded.img of=/dev/rdiskN bs=1m. 

I had quite the search trying to find a simple solution for this. I attempted to mount the image multiple times and nothing seemed to with the USB full partition set as MS-DOS. The fix was quite simple and a small program that does all of the steps for you once you've downloaded the OS from the Ubuntu site. Just go to [http://penguintosh.com/](http://penguintosh.com/) for the readme or download the program directly at [http://www.mediafire.com/?rctco14mzjayaa9](http://www.mediafire.com/?rctco14mzjayaa9). The program comes with detailed instructions and handles the entire process for you. Let me know if this is of any help! 

[Follow me on Twitter ](http://twitter.com/codelitt)
