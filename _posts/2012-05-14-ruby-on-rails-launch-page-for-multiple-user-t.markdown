---
author: Cody Littlewood
comments: true
layout: post
slug: ruby-on-rails-launch-page-for-multiple-user-t
title: Ruby on Rails Launch Page for Multiple User Types - Open-Source
wordpress_id: 130760838
categories:
- ruby on rails
- ruby 
- startups
- launch page
---

There are quite a few options for one click pre-beta launch pages for the startup world right now. [Launchrock](http://launchrock.com) is the first example that leaps to mind and they  really do an awesome job, but we ran into the problem that we have a two-sided market. Here at [Backstagr](http://backsta.gr), we needed a prelaunch page that represented that.  Specifically, we needed multiple user types to be able to signup and indicate which user type they were for our records. First of all, we want to be able to deliver them a customised experience when they come back to sign in for that first time, and secondly we want to see the interest level of both sides of the market. Since we couldn't find a good solution out there that allowed for multiple user types, we did what every good development team (in our humble opinion) should do-- we built the tool and released it into the open-source world for anyone to tweak, fix, test, and use. 

I've completed the back-end server/database (and VERY BASIC front-end) side of things, and over the next couple weeks the talented Mike Babb will be adding the much needed front-end styling and Vicky Jaime will be adding some design. Once we have those two items completed, you'll be able to see a working example at the [Backstagr](http://backsta.gr) homepage. 

When Mike adds the front-end styling, you'll probably want to keep the styling, but utilise design assets which match your own brand. Other than that, and a few tweaks to the setup_mailer.rb/database.yml files, you'll have a working multiple user type launch page for your startup with just an hour of setup. Please feel free to fork the repo on Github, make pull requests, or submit issues. We all have full-time day jobs, so we only get nights and weekends to work on these things, but I'll do my best to keep up on them. Without further ado, [here is the link to the repo! ](https://github.com/codelitt/launchpage-rails)

Follow us on Twitter: [@codelitt](http://twitter.com/codelitt) [@mikegbabb](http://twitter.com/mikegbabb) [@vickyjaime](http://twitter.com/vickyjaime) and [@backstagr](http://twitter.com/backstagr)
