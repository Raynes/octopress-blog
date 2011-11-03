---
layout: post
title: "Moving Try Clojure to Heroku"
date: 2011-11-03 00:46
comments: true
categories: 
- Clojure
- Heroku
- TryClojure
---

tl;dr: At first I was like "uhh..." but then I was like "AH!".

I started playing with Heroku a bit recently. I had heard all sorts of wonderful things about it and figured it was worth giving a go. I never expected to get anything done because I wasn't willing to put a lot of effort into it, since I was planning on just playing around with things.

So, my plan was to see if I could get [Try Clojure](http://tryclj.com) running on Heroku. I took a seat and began reading documentation. Well, if a two minute blog post is documentation. The funny thing is, it wasn't until I actually decided to move it to Heroku permenantly that I actually had to read official Heroku documentation, and that was only to find out how to get my domain pointed at the Heroku instance and to find out more info about how much the site would be able to handle on the free plan.

It took me all of 10 minutes to get Try Clojure running on Heroku. The only reason it took that long was because I didn't know that I had to start my server on a port that Heroku sets the environment variable PORT to. So, about 8 of those 10 minutes were me committing a fix for that. It was less a less than 5 line change. 

I had to modify its `project.clj` file to add a :jvm-opts line to set the Java policy file for the application, but that was only because cake reads jvm options from `.cake/config` and Leiningen takes a key in project.clj for them. At this point in time, Heroku only supports Leiningen on the Clojure stack. I've been told that cake is not out of the question in the future. It doesn't really matter because it was a good idea to add :jvm-opts to my project.clj so that I could maintain complete Leiningen compatibility in Try Clojure.I have absolutely nothing against Heroku for not supporting cake as well as lein &ndash; it simply isn't a big deal.

Finally, I had to create a proc file containing the following:

    web: lein run
    
and this tells Heroku how to run my code.

After my code was set up, all I had to do was `sudo gem install heroku` and then `heroku create --stack cedar` and bang, my project was a Heroku project. To deploy, I simply ran `git push heroku master`, which pushed my code to the `heroku` remote that heroku set up when I ran the `create` task. The code was pushed, it ran my code with `lein run`, shot me a link to the app, and tada &ndash; Try Clojure runs on Heroku.

I was completely floored by how easy all of this was. And that's how easy it **always** is. When I want to deploy, I just push my code to the heroku remote with git and Heroku handles the restarting and everything with absolutely no work on my part *at all*. This workflow won me over immediately.

So great, I have Try Clojure running on Heroku. Just an experiment and it worked really well. At this point, I had no intention of moving to Heroku permenantly. I didn't know much about Heroku and how much the free account would give me.

I mentioned that it was running on Heroku on Twitter (and probably in the #clojure IRC channel as well, but I don't quite remember). The next day, [technomancy](http://technomancy.us/) (Phil Hagelburg) asked me in the IRC channel if the main Try Clojure instance was running on Heroku, and I told him the equivalent of "Hell no, I don't know anything about Heroku and I only have a free account. I don't trust Heroku enough to move it over.". It struck me as odd when he replied with "Heh" followed by a cute joke, but it didn't dawn on me why he did it.

The next day, I remembered that technomancy had just recently started working at Heroku himself! One of our very own popular Clojurians! I ended up talking with him (he kindly approached me, offering to help) later that day. I read a lot of documentation around the same time and asked him a few questions.

What confused me the most was the concept of a 'dyno'. This is a process of any type running on Heroku. Processes for stuff like Mongrel or Resque and, of course, your app and its own processes. In my case, I have one web process, which is what I defined in my Procfile.

Heroku takes a time-based approach to its services and billing. The idea is that you pay for how many hours each dyno that you use has run in a given month, and every free account has 750 free dyno hours. If I only run one dyno all month, I don't have to pay anything at all. And even if an app that I use was to get slashdotted or something similar that would generate massive amounts of traffic, all I'd have to do is run a command or two to scale things up. I could add a dyno or two and only run them for as long as the traffic was too hard for a single dyno to handle the traffic, then I could scale it back down and I'd hardly have to pay anything at all unless I used those dynos for quite a while. You only pay for what you use, and you aren't obligated to use anything more than what you need/want!

This all made me feel much better about the whole thing. Try Clojure is a fairly simple site that doesn't get *tons* of traffic, so I decided I'd go ahead and move the main instance to Heroku. I even bought a new domain, [tryclj.com](http://tryclj.com) to celebrate the move.

Try Clojure is now running entirely on Heroku, and I'm really enjoying it. I haven't had to scale things up at all at this point, even though it got pounded with traffic when Heroku tweeted about the move. I kept an eye on it during that time and it never even slowed down.

Really, what won me over is the workflow. It is *soooo* easy. I mean, deployments by pushing to a git remote? That's just beautiful. I can make changes and deploy them faster than a Github employee! Finally, the fact that Phil was so eager to help me really says something about Heroku in my book. If they hire people like him, I bet the rest of their team is equally as lovely.

I look forward to a long and fruitful future with Heroku, and I thank them for their fantastic service and giving me a reason to not have to host and maintain that site on my own VPS.

[Try Clojure's](http://tryclj.com) source code is on [Github](http://github.com/Raynes/tryclojure). Check it out if you're interested in seeing a simple website that runs on Clojure, the excellent [Noir](http://webnoir.org) web framework, and Heroku.
