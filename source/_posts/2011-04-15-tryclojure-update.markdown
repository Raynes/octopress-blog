---
author: Raynes
date: '2011-04-15 19:30:00'
layout: post
slug: tryclojure-update
status: publish
title: TryClojure update
wordpress_id: '136'
categories:
- Clojure
---

I've been seeing quite a few
[complaints](http://groups.google.com/group/clojure/msg/3c2a21ee61165c24)
about [TryClojure](http://try-clojure.org)'s stability/uptime lately.
Understandably. The uptime of the site has been unbelievably pathetic.
When I started TryClojure, Licenser, the author of its original sandbox,
bought the try-clojure.org domain and hosted the site on his own server
because I didn't have anywhere to host it myself. His server used to run
SunOS and now it runs OpenIndia. Both of which I am insanely unfamiliar
with. This limited my ability to maintain the server and such. These
complaints pointed out an obvious problem, and one that I am currently
addressing and want to talk about here. My first step was to move
TryClojure to Alan Malloy's and my Linode VPS. As of right now, this
server has been up for over 130 days since the very day we purchased it.
Server uptime will not be a problem. Furthermore, I have the utmost
control over the server and what happens to TryClojure. I can monitor
errors and figure out problems that might make the site go down. A while
back, I did a bit of a [call to
action](http://groups.google.com/group/clojure/browse_thread/thread/21a0f7c45fb8c112?fwc=1)
for TryClojure. With the help of the people who participated, TryClojure
was moved to Clojure 1.2, all dependencies updated, a [new
sandbox](http://github.com/cognitivedissonance/clojail), and a fantastic
new design. Unfortunately, some things didn't get done, people lost
interest, and once again TryClojure was left in the dust. I've been
fairly busy lately. With the book and general everyday life, my whiny
ass gets a little overwhelmed. Over the past week, I've been **making**
time for TryClojure and I'll keep doing that until TryClojure meets its
expectations. My problem is that I'm just not that good at web
development. TryClojure was my first and is my only serious webdev
project. I can't design a website for shit (thankfully, I know people
who can) and I don't know JavaScript. When I have to write JavaScript, I
rely on JQuery and JS documentation to get me by. I'm learning slowly,
but I don't use it enough to know it at this point, which is unfortunate
for TryClojure. However, I know people who do know JavaScript, and some
of those people don't mind helping out. This brings me to the actual
point of this post. TryClojure is now live on my server with a huge
update.

-   Everybody wanted a TryHaskellish interactive tutorial, well now
    you've got one! Thanks to mefesto from \#clojure on IRC, we've been
    able to get an interactive tutorial up and running.
-   I've rewritten all the tutorial content and am working on adding
    more content to it over the next couple of weeks.
-   I've got rid of syntax highlighting in the tutorial and in the
    console, because it was never all that useful in the first place and
    required a jquery-console hack to work that meant I couldn't update
    jqeury-console without doing that hack over again.
-   Because of this, I've been able to update jquery-console to the
    newest version which allows for copying and pasting! Copying and
    pasting was another of the big gripes (once again, understandably).
    You can do it now.
-   Clojail has been updated to the latest version (which is almost
    always the same version that sexpbot uses), and I'll be updating the
    rest of the dependencies (ring and such) to the latest versions
    soon.

All of these changes are brand new and not guaranteed to be bug free.
I'm still in the process of putting finishing touches on everything.
Finally, I need to make one last point. Do you know how many TryClojure
bug reports I've received since it was conceived (early last year, I
believe)? Less than 10. Hell, maybe around 5. I don't really use it, so
I rarely find these bugs myself. If they are left unreported, I may
never notice them. Remember google groups thread I linked earlier?
That's where these issues were raised. When those posts were made,
TryClojure was down. As a matter of fact, that's why they were made in
the first place. I did not know that it was down. As a matter of fact, I
didn't even see those group posts until Alan mentioned it to me! I was
kind of shocked that they were arguing about stability issues and
TryClojure's uptime without even letting me know that it was down in the
first place so that I could figure out the problem. It is really easy to
contact me. I'm always on IRC on \#clojure and \#sexpbot, my Github page
is linked from TryClojure's about page, and my Github page even has my
email address on it. You can send a telegram if you want, I don't care,
just please let me know if something is wrong or else I can't do
anything about it. I am open to ideas as to how to make TryClojure
better. If you've got an idea, by all means, pull the **shit** out of
the repository and start implementing stuff. The great thing about open
source is that we can all make TryClojure better together. I can't do it
alone.
