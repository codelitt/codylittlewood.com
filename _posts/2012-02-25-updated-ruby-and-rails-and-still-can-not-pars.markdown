---
author: Cody Littlewood
comments: true
layout: post
slug: updated-ruby-and-rails-and-still-can-not-pars
title: 'Updated Ruby and Rails and still can not parse yaml with Psych? Here''s my
  fix:'
wordpress_id: 105188905
categories:
- parser engine
- psych
- ruby on rails
---

Today while updating the en.yml file for my rails app, I hit a snag and continually kept getting told that the yaml could not be parsed by the new parsing engine Psych. I was already updated to Rails 3.2 and Ruby 1.9.3 so this problem was supposedly fixed when you look at the documentation. Still, I could not get past parsing errors for one simple line of code.

I looked for answers everywhere only to find people saying to "force Rails 3.2 back to the Syck parsing engine." I really hate relying on outdated and unsupported engines whenever I can avoid it. It took a couple ABSOLUTELY INFURIATING hours, but here's what worked for me:

  * Open up config/boot.rb require yaml and _define the parsing engine as Psych. _It should contain something like this: 

[https://gist.github.com/1912763](https://gist.github.com/1912763)

  * The next thing that you need to watch for (if you've changed your en.yml file at all) is that you're using the correct amount of spaces for the yaml syntax. There should be two spaces in between lines. I was hacking together a solution so my error message says "Password" instead of "Password Digest." And yes, I understand that this is a hacked together solution. If you don't have the correct syntax for the yaml code then you will get an error similar to an "Undefined Method" error. Here's what my code looked like with the syntax:

[https://gist.github.com/1912780](https://gist.github.com/1912780)

Edit: I'm now having trouble reproducing the errors, so there may be another item to do. The only other item I did was installing and uninstalling the psych gem. I realised that the gem doesn't need to be included because the engine is already included in updated Ruby and Rails.

If you have any questions or think this is completely bogus, leave a comment. :)

-codelitt
