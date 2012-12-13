---
layout: post
title: "Moving away from Noir"
date: 2012-12-13 16:35
comments: true
categories: 
- Clojure
---

This might be a bit of a sad post for some people, but most will find that it
was simply long overdue and necessary. This post is a formal announcement of
[Noir](https://github.com/noir-clojure/noir) being more or less deprecated.

First, let's talk about what Noir is. Noir is not really a web framework. What
Noir is is a collection of libraries that most people use when developing a web
application. It was originally designed to make it extremely easy to get new and
potential Clojure developers a website they can look at and use. It served that
purpose really well.

Noir provided things like stateful session, flash, and cookie management, as
well as simple things like a little validation library and a utility for hashing
passwords and such using jcrypt. This what made Noir so popular. People wanted a
batteries included 'framework' and Noir served that purpose for them.

Under the hood, Noir uses [Compojure](https://github.com/weavejester/compojure)
for routing. On top of it, Noir has an
abstraction called 'defpage' that allows you to create routes statefully and
then call a 'start' function and have it Just Work. At the time, Compojure
generally required directly calling the jetty adapter and such to get a server
running. You never needed to do that yourself in Noir.

With defpage came some agony as people realized that while Noir's abstraction
*looks* fantastic, you have no control at all over how the routes are defined
and wrapped by middleware. Because of that, Noir was much less flexible than
Compojure. If you've ever tried to have a complex middleware setup on your
routes in Noir (or used [Friend](https://github.com/cemerick/friend) with it for that matter), you already know what
I'm talking about. It is generally impossible to do so much as wrap a specific
set of routes in middleware. This could have been added to Noir, but why bother?
For the longest time, even Chris himself said that Compojure was better for
these situations.

Given all of these realizations, I stopped using Noir quite a while ago. I
recently even moved [refheap](https://www.refheap.com) to Compojure.

A big problem that everybody has noticed recently is that Noir is no longer
moving. My buddy Chris has moved on to a bigger part of his life now, working on
Light Table, and does not have time for a lot of his old projects. Noir is used
by quite a few people, so I took on the role of maintainer of the project to the
best of my ability, but I do not know much about Noir's internals nor do I have
the time or motivation to work on it. Neither does anyone else.

Chris and I discussed this last night, and we decided that it's time to
deprecate Noir and ask that people focus on Compojure instead. The good news is
that you don't have to give much of anything up if you move to Compojure! A
while back when I started moving my own sites to Compojure, I took most of the
useful libraries that were embedded in Noir and I split them out into a new
library that you can use from compojure!
[lib-noir](https://github.com/noir-clojure/lib-noir) is the legacy of Noir. The
best thing that came out of it. It has all the useful stateful sessions,
flashes, cookies, as well as the other useful libraries. In fact, the latest
release of Noir depends on this library so if you're using it, you're already
secretly using lib-noir!

For new websites, please use Compojure and lib-noir. This is pretty much just as
batteries included as Noir itself ever was! You just have to learn how to write
routes with Compojure. It's easy and just as concise as it was in Noir. You
don't have to use ring-jetty-adapter and stuff, just use the
[lein-ring](http://github.com/weavejester/lein-ring) plugin to start your
server. Also, if you took advantage of Noir including hiccup by default, you'll
have to have an explicit dependency on it now. No biggy, right? Right!

At this point, I imagine the biggest concern most of you have is what happens to
your existing Noir website. Well, does it work? If so, you should be fine. If
you plan on doing extensive work on Noir websites in the future, I recommend
moving to Compojure as soon as possible.

I will not be maintaining Noir. If there is a serious bug or something that you
would like to fix, open a pull request and ping me on IRC or email or something
and I'll merge and release a new version. I wont be updating documentation or
anything like that anymore, and I wont be adding new features. Beyond that,
you're welcome to continue using Noir for the indefinite future, but keep in
mind that as your application ages, it'll be much harder to update and
eventually you'll end up having to fork Noir to update the versions of libraries
it uses. Better to avoid it altogether.

Understand that there is nothing *wrong* with Noir, really. We just don't think
there is a good reason to keep it around anymore. Compojure and lib-noir are in
such a state that they are a great and potentially better solution. We are
simply moving on.

I will absolutely continue maintaining lib-noir, so if there is anything you'd
like to add to it, run it by me!

Three cheers for Compojure (and lib-noir)!
