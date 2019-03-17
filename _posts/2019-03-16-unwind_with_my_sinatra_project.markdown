---
layout: post
title:      "Unwind with my Sinatra Project!"
date:       2019-03-17 00:28:41 +0000
permalink:  unwind_with_my_sinatra_project
---

Big news.  BIG news with this project!  I had a gem issue and I fixed it all by myself!!!  (For me this is a big stride, so let me tell you about it quick before I get into my actual project.)

I ran into a small hiccup and needed to ammend one of the columns on one of my database tables.  No biggie.  So I tried to create a new migration file to make the change.  Instead of creating a file, I got:  

You must use Bundler 2 or greater with this lockfile.

It was an error.  It was red.  You all know how I feel about gems and the like – I was nervous.  
But it didn't look it was capable of burning down the world so I took a stab at it.  I googled.  I looked at my version.  I deleted the gemfile and re-ran ```bundle install```.  I googled some more and tried ```gem update --system``` (because it didn't look too intimidating), and that was it!  Fixed!  It took less than 5 minutes to figure out.  What have I been so worried about???  I realize it was a super minor fix, but for me (someone who a few months ago didn't even know that her computer even HAD a terminal, much less how to use it) it feels like a definitive step in the right direction.

As for my actual Sinatra project, I created an organizer for my yarn stash.  Yes, I have a yarn stash, and like any good crochetter/knitter, it is out of control!  I wanted a place to keep track of what yarn I own, what project I bought it for, and how much of it I have.  I made it a little more complicated than it was required to be by having five models with varying relationships, but it helps make it more condusive for what I want to use it for.  I also added a couple extra CRUD features like creating multiple resources from one form and automatically updating items with associated relationships when a project is deleted or the status is updated to “Finished”.

When I started my Sinatra project, I sat down to think through the flow I wanted to implement.  Then when I started building it, I ignored that time spent thinking things through and went through the process of creating ALL of the restful routes for EACH of my models (whether I really needed or wanted them or not).  In the end, I made way more erb files than I needed and reworked a few key index files a few times.  Not be best use of my time, but I suppose it was good practice.  It's not as pretty as I had hoped it would be – I never got around to attempting to do the CSS since I spent so much time rethinking the functionality.  Overall, this project taught me about pre-planning and then sticking with that plan!

If you have some yarn you need to wrangle or are just curious, please check out my Sinatra [Yarn Organizer](https://github.com/LLHolmes/yarn_organizer).  I'd love to know your thoughts!
