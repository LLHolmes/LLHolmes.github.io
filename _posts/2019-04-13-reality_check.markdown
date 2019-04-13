---
layout: post
title:      "Reality Check"
date:       2019-04-13 21:48:15 +0000
permalink:  reality_check
---

Oh Rails project...

Did you hear that?  I audibly sighed.  I've been doing a lot of that this week.

This project has been a reality check for me.  To be fair, I've only been coding for 13 weeks, but up until this point I've felt like I've been rocketing along.  But fate put this project in my path this week specifically to put me in my place and remind me that I'm still very new at this.

Since it already had the required associations built in, I based [my Rails project](https://github.com/LLHolmes/yarn_stash_organizer) on my Sinatra Yarn Organizer project.  I thought it would give me a chance to clean it up a bit, add a kind of trading post, and do some CSS for the first time.  It's definitely cleaner code (helpers and partials are amazing) and I at least have a header with some super basic CSS, but nothing fancy and no trading post.  I did add another model to let a user add multiple notes to each project.  Unfortunately, basing my Rails project on something I'd already started didn't actually save me all that much time.

Frankly, I feel pretty disappointed.

By far the biggest problem was working with Devise and OmniAuth for sign in and authentification.  They're awesome gems with amazing functionality, but being the first time I've worked with Devise, much less the Devise/OmniAuth combination, it led to 1.5-2 days of Google learning.  I think I hit every possible issue.  I could be a fountain of information on this subject if I only remembered the specific problems and which of my multiple attempts to fix them acutally ended up solving each issue.  I'd like to go back and write out the process that actually worked for me, but for today, the wound is still too fresh.

Other than that, there were a handful of minor issues, mainly stemming from the magic of Rails.  I definitely learned a lot about nested forms, form builder objects, and collection_check_boxes.  I'm not entirely sure I managed the routes for some of the additional features effectively.  Trying to use Rails to mirror my Sinatra project (that is by it's nature more flexible) was a bit tough.  Even after attempting to break my Rails project for a few hours, I still ran into quite a few bugs when attempting to record my demonstration.  Finally, on the fourth try, I found another issue...and just kept on rolling.  I fixed it, of course, but I can't bring myself to do another demo.  Most of the issues I found when trying to record my demo were...“unintended consequences” of the Sinatra to Rails conversion.  Maybe I didn't remember all the functionality I had included in the Sinatra version until I was trying to use it extensively?  Maybe I thought I had already built it because I had created it in the original version?  Maybe I just did it wrong the first time in Rails?... Who knows.  At this point, I seriously doubt I've found all deviations between my vision and reality.  There are definitely things I'd like to change, but at some point I just need to cut myself off.

Right now I feel like I've just been though a super intense workout, the kind where you can feel it deep in your muscles when you try to stand up from your chair and you hobble down the stairs a bit.  After this Rails project, I'll be sore for a day or two.  But yeah...I'll be stronger on the other side of it.
