---
author: Raynes
date: '2010-11-14 19:18:00'
layout: post
slug: building-a-clojure-code-evaluating-irc-bot-with-irclj-and-clojail-2
status: publish
title: An indirect guide to getting started with Clojure
wordpress_id: '48'
categories:
- Clojure
---

This isn't a step by step guide or tutorial to getting started with
Clojure. This post is merely to help you get on the right track,
avoiding some of the more common painpoints associated with the JVM. I
want you to have the best beginner experience that you possibly can with
Clojure. If you aren't familiar with the Java ecosystem (not necessarily
through Java), coming into Clojure can be bizarre and admittedly
painful. You're going to need to do things that you aren't used to and
don't necessarily see the need for or understand. I assure you that it
will all become clearer with time. The biggest mistake you can make is
to have assumptions about how things work when you come into Clojure.
You're going to expect to go to clojure.org and be greeted with
installers and detailed spoonfeeding instructions for getting set up.
You aren't going to find this. Not that Clojure doesn't have some
[official beginner
material](http://www.assembla.com/wiki/show/clojure/Getting_Started).
But it isn't extremely abundant at this point. This post is meant to
complement that material with a "Hey, this isn't totally insane, and
we're not trying to screw with you on purpose." One of the primary
things that freak new people out is the lack of installers, as I
mentioned above. They're used to finding a new language, going to the
website, installing it, and using it from a single place within their
system. It's dead simple to install a C++ compiler, Ruby, Python, etc.
That sort of thing isn't really what you do in Clojure. Instead, you'd
probably use a build tool. The reason this is so is because the Java
ecosystem is centered around 'jars'. jars are just zip files with a
clever extension that is used to package code (libraries and
applications) along with some metadata that Java can use to do things
like execute code within the jar and other fun stuff. In order for the
code inside of jars (or anywhere, really) to be available to **your**
code, that code (inside of jars or directories or whatever) has to be on
the Java classpath. Now, the classpath is a necessary evil that if
you're forced to manage directly as a beginner, is going to drive you
insane. The Java classpath is usually set when you call the 'java'
application on the command-line. You can set it with the -cp option.
Now, if you're a special kind of fellow, you might find the whole
classpath thing simple and easy to manage. I didn't, and most people
won't. It gets even more complicated when it comes down to projects,
managing the jars that those projects depend on (versions, o versions),
and packaging your own code in jars and distributing your code (and
jars) to the world. This is where build tooling comes in. Cljr
[http://github.com/liebke/cljr](http://github.com/liebke/cljr) I'm going
to start with cljr because it is probably the one that is going to make
the most sense to you as a beginner. This is the one thing that I'm
going to talk about that *isn't* a build tool. Cljr aims to be an easy
way to launch an [REPL](http://en.wikipedia.org/wiki/REPL) and a
package/dependency manager for that REPL. Installing cljr is the closest
thing you're going to get to 'installing Clojure'. When you install
cljr, you'll get a neat little command line tool that you can use to
start an REPL and set up dependencies ("packages") for that REPL. Cljr
is absolutely excellent for when you want a quick and easy REPL. It's
easy to install and it shouldn't eat your laundry at all. I'd recommend
cljr as the first thing for a beginner to install to try out Clojure
with, but when it comes time for your first real 'project' and you need
to move beyond the REPL and into build tooling land, cljr will not
really suffice. So, lets move on. Cake
[http://github.com/ninjudd/cake](http://github.com/ninjudd/cake) Cake is
an honest-to-God build tool. It's almost totally written in Clojure, but
there is a small piece of it that is written in Ruby. The reason for
this is so that the JVMs can be persistent. When cake starts up a JVM,
it keeps the JVM running for as long as possible (there are times when
it needs to be restarted because certain things have changed in your
project, such as the classpath) in order to eliminate JVM startup time,
which tends to be rather significant. It uses Ruby as a way to bootstrap
itself and control the processes it creates. It uses Ruby where
Leiningen (we'll talk about this soon) would use bash. I doubt you're
interested in this at the moment though, so lets move on. Cake is also
very easy to install. If you're familiar with Ruby, you'll be very
excited to find that you can install it as a gem! There are two other
ways to install it as well. Check out the
[README](http://github.com/ninjudd/cake#readme) on the github page for
more information. So, what will cake give you? Everything you'll need as
a hardcore Clojure developer. Besides offering a lot of the same "REPL
and package manager for that REPL" in a box features that cljr provides
(via the global project, described in the README), albeit not quite as
intuitively at the moment, it also provides you with the ability to
easily create and manage Clojure projects, manage their dependencies,
build them, distribute them, work with them, and anything in between.
Cake offers a dependency based task model (much like the Rake build
tool) and can be extended via tasks and plugins to do anything you could
ever imagine or want. Cake tasks are usually defined with the 'deftask'
macro, in which dependencies on other tasks can be specified. Plugins
are just collections of tasks that can be made available to cake. Cake
uses Maven (soon Ivy, with Maven as an option) as it's backend for
dependency management. All of the jars that you depend on are copied
over to the lib/ directory in your project and put on the classpath for
you when you start the repl, a swank server, a rally for sanity, etc. If
you're ready to write your first project, or would rather go with a real
build tool rather than cljr, cake is for you. Leiningen
[http://github.com/technomancy/leiningen](http://github.com/technomancy/leiningen)
Leiningen fills mostly the same space as cake does. It's another Clojure
build tool. It's older than cake and currently more popular, so if I was
being appropriate, it probably should have come first. I'm a huge cake
fan and am totally biased, so sue me. ;) Most people were using ant and
maven or \*gasp\* no build tooling at all until Leiningen came along and
saved the day. It innovated the Clojure build tooling scene, making it
easy to manage Clojure projects and their dependencies. One owes
Leiningen much respect every time they create an executable jar using
the 'lein uberjar' or 'cake uberjar'^1^ commands. Leiningen too can be
extended with plugins. However, the task system that Leiningen provides
is not dependency based. To create a new task that can be executed with
"lein foo", one defines a function called foo in the namespace
leiningen.foo. All of the built-in leiningen tasks are ordinary Clojure
functions too and can be called from the plugin. The return value is the
exit code, and the docstring is displayed in "lein help". Leiningen also
has a hooking mechanism called robert-hooke (ol' Phil sure is a sucker
for queer names), which lets you modify and extend the behaviour of the
built-in tasks. Whether a dependency based system or plain ol' functions
is the best solution depends on who you're talking to and one's own
personal opinion. Leiningen also uses maven as it's backend for
dependency management. It works in mostly the same way as cake at this
point. While Leiningen does have a command for starting an REPL outside
of a project (a la cake, cljr), it does not currently have a way for you
to manage dependencies for that "outside-of-a-project REPL" like cljr
and cake does. If you want something like that, you'll have to look
elsewhere for now (or, add it to Leiningen! Might be a fun first project
for you)^2^. The project.clj build file format that Leiningen, Cake, and
[Polyglot Maven](http://polyglot.sonatype.org/clojure.html) all use was
invented by Leiningen. One of the greatest things about all of these
build tools is the fact that you don't have to write your build files in
XML like you would be forced to do with straight maven or ant. In any
case, Leiningen is most definitely a great choice for a build
tool/dependency management system. Leiningen, just like cake, makes it
very easy to manage your projects. There are, of course, other build
tools and dependency managers (maven/polyglot maven, ivy, ant, gradle,
etc) from Javaland that can be used with Clojure. I've given you the
rundown of the entirely Clojure-based ones. It isn't difficult at all to
'install' Clojure. You just have to reconsider what you mean by
'install'. There will soon be a 'clj' launcher script included with
Clojure releases, and this will make it easier for people to get their
REPL running for the first time and immediately get to playing with
Clojure. The biggest problem that people have with starting with Clojure
is that they simply don't understand the ecosystem and how it works, but
that isn't necessarily unexpected. The Java/Clojure ecosystem can
definitely seem bizarre to outsiders who haven't experienced it. Clojure
is still very young, and these sort of kinks will work themselves out in
time. Things will become more standardized and simple. I hope this post
can help you get started without serious pain and blood, and that you'll
have a high regard of Clojure when it's all said in done. Such an
amazing language should not be overshadowed by difficulty getting
started.
~1:\\ Okay,\\ so\\ this\\ was\\ actually\\ a\\ maven\\ invention.\\ Nonetheless...~
~2:\\ I've\\ been\\ told\\ that\\ there\\ is\\ talk\\ about\\ implementing\\ this\\ sort\\ of\\ thing\\ in\\ Leiningen\\ already.\\ Look\\ for\\ it\\ in\\ the\\ future,\\ and\\ get\\ it\\ faster\\ by\\ helping\\ out!~
Feedback is welcome. If there is anything that you think I should add,
mention, or correct, please let me know. Also, thanks to raek from
\#clojure for helping me out with the Leiningen section. He helped me me
be less totally wrong and biased. :D!
