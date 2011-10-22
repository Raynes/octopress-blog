---
layout: post
title: "Problems With Clojure + Homebrew?"
date: 2011-10-22 16:53
comments: true
categories: 
- Clojure
---

There is a fix-all solution to any problems you're having with Clojure + Homebrew.

   $ brew uninstall clojure
Followed by
   $ brew install cake
Or
   $ brew install leiningen

In all seriousness, if you've installed Clojure via *any* package manager and you're having problems, that's because it doesn't make any sense whatsoever to install Clojure through a package manager. The JVM ecosystem just doesn't work that way. Clojure is essentially a library -- it's stored in a jar.

Instead, you want to install a build tool. [Cake](http://github.com/flatland/cake) or [Leiningen](http://github.com/technomancy/leiningen) are Clojure build tools. They are your 'interface' to Clojure. They handle all the classpath nonsense, project management, classpath management, REPL management, compilation, and everything else that you shouldn't need to or try to do on your own.

Do **not** try to 'install' Clojure. Install a build tool instead. It'll prevent 98% of all beginner issues related to classpaths and make it easier for people in the Clojure mailing list and IRC channel to help you with real problems. Furthermore, they make it really easy to manage everything from small scripts to large projects, and can even give you properly classpath repls as easy as `cake repl` and `lein repl`.
