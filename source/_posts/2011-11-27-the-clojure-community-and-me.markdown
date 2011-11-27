---
layout: post
title: The Clojure Community and Me
date: 2011-11-27 08:25
comments: true
categories: 
- Clojure
- Clojure Conj
---

## Origins

When I was around 13 years old, I played a game called Diablo II. You've probably heard of it or played it. It was a fun little RPG that had a huge community of hackers surrounding it. I would often visit the blizzhacker's IRC channel and chat with the community of programmers, or just listen in awe as they talked about amazing things that made no sense to me and left me baffled at how anyone could understand it. One person in particular was a Haskell programmer. He was a very smart fellow that I looked up to and whose opinion I valued. Some of these people were C#, C, and even D programmers. This was my first introduction to programming.

I decided that this was something I was interested in and would like to do, so I spent the next couple of months beating my head against Python, C#, Boo, and a number of other languages that I probably played with and simply don't remember. Nothing really made much sense during that time. Python was the closest I got to things clicking. I had a lot of time to do these things because I've always been home-schooled.

I always say that Haskell was my first language. That is not entirely true. I played with many languages in the beginning. Haskell was, however, my first *real* introduction to programming. It was the first language that made sense. It was through Haskell that I was able to make sense of programming as a whole, including Object Oriented programming.

A lot of people wonder if learning Haskell, a purely functional language, as a first language would make OOP languages be as difficult to learn and understand as functional languages are to OOP developers. My answer, given my experiences, is no. Learning a functional language first gave me the programming foundations that I needed to understand OOP and how it compared to other paradigms. Basically, Haskell told me "Hey, this is programming. This is what it means and this is what you do." and then Object Orientation was just another layer on top of that. Another way of doing things. Programming is simple, but OOP clouds that and makes it look unnecessarily complex. If I had to teach someone to program, I think I'd start with a functional language given my experiences.

## Enter Clojure

So what comes after Haskell? After I had used Haskell for a couple of months and had got a few projects under my belt, it was time for a new step. I felt that my next logical step was a Lisp. Clojure was new and getting a lot of attention. It was very practical, had an amazing set of minds behind it, and a very welcoming tight-knit community. 

I didn't do a whole lot with Clojure at first. I spent a lot of time just learning the language and not actually doing anything. My first serious Clojure project was [lazybot ](https://github.com/flatland/lazybot) (originally known as sexpbot), an extensible IRC bot. It was a great project that I still use and actively maintain.

Originally, lazybot was a Pircbot. While Pircbot is an excellent Java library and is easy enough to use from Clojure, I really wanted to have a Clojure IRC library to use for it. That was my next project, [Irclj](https://github.com/flatland/irclj). Irclj is an asynchronous (a word that I didn't understand at the time of writing it) IRC protocol library that I am currently in the process of rewriting. Nonetheless, it was my first introduction to lower-level network programming and the first time I'd ever done anything with any internet protocol. You can find lazybot in its current incarnation in the #clojure IRC channel where he provides services such as Clojure and Haskell code evaluation.

That project was my gateway into the world of serious programming. I spent a lot of the next year working on lazybot and writing plugins for it. I had some other projects here and there, but lazybot was the one that mattered the most to me.

## Things People Enjoy

If I'm known for anything at this point, then I'm known for [Try Clojure](http://tryclj.com). It is a website that, like [TryRuby](http://tryruby.com), is designed to introduce people to the Clojure language very quickly by providing an interactive tutorial and an in-browser REPL that evaluates code **on the server side**. This was accomplished originally using Heinz N. Gies's clj-sandbox Clojure sandboxing library and eventually one that my best friend Alan Malloy and I  wrote later, called [Clojail](https://github.com/flatland/clojail).

## How To Get Famous

Clojure Conj 2010 brought a lot of firsts for me. It was my first trip outside of the state of Alabama where I was born, raised, and still live; my first time flying in an airplane; my first conference; and the first time that I felt that I was a real member of a community. It was, in general, a huge leap in my life. The last two years have seen me grow the courage to consider myself a 'real' programmer, and begin programming professionally.

But it wasn't really just the conference that did these things for me. It was the months leading up to the conference.

I wasn't going. I was 16 and I didn't have a job. My family is not particularly poor, but with the bulk of our income coming from disability checks, conferences were just not really something that could ever possibly happen. But I was okay. I mean, I was sad that I'd miss such a huge event, but I never expected to be able to go in the first place so the realization that it wasn't happening was easy to take.

{% img left http://1.gravatar.com/avatar/1dd69a4fb995d41e4f5d3c9e786d2d64?size=420 Chas Emerick %}
I'm not sure exactly how it happened, but I think somebody in the #clojure IRC channel asked me if I was going to the conference. Either that or I mentioned it for some reason. [Chas Emerick](http://cemerick.com/) happened to be watching. He immediately messaged me privately and asked me why I wasn't going to be able to make it. I told him that I simply did not have the funds to travel to North Carolina, pay for lodging, and register at the conference. Furthermore, it wouldn't be just me -- my mother would need to travel with me (for obvious reasons). He said, in so many words, "well, let's see what we can do".

Not much time passed before Chas told me [Relevance](http://thinkrelevance.com/) had happily agreed to cover my stay at the hotel and waive the conference registration fee. I was floored. Chas decided that the best way to get the rest of the funds for air travel and miscellaneous travel expenses would be to hold a fund-raiser. He [wrote a blog post](http://cemerick.com/2010/09/10/a-clojure-scholarship-lets-send-raynes-to-the-conj/), set the goal for $1000, and threw up a paypal donate button. And so it began. 

Oh how the twitterverse rooted for Anthony Grimes on that day. Hacker News, Reddit, IRC. Even people outside of the Clojure community itself were donating for my cause. A whopping 72 minutes later, our goal had been reached (and slightly exceeded). 72 minutes is what it took for the Clojure community and others to raise $1000 for a 16 year old kid to be able to attend his first conference. Chas bought my mother and I our plane tickets and I was on my way.

This would spark what is now becoming a tradition. For this last conference, Chas Emerick once again took it upon himself to [help](http://cemerick.com/2011/09/20/2011-clojure-scholarship-help-send-ambrose-to-the-conj/) somebody get to the conference. Ambrose Bonnaire-Sergeant had to travel much farther than me and thus his expenses were much greater. The Clojure community once again rose to the task and met the goal.

## Clojure Conj 2010

Oh wow. What a whirlwind. I honestly only remember the very best parts of the whole trip. It's all just a big bright and colorful blur.

I arrived at the hotel and walked into the lobby. How appropriate was it that the first person I met and talked to was Chas Emerick, sitting right there in the lobby with his laptop as if he were waiting on me. I resisted the urge to hug and kiss him and instead opted to shake his hand.

When I checked in to get my room key, the woman at the counter said, and I quote, "It's about time! Everybody has been asking if you had arrived.". That certainly blew my mind.

The next two people I met, also appropriately, were Lance Bradley and Justin Balthrop, two of my very best friends and people who would eventually be my co-workers. I didn't even get to sit down once I got to the room, they already had me walking with them to a restaurant. We had fun that night.

The next day was even crazier than the last. Everyone who saw me wanted to shake my hand and talk to me. People would ask me about my experiences and praise me for my projects. I felt so much like a celebrity. Even Rich Hickey himself would speak to me and shake my hand later on.

The conference was wonderful. The talks were great, but the social aspect of it is what truly made it the most important experience of my life. Meeting everybody for the first time. The energy that emanated from everyone pulsed so hard that it could almost make your teeth rattle.

## 2011

The next year was a blast for me. A few months  after the conference, I was hired as an intern at [Geni](http://geni.com), working remotely under Justin Balthrop and Lance Bradley, two close friends, on the site's backend with Clojure. With Clojure! My first job is developing Clojure! Winning! I quickly learned that making money is wonderful and that Macbook Pros are the best things since sliced bread.

At about the same time I got my job, I also landed a book deal with [No Starch Press](http://nostarch.com). I'm currently writing a book called [Meet Clojure](http://meetclj.raynes.me) for them.

## Clojure Conj 2011

The moment Clojure Conj 2011 was announced, I was plotting my way there. Clojure conferences are wildly addictive. Worse than heroin. But hey, I was making some money now. But hey, this conference is going to cost even more than the last. But hey, maybe Geni will pay for some of it? But hey, I'm 17, so liability is a big complicated legal bitch. Mostly, I knew I would get there *some how*, but I hadn't quite worked out exactly how. If I had to, I would just save my paychecks for a month or so and not spend very much at all. This would have worked out, since I still live at home and have the privilege of being fed by a lovely woman with excellent shopping and frying pan foo. But hey, it didn't have to work out.

### Speaking

I vowed that I would not submit a talk for Clojure Conj 2011. I was utterly certain that I would not be able to perform a 40 minute long presentation in front of the entire Clojure community and be able to keep them interested while doing it.

But damn them all to hell, Justin, Lance, **and** Alan decided that they would submit talks. Every single Clojure person at Geni was submitting a talk except for me. I conceded, assuming that there would be no earthly chance of my talk getting chosen, and submitted a talk about JVM sandboxing and Clojail.

So, my talk got accepted. As soon as I cleaned out my undergarments and finished receiving congratulations from my friends, I went and cleaned them out once more. This *was* great though. It meant that Relevance would pay for my room (which also meant my mother's room), waive the conference registration fee, and even give me a travel stipend (once I arrived) that would cover my own plane ticket. That dropped the total costs of the conference (disregarding miscellaneous expenses) to around $250. Winning!

### Actually Doing It

But shit, now I have to compose and perform a talk. For 40 minutes. In front of Rich Hickey and everyone else that I am intimidated by and who is exceedingly smarter than I am.

So, I did what any person this terrified would do. I forgot it happened and waiting until about two months before the conference before I even thought about writing the talk. I ended up doing the majority of it over about two weeks because Justin had given me the ultimatum that I would either practice it live with my co-workers over Skype or not get paid. ;)

It went surprisingly well over Skype. They gave me insanely awesome feedback and  a lot of encouragement. The Clojail talk was the product of not only my blood sweat and tears, but those guys as well. That they convinced and (half-serious) forced me to practice the talk was the best thing that could happen to me. It showed me how nervous I would be at the Conj and taught me how to control it.

### The Conference

I was once again greeted with a healthy round of handshakes after my arrival at the venue. Early registration was great. I met Carin Meier, a person whom I had worked with on [4Clojure](http://4clojure.org). A handshake would not do for her, no, she wanted a hug. That was when I really felt that things would be okay and that, regardless of what happened with my talk, everything would work out in the end.

And it did. After meeting my best friend in person for the first time, Alan Malloy; after Carin Meier's big ol' bear hug; after letting the atmosphere of Clojure Conj 2011 and the Clojure community once again sink in and take hold of me, I felt so good that I didn't bother practicing my talk again before I performed it. I was okay.

My talk went great. It was well received and people praised me for it for the remainder of the conference. Either immediately after or during my talk, Rich Hickey [tweeted](https://twitter.com/#!/richhickey/status/134672231140835328) (a rare occurrence) the following: "@IORayne is awesome, killer job on library and talk! #clojure_conj". And if that wasn't enough to make me piss myself in glee, he approached me right after my talk, shook my hand, and said the equivalent in person. This man, the public speaker of the decade, praised me on my speaking abilities. Holy. Shit.

After that, I did a brief interview with Chas for his podcast [Mostly Lazy](http://mostlylazy.com/), and a slightly longer interview with [Runtime Expectations](http://codebassradio.net/shows/runtime-expectations/) of Codebass radio. Both should be made public soon.

Oh man, Clojure Conj 2011. I've never attended a non-Clojure Conj conference before, but surely this is as good as it gets, right? I can not think of a single thing that I did not like about the whole experience. Relevance knows how to put on a conference.

## Conclusion

I'm not really sure why I wrote this. I suppose I did it for two reasons:

1. At least 10 thousand people asked me about my origins at the Conj. Wondering how I got into programming and such.
2. I can not say enough good things about the Clojure community. Sometimes I just need to tell people about it, and I can't think of a better way to do that than by pointing out that **my entire life** has been built around it.

So, that's me. Where I came from, what I've done, and more importantly, what the Clojure community has done for me. I owe so much to all of you that I can't really express my thanks in words. Instead, I hope that my contributions to the world will be enough. My motivation is to make you proud.

### Future

It's nice to have one of these.

I don't really know what the future will hold. At the very least, more projects will always be forthcoming. I want to make nice things. Furthermore, I do currently have a job, albeit an internship (a paid one), with Geni. I'd like to work with them full time at some point after I turn 18 and graduate high school. This would obviously involve extended stays in California and most likely a move, even if not immediately. That's all some pretty serious stuff that I nor my co-workers are taking lightly. They are very supportive of me to the point that they allow me to work remotely, even though I am the only one who does so on a regular basis. This means that they have to actively put effort forth to keep me in the loop about things, since I'm not there to hear their conversations. I've never once saw them complain about it.

So, I think my future is bright. I'm happy that I'm busy and I have a lot of great things ahead of me. I look forward to the future and whatever it may bring.

Thanks for reading.

-Anthony

*P.S. Many thanks to Dominik Picheta and Ricky Elrod for reading this for me and making sure I'm not insane.*
