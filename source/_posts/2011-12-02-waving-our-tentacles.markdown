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

Next, edit your project.clj file and add `[tentacles "0.1.2"]` to your `:dependencies`. After that, just run `lein repl` and have a blast.

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

You might note that Alan's bio is nil. That's because he has no life. Robot and all that.

Next, let's learn a thing or two about ourselves. First, we're going to need our authentication information quite a bit, so lets go ahead and def it so we don't have to type our password 10 thousand times.

```clojure
user> (def auth "Raynes:REDACTED")
#'user/auth
```

Truthfully, I just wanted to only have to do that once because I have a habit of forgetting to remove my password during demonstrations like this.

```clojure
user> (pp/pprint (users/me {:auth auth}))
{:disk_usage 46932,
 :collaborators 0,
 :plan
 {:private_repos 0, :collaborators 0, :space 307200, :name "free"},
 :followers 85,
 :following 36,
 :gravatar_id "54222b6321f0504e0a312c24e97adfc1",
 :name "Anthony Grimes",
 :avatar_url
 "https://secure.gravatar.com/avatar/54222b6321f0504e0a312c24e97adfc1?d=https://a248.e.akamai.net/assets.github.com%2Fimages%2Fgravatars%2Fgravatar-140.png",
 :bio
 "My name is Anthony Grimes. I live in a small town called Eldridge in Alabama. I am a programmer and a very avid supporter of functional programming and Clojure. I am currently writing a book on Clojure called [Meet Clojure](http://meetclj.raynes.me). I'm the author (or co-author) of a number of Clojure projects including [Try Clojure](http://tryclj.com) and [clojail](https://github.com/flatland/clojail), having spoken about the latter at Clojure Conj 2011. I am currently employed by [Geni](http://geni.com) as a paid intern working with Clojure on the backend.",
 :location "USA, AL",
 :html_url "https://github.com/Raynes",
 :owned_private_repos 0,
 :public_repos 58,
 :created_at "2009-02-14T03:25:23Z",
 :private_gists 137,
 :login "Raynes",
 :url "https://api.github.com/users/Raynes",
 :email "i@raynes.me",
 :type "User",
 :public_gists 457,
 :total_private_repos 0,
 :hireable false,
 :blog "http://blog.raynes.me",
 :id 54435,
 :company "Geni"}
nil
```

As you can see, I definitely have a life.

So, what's up with how we gave tentacles our authentication info? We passed it a map containing an `:auth` key. This is an idiom in tentacles. The Github API has a lot of API calls that can have *required* as well as *optional* arguments. Most of the POST api calls take a JSON hash of data (but not all of them). Tentacles represents this in a very simplistic way. Any API call that takes optional arguments or does something special when given auth info translates to a function whose last argument is an optional map. Any API call that requires authentication, regardless of whether or not it has optional arguments, must take an options map (even if it will only ever contain the auth info). Furthermore, any **required** arguments that an API call takes are required positional arguments to the equivalent tentacles function.

Okay, so I have this theory. I am pretty certain that I am much more popular than Alan. I'm nicer, arguably smarter, definitely better looking, and I certainly wear better clothes. Can the Github API tell me if I'm correct? You bet it can! The obvious and canonical measure of epeen size is follower count! Let's tally 'em up!

```clojure
user> (pp/pprint (users/followers "amalloy"))
[{:login "MayDaniel",
  :avatar_url
  "https://secure.gravatar.com/avatar/e917432c8e85f043647b2c57b443c56f?d=https://a248.e.akamai.net/assets.github.com%2Fimages%2Fgravatars%2Fgravatar-140.png",
  :url "https://api.github.com/users/MayDaniel",
  :gravatar_id "e917432c8e85f043647b2c57b443c56f",
  :id 188450}
 {:login "Raynes",
  :avatar_url
  "https://secure.gravatar.com/avatar/54222b6321f0504e0a312c24e97adfc1?d=https://a248.e.akamai.net/assets.github.com%2Fimages%2Fgravatars%2Fgravatar-140.png",
  :url "https://api.github.com/users/Raynes",
  :gravatar_id "54222b6321f0504e0a312c24e97adfc1",
  :id 54435}
 {:login "tonyl",
  :avatar_url
  "https://secure.gravatar.com/avatar/43b2ca65e7a2cadf849adf103e6c066d?d=https://a248.e.akamai.net/assets.github.com%2Fimages%2Fgravatars%2Fgravatar-140.png",
  :url "https://api.github.com/users/tonyl",
  :gravatar_id "43b2ca65e7a2cadf849adf103e6c066d",
  :id 31147}
  ...]
```

Okay, so that isn't very helpful. Github gives us very verbose information about followers and it gives us *a lot* of info. Let's trim that down a bit.

```clojure
user> (pp/pprint (map :login (users/followers "amalloy")))
("MayDaniel"
 "Raynes"
 "tonyl"
 "jColeChanged"
 "hoggarth"
 "ossareh"
 "stonegao"
 "ohpauleez"
 "raek"
 "segfaulthunter"
 "invaliduser"
 "gigasquid"
 "Clinteger"
 "odekopoon"
 "devn"
 "dbyrne"
 "imissmyjuno"
 "k4rtk"
 "xmlblog"
 "jeremyheiler"
 "amcnamara"
 "sconover"
 "srid"
 "mbacarella"
 "carlwarnick"
 "spoon16"
 "octopusgrabbus"
 "daviddavis"
 "andreisavu"
 "RafalBabinicz")
nil
```

That's better. Now count them.

```clojure
user> (count (users/followers "amalloy"))
30
```

Huh? That's no good! He definitely has more followers than that. What's going on here is that Github paginates results. By default, there are 30 items per page of results and the maximum number of results per page is 100. Let's go ahead and set it to 100.

```clojure
user> (count (users/followers "amalloy" {:per-page 100}))
30
```

Oh... He really does only have 30 followers. Well then. Moving on.

```clojure
user> (count (users/followers "Raynes" {:per-page 100}))
85
```

I think I've won this one. It's the hair that does it, I think. There is also a specialized call to get the currently authenticated user's followers, so we could have used it instead:

```clojure
user> (count (users/my-followers {:auth auth :per-page 100}))
85
```

Any way you skin it, I'm better looking than Alan. But who is the better person? Who is the most... outgoing? Let's find out!

```clojure
user> (count (users/following "amalloy" {:per-page 100}))
9
```

9... What about me?

```clojure
user> (count (users/following "Raynes" {:per-page 100}))
36
```

This just isn't looking good for ol' Alan. That's enough of that, before I hurt his feelings. 

This isn't all you can do with the user API. Check out the [docs](http://raynes.github.com/tentacles/#tentacles.users) if you want to see the rest.

## [Repos](http://developer.github.com/v3/repos/)

So far, we've found out that I am better looking than Alan, more popular, and that I am a better person. But that isn't all that matters! Who is the most hardcore coder? Tentacles tentacles on the octocat, who of us two does not code trash?

Our first measure will be a repository count. If I have more repositories than he does, then obviously I love coding more than him. Github has a dedicated API call for getting the authenticated user's repositories, but we don't want to use that, since it would also list private repos. While I don't actually have any of those, I don't want to be accused of skewing these numbers. This is srs bsns! Nonetheless, tentacles has a function for that called `repos`, just in case you need it (for cheating).

```clojure
user> (require '[tentacles.repos :as repos])
nil
user> (count (repos/user-repos "Raynes" {:per-page 100}))
58
```

Wow! I have 58 repositories! Let's see how many of those are forks.

```clojure
user> (count (filter :fork (repos/user-repos "Raynes" {:per-page 100})))
27
```

Wow! 27. I'm a forkaholic. Now for Alan.

```clojure
user> (count (repos/user-repos "amalloy" {:per-page 100}))
56
```

Ooooooh, it's close. It all comes down to the forks now.

```clojure
user> (count (filter :fork (repos/user-repos "amalloy" {:per-page 100})))
38
```

HAH! Now, I'm not good at math, but I'm pretty sure that means I have more original repositories than he does. Cause that's how I roll. Obviously, given these numbers, I love this craft waaaaaaay more than Alan does.

But who has the most popular code? I say we compare the number of watchers on our most popular projects.

```clojure
user> (count (repos/watchers "4clojure" "4clojure" {:per-page 100}))
100
```

I highly doubt that that is an even 100. It probably just cut off at 100 because that's our max. Let's check the next page.

```clojure
user> (count (repos/watchers "4clojure" "4clojure" {:per-page 100 :page 2}))
27
```

127 watchers! What about my biggest project?

```clojure
user> (count (repos/watchers "flatland" "lazybot" {:per-page 100}))
53
```

Oh noes! Maybe that isn't my biggest one.

```clojure
user> (count (repos/watchers "Raynes" "tryclojure" {:per-page 100}))
57
```

._. I concede. He wins this one. Alan obviously has the most popular projects. You can't win 'em all.

Just for fun, let's look at language stats:

```clojure
user> (pp/pprint (repos/languages "Raynes" "tryclojure"))
{:Clojure 7419, :JavaScript 4471}
nil
user> (pp/pprint (repos/languages "4clojure" "4clojure"))
{:Shell 527, :Clojure 159307, :JavaScript 106110}
nil
```

We've both employed some tricks to make it so Github doesn't think our projects are Javascript projects. I don't store my JS dependencies in the repo and Alan puts them in a directory that Github doesn't care about. We're both clever, in any case.

This doesn't even touch what the repo API is capable of. With Github's v3 API, even download uploads are supported! Check out the full API in the marginalia [docs](http://raynes.github.com/tentacles/#tentacles.repos)

## [Issues](http://developer.github.com/v3/issues/)

I wonder who has the worst code? I bet that if we check the number of issues on our top projects, we'll be able to find this out.

```clojure
user> (count (issues/issues "Raynes" "tryclojure" {:per-page 100}))
0
```

Bang! No open issues. 'Cause that's how I roll. Let's check the closed ones as well.

```clojure
user> (count (issues/issues "Raynes" "tryclojure" {:per-page 100 :state "closed"}))
20
```

So a total of 20 issues. What about 4Clojure?

```clojure
user> (count (issues/issues "4clojure" "4clojure" {:per-page 100}))
29
```

Ahahaha! They've got more issues *open* than I have open and closed combined.

```clojure
user> (count (issues/issues "4clojure" "4clojure" {:per-page 100 :state "closed"}))
100
```

Doesn't even fit on one page!

```clojure
user> (count (issues/issues "4clojure" "4clojure" {:per-page 100 :state "closed" :page 2}))
59
```

159 closed issues! Wow! That must be some terrible code. I feel sad for the little fellow.

As with the rest of our examples, this is only the tip of the iceberg. Check out the Marginalia [docs](http://raynes.github.com/tentacles/#tentacles.issues) for more.

## [Gists](http://developer.github.com/v3/gists/)

Finally. One final competition. Gists. The way I see it, if I have more gists than Alan does, then I'm... uh... I'm just a damned better person. I don't need a reason.

```clojure
user> (require '[tentacles.gists :as gists])
nil
user> (count (gists/user-gists "amalloy" {:per-page 100}))
100
user> (count (gists/user-gists "amalloy" {:per-page 100 :page 2}))
60
```

160 gists. Not bad. I bet I can do better.

```clojure
user> (count (gists/user-gists "Raynes" {:per-page 100}))
100
user> (count (gists/user-gists "Raynes" {:per-page 100 :page 2}))
100
user> (count (gists/user-gists "Raynes" {:per-page 100 :page 3}))
100
user> (count (gists/user-gists "Raynes" {:per-page 100 :page 4}))
100
user> (count (gists/user-gists "Raynes" {:per-page 100 :page 5}))
57
```

457 public gists! PUBLIC ONES! I am obviously, for reasons I am not certain of, the better person.

The Gist API is extensive and complete. It even includes a way to create gists now! Check out the rest in the Marginalia [docs](http://raynes.github.com/tentacles/#tentacles.gists).

## Conclusion

I'm better than Alan.

## Real Conclusion

The Github API is awesome. Clojure is an awesome language for working with APIs. Working with APIs interactively is fun. Tentacles is fun. I like Tentacles. Tentacles is cool.

Thus concludes our demonstration. This post only just touches the capabilities of the Github v3 API. Tentacles supports all of it. Check out the Marginalia [docs](http://raynes.github.com/tentacles/) and the Github API [docs](http://developer.github.com/v3/) for more awesome.

Finally, these numbers/stats are obviously meaningless, so don't look into them too much. It's all in good fun.
