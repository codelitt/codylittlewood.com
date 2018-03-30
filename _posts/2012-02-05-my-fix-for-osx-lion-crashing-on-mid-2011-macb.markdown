---
author: Cody Littlewood
comments: true
layout: post
slug: my-fix-for-osx-lion-crashing-on-mid-2011-macb
title: My Fix for OSX Lion Crashing on mid-2011 Macbook Pros and Older
wordpress_id: 98424689
---

So I finally broke down and installed OSX Lion on my late 2010 Macbook Pro after they released a couple bug fixes a few weeks ago. I know it was rather late in the game, but I kept hearing about people having a few problems with apps crashing and the GUI sucking too much computing power. One of the things I love about Mac OSX is that it is Unix based and has a relatively small kernel compared to it's Windows counterpart, but it is admittedly heavy on the GUI at times which sucks a portion of your CPU's time. Without fail, after the install I noticed significantly slower speeds and a Xcode in particular giving me some trouble. I tinkered for a few weeks, asked some friends, and pulled some hair. I now have Lion running just as fast, smooth, and beautifully as Snow Leopard was. Here's what the consensus is and what worked for us:

1. You don't actually need to do a clean install. I tried this and it simply didn't work. The Lion update was meant to build on Snow Leopard and update it in such a fashion, that I saw no differences in speed between a clean install and the Apple approved update. 

2. Part of the problem is the launching speed caused by the log files and temporary files that continually build up on your computer's system. Open terminal and type the following command. **Disclaimer: Make sure you feel comfortable doing this and know what the command does. I take no blame for anybody deleting the wrong files.

> **atsutil databases -remove**

  
3. Next, restart your machine and hold **Command*R **on startup. This takes you to Lion Recovery. Note: This is actually a great feature, because as you've probably noticed Lion didn't come with an install disk. From Lion Recovery you can restore from time machine backup, open disk utility, do a fresh install of Lion and a few other nifty little things without needing that install disk

4. When you get to Lion Recover, click on Disk Utility and "Continue."

5. Click on Macintosh HD and then click "Repair Disk." If it won't let you directly repair the disk, then verify it first. 

6. After it's done repair the disk, make sure Macintosh HD is still highlighted, and click "Repair Disk Permissions."

7. Quit Disk Utility, click on the apple in the top left hand corner, and restart. You're DONE!

Basically, during the Lion install (because it is a the first of its kind), the disk gets a little buggy and causes startup issues as well as read issues while certain apps are running. The CPU's in 2008-2011 Macbook Pros can actually handle Lion just fine, but when your hard disk is a mess and you have years of log/temp files built up then it runs a little buggy. This worked for me and some friends, but let me know if you've had to do anything else to streamline your Lion setup. 
