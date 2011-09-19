---
author: Raynes
date: '2010-12-03 17:54:00'
layout: post
slug: introduction-to-sexpbot
status: publish
title: Introduction to sexpbot
wordpress_id: '82'
categories:
- Clojure
---

# Introduction

[Sexpbot](http://github.com/Raynes/sexpbot) is an IRC bot written in
Clojure. He was my first serious Clojure project, and my largest project
to date.
     
    commit 583717820d07c899250bb58a69721feac33cba00
    Author: Rayne
    Date:   Sun Mar 14 09:43:59 2010 -0500
    First commit.

Since that first commit, sexpbot has undergone 10 zillion partial
rewrites, redesigns, and rethinks. In the beginning, sexpbot was an IRC
bot built on the Pircbot IRC framework written in Java. Eventually, when
I learned more about the IRC protocol itself, I wrote
[Irclj](http://github.com/Raynes/irclj) to replace pircbot in sexpbot.
That project is still active, and I've got a contributor working with
me. Sexpbot is an extremely extensible IRC bot. It has a full on plugin
framework, including a DSL for writing plugins. It can handle web
serving with compojure and jetty along with commands and a hooking
mechanism that can be used to make the bot do pretty much anything
imaginable. There are quite a few
[plugins](https://github.com/Raynes/sexpbot/tree/master/src/sexpbot/plugins)
included with sexpbot, including plugins for searching with Google,
evaluating Clojure code, evaluating Haskell code, and other things you'd
expect from any IRC bot such as maximum user tracking and checking to
see when a user was last seen by the bot. Recently, I've been working on
generalizing sexpbot to allow for multiple protocol support. I've
already added twitter support, and I plan to add things such as email
and XMPP support in the future.
# How does it work?

At it's core, sexpbot is only concerned with receiving messages from a
source, and sending messages to a source. Those are the only things that
really matters as far as communication with a protocol goes. When
sexpbot starts up, it connects to whatever it is supposed to connect to.
It constructs a callback map of functions that will be called when
certain events happen. The most important and general is the on-message
callback, but there can be protocol specific callbacks as well. For
sexpbot to recognize a plugin, a plugin has to be on the classpath, have
a namespace starting with sexpbot.plugins, and include a call to
defplugin. Sexpbot requires all of the plugins that it has been asked to
require, and loads the ones that it has been asked to load. There are
two distinct steps where sexpbot requires a plugin namespace, and then
loads the plugin by calling a function that has been defined in the
namespace by the defplugin macro. When the loading function for a plugin
is called, it adds relevant information to the 'bot' ref. This is a ref
containing a map that contains all information relating to the bot,
including the configuration, plugin, and command information. This ref,
along with a communication or 'com' map is passed around to all
commands, hooks, and everything else that needs it. When a callback is
triggered, all associated hooks are called. A hook is a function that is
called with all of the information pertaining to the callback (in the
case of on-message, stuff like who sent the message, where the message
came from, the message itself, etc), along with the com and bot refs.
Without any plugins loaded, there will only be a single hook, and this
is an on-message hook. This default hook calls an internal function
called `try-handle` that forms the basis of the bot. The try-handle
function basically just splits up the message into pieces, and checks to
see if it is a command. A normal command is any message sent to sexpbot
that starts with a configured prefix. There can be as many as you like.
In the live sexpbot (in \#clojure), the prefixes are `$` and
`sexpbot: `. When try-handle decides that a message is a proper command
issuance, the command is looked up in the bot ref. If the command is
there, it is called with the map containing all of the information that
was passed to try-handle, and then it is up to the command to do it's
work and talk to a protocol. And then we have hooks that plugins can add
themselves. When a plugin defines a hook function, it is added to the
callback list. This allows for a plugin to have the flexibility to do
absolutely anything, like get and respond to non-commands. Whenever a
callback is triggered, all hooks attached to that callback are called.
# Example

Here is a simple example plugin (taken from
[google.clj](https://github.com/Raynes/sexpbot/blob/master/src/sexpbot/plugins/google.clj)):
[code lang="clj"] (ns sexpbot.plugins.google (:use [sexpbot registry]
[clojure-http.client :only [add-query-params]] [clojure.contrib.json
:only [read-json]]) (:require [clojure-http.resourcefully :as res])
(:import org.apache.commons.lang.StringEscapeUtils)) (defn google [term]
(-\> (res/get (add-query-params
"http://ajax.googleapis.com/ajax/services/search/web" {"v" "1.0" "q"
term})) :body-seq first read-json)) (defn cull [result-set]
[(:estimatedResultCount (:cursor (:responseData result-set))) (first
(:results (:responseData result-set)))]) (defn handle-search [{:keys
[args] :as com-m}] (if-not (seq (.trim (apply str (interpose " "
args)))) (send-message com-m (str "No search term!")) (let [[res-count
res-map] (-\> (apply str (interpose " " args)) google cull) title
(:titleNoFormatting res-map) url (:url res-map)] (send-message com-m
(StringEscapeUtils/unescapeHtml (str "First out of " res-count " results
is: " title))) (send-message com-m url)))) (defplugin (:cmd "Searches
google for whatever you ask it to, and displays the first result and the
estimated number of results found." \#{"google"} \#'handle-search) (:cmd
"Searches wikipedia via google." \#{"wiki"} (fn [args] (handle-search
(assoc args :args (conj (:args args) "site:en.wikipedia.org"))))) (:cmd
"Searches encyclopediadramtica via google." \#{"ed"} (fn [args]
(handle-search (assoc args :args (conj (:args args)
"site:encyclopediadramatica.com")))))) [/code]
# Where is it?

Sexpbot runs in several channels on the Freenode IRC network, including
\#clojure, \#cake.clj, \#clojureql, \#(), and various others. It
provides services such as Clojure and Haskell code evaluation, most
importantly. You can find the source code and documentation on
[Github](http://github.com/Raynes/sexpbot) I'm starting an IRC channel
on freenode. \#sexpbot. sexpbot will be there, and I'll start collecting
users and contributors into the channel as well. If you need support,
this is the place to go.
# What is sexpbot not?

Sexpbot is not clojurebot.
# Goals

The long term goal of this project has always been to provide an
excellent IRC bot for non-coder end users. It hasn't reached that goal
yet, and might still be quite a ways off yet. I've heard of a few
Clojurians using sexpbot themselves, but it isn't fit for non-developers
yet. Going forward, documentation is going to be a big goal. We also
need to start versioning properly and actually pumping out
distributions. Configuration needs to be totally documented, so that
users know how to configure their bot. There is plenty to do, and plenty
of time to do it!
# Contributors

First and foremost, the primary contributors to this project are myself
and Alan Malloy (amalloy). There have been several contributors here and
there in the past. Here are a list of them (if I forgot you, let me know
and I'll add you):
-   Michael D. Ivey (ivey)
-   Erik Price (boredomist)
-   Pepijn de Vos (fliebel)
-   Justin Balthrop (ninjudd)
-   Curtis McEnroe (programble)

Here is to many more in the future!
