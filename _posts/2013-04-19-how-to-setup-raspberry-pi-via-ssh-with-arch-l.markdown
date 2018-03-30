---
author: Cody Littlewood
comments: true
layout: post
slug: how-to-setup-raspberry-pi-via-ssh-with-arch-l
title: How to setup Raspberry Pi via SSH with Arch Linux (No monitor, no keyboard)
wordpress_id: 176648323
categories:
- arch
- arch linux
- linux
- raspberry pi
- ssh
---

The information provided will help you find your RPi on the network and get you connected and logged in via SSH to your RPi to begin your install and setup. If you don't already have a [Raspberry Pi you can order one here.](http://www.amazon.com/gp/product/B009SQQF9C/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B009SQQF9C&linkCode=as2&tag=codelitt-20) Once you get to this step, check out the [Arch Linux Beginner's Guide](https://wiki.archlinux.org/index.php/Beginners'_Guide) and [Arch Linux Installation Wiki](https://wiki.archlinux.org/index.php/Installation_Guide) for the best information.

Specs:

  * [Raspberry Pi board](http://www.amazon.com/gp/product/B009SQQF9C/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B009SQQF9C&linkCode=as2&tag=codelitt-20). 
  * SSH on your Mac or Linux computer (Macs and most distros come with it preinstalled)
  * [SD card](http://www.amazon.com/gp/product/B00B7ID99I/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B00B7ID99I&linkCode=as2&tag=codelitt-20) loaded with [Arch Linux](http://archlinuxarm.org/) - [RPi Easy SD Card Setup](http://elinux.org/RPi_Easy_SD_Card_Setup) (Could be Raspbian distro as well but change SSH login details in steps 8 and 9 to: pi (username) and raspberry (password). I like the [Sony Class 10 card](http://www.amazon.com/gp/product/B00B7ID99I/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B00B7ID99I&linkCode=as2&tag=codelitt-20), but it's up to you.
  * Raspberry Pi connected via ethernet to the network.

1. Plug in your RPi and give it about 2 minutes (just to be sure) to get fired up

2. On your Mac or Linux open a terminal. Both come preinstalled. Search "terminal" if you have problems finding it.

3. In the terminal window, search for the broadcast IP on your local network by:

` ifconfig | grep broadcast `

It should return something like this:

` inet 192.168.0.3 netmask 0xffffff00 broadcast 192.168.0.255 `

4. Copy the broadcast IP (Mine is 192.168.0.255) and then ping the broadcast IP with this:

Linux:

` ping -b 192.168.0.255 `

Mac:

` ping 192.168.0.255 `

It should begin to return something like this:

` 64 bytes from 192.168.0.3: icmp_seq=1 ttl=64 time=46.7 ms  64 bytes from 192.168.0.2: icmp_seq=1 ttl=64 time=74.1 ms (DUP!)  64 bytes from 192.168.0.3: icmp_seq=2 ttl=64 time=69.8 ms `

5. Use CTRL + c to exit out of the ping

6. Now to find which other IP's are connected on your network (one of which is the RPi) run:

` arp -a `

which should return something like:

` ? (192.168.0.1) at 21:b3:17:94:f4:71 [ether] on eth0  ? (192.168.0.7) at f3:9e:df:e1:32:34c [ether] on eth0 `

7. The first number in each line is an IP of a device connected to the network. Normal routers use what is called DHCP or Dynamic Host Configuration Protocol to "lease" IP's to devices connecting to the network so they can be identified. If a lease expires or a device removes itself from the network then this frees up the IP to be assigned to a new leasee upon request. In most situations, routers assign IP's starting from low to high and if your RPi was the last device connected you can make a reasonable guess that it is the highest IP.

Copy this number and use it for the next steps. If these steps do not work then it stands to reason that your RPi is one of the other (hopefully) few IP's listed. So give those a try before asking a question in the comment section or on [Twitter.](http://twitter.com/codelitt)

8. In your terminal window run:

` ssh root@192.168.0.7 (or whatever IP got from steps 6,7) `

9. It will prompt you for a password, so enter the default Arch password:

` root `

10. All done. You are logged in via SSH to your Raspberry Pi and now can continue with installation as normal.
