---
layout: post
title: "Waving Our Tentacles"
date: 2011-12-02 13:27
comments: true
categories: 
- Clojure
- Tentacles
- Github
---

Everybody knows that Github is fantastic. It is a wonderful service that not only hosts your Git repositories but also gives you a great platform for managing your projects and handling large-scale collaboration. It has arguably the best code review mechanism around (the pull request) as well as project wikis, issues, organizations, and even private repositories (on paid accounts) for the non-FOSS and business folk. It is an essential tool for the open source developer.

But we're not really here to talk about Github as a service. We're here to talk about Github's new [v3](http://developer.github.com/v3/) API and my Clojure library, [tentacles](https://github.com/Raynes/tentacles), that provides a Clojure interface to it.

While writing the library, I obviously had to test things as I went along. In doing this, I realized how elegant and powerful it is to work with APIs from Clojure if you have a simple and concise library for doing so. The Github API has some really neat things in it that are really fun to play with, even if we aren't doing anything with them. This post is going to serve several purposes:

1. We're going to learn how to use Tentacles.
2. We're going to learn how to use some important parts of Github API.
3. We're going to use the Clojure REPL as a prompt for interacting with the Github API -- a very powerful concept.
4. We're going to get to see what Tentacles can do.

Note that I'll be using two accounts in my examples: `Raynes`, my own account and `amalloy`, Alan Malloy's account. He is mah buddy and agreed to have his info slathered across my blog. Furthermore, I'm going to pretty-print results with `clojure.pprint`.

## Playing along

If you want to play along, go ahead and create a new project.

    lein new tentacles-fun

Next, edit your project.clj file and add `[tentacles "0.1.1"]` to your `:dependencies`. After that, just run `lein repl` and have a blast.

## [Users](http://developer.github.com/v3/users/)

Tentacles supports the entire Github API, but we're going to start with Github's user api. It has some very interesting data that we can look at.

We can, of course, get information about a user.

``` clojure
user> (pp/pprint (users/user "amalloy"))
{:followers 30,
 :following 9,
 :gravatar_id "1c6d7ce3810fd23f0823bf1df5103cd3",
 :name "Alan Malloy",
 :avatar_url
 "https://secure.gravatar.com/avatar/1c6d7ce3810fd23f0823bf1df5103cd3?d=https://a248.e.akamai.net/assets.github.com%2Fimages%2Fgravatars%2Fgravatar-140.png",
 :bio nil,
 :location "Los Angeles, CA",
 :html_url "https://github.com/amalloy",
 :public_repos 56,
 :created_at "2010-08-18T16:37:15Z",
 :login "amalloy",
 :url "https://api.github.com/users/amalloy",
 :email nil,
 :type "User",
 :public_gists 160,
 :hireable false,
 :blog "http://hubpages.com/profile/amalloy",
 :id 368685,
 :company "Geni"}
```
