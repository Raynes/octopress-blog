---
layout: post
title: "I Code With Things and Stuff"
date: 2012-02-01 11:21
comments: true
categories:
- Clojure
- Vim
- Emacs
- Leiningen
---

A lot of radical changes in my development environment resulted in
someone asking me if I'd write a blog post about it. I was already
considering doing that, and the interest of at least one person is
motivation enough to actually do it.

## Background

I used Emacs for around two years. It started back in my Haskell days.
You know how Lispers love Emacs because it has SLIME? Haskellers seem to
love Emacs because it can handle Haskell's complex indentation rules
(rules that aren't really rules but still shouldn't be broken).

When I started using Clojure, I was lucky enough to already have some
Emacs under my belt and so I continued using it.

## Emacs

Oh how I love Emacs. When I used Emacs, I even used it as my IRC client
via ERC. ido-mode was my lord and savior and a day without it was a day
spent in the deepest bowels of hell.

When you use Emacs for a language like Clojure, you tend to get smug
towards people who use other editors (particularly Vim) because you have
SLIME, the Superior Lisp Interaction Mode Environment Blah Whatever. It
is a fantastic development environment for Lisp languages (including
Clojure) that provides debugging (I don't think this actually applies to
Clojure yet), completion, interactive development inside of Emacs, and
all sorts of other stuff that makes birds sing and babies be born.

Though I used and loved Emacs, I almost never used SLIME. Don't get me
wrong, I respect and understand why it is so important to people, I just
never really used it. If I did use it, it was because having a REPL
inside of my Emacs window was more convenient than switching to a
terminal window to enter some code. Plus, when you use SLIME (or
inferior-lisp which is a completely different and simpler thing), you
can send code from one buffer to another without copying and pasting. It
makes things really quick and easy. That's all I ever used SLIME for.

The greatest thing about Emacs is just how much you can do
with it. The fact that SLIME is possible in the first place and that it
can work out so well is spectacular. Emacs is built around a Lisp called
elisp. As such, Emacs is probably the most extensible editor that ever
has been or every will be. You can make Vim do magic tricks, but Emacs
really can pull a rabbit out of a hat.

And that's what has always bothered me. When people talk about Vim vs
Emacs they end up focusing on the least important things. You can't even
have a discussion about the merit of one or the other without a flurry
of childish remarks. Sure, Emacs is an operating system. **That's the
point.** Emacs is an execution environment for a Lisp. It is *designed*
for you to be able to do anything with it. That isn't a flaw. It is
insignificantly bigger than Vim and insignificantly more
resource-intensive because it is *supposed* to be.

So, what about important things? Key chords versus colon commands?
Sure, this one is relevant. A lot of people dismiss Emacs because of it
being driven by keyboard shortcuts off of a number of modifier keys. In
particular, a lot of people complain of "Emacs pinkie" because of the
position of the control key and the number of times an Emacs user has to
use it to get anything done. I can understand this complaint, though
I've never experienced problems related to it. As a matter of fact, I've
only had control mapped to caps lock for one year. My work sent me this
laptop with it already remapped and I decided to leave it like that. I
got used to it so it wasn't in my interest to switch it back. I'm still
not sure I've gained anything by having it moved there.

But yes, key chords vs colon commands does matter to some people and I
can understand why.

## Experimentation

I like trying new things in my development environment. New editors, new
IRC clients, new color schemes, new terminal emulators, etc. I get
bored. I recently started playing around with text editors with no real
intention of moving away from Emacs.

First of all, a friend and I have been working on
[RefHeap](https://refheap.com), a pastebin with an API. The first thing
I did was write an [Emacs plugin](https://github.com/Raynes/refheap.el) 
(library?) for it. A lot of people wont even consider a pastebin unless 
it has support for their editor, so I figured hey, I should just use as 
many popular editors as possible and write a plugin for all of them. Next 
stop: Sublime Text 2.

### Sublime Text 2

At first glance, this editor is beautiful. It has the prettiest indent
guides I've ever seen and lovely syntax highlighting. It has an
excellent set of themes including the full set of tomorrow night themes
which I used the most at the time. It is also a highly extensible
editor. Plugins are written in Python and aren't very difficult to write
if you can read an existing plugin or manage to find up-to-date
information about them.

Unfortunately, it just doesn't seem to have a very good editor. Trying
to edit Lisp code in it is so painful that it is nearly impossible. It
doesn't know how to auto-indent even remotely properly and the Clojure
syntax file that comes with ST2 breaks on regexes with escaped double
quotes. I fixed that with a guy's recommendation from twitter, but it
was a bandaid on an infected wound.

The thing that bothers me the most about Sublime Text 2 is the way you
open files with it. In Emacs and Vim, you can execute a command and type
a path to a file anywhere on the harddrive with completion and
interactive features (see ido-mode for Emacs and wildmenu for Vim). In
ST2, you have two options for opening files:

* You use the operating system's Open File dialog.
* You use the goto anything feature which lets you type a path and gives
  you fantastic fuzzy completion, but only works relative to your
  current directory, meaning you can't be in `/Users/anthony/foo` and open
  a file in `/Users/anthony/` like this.

Also, ST2's Open File dialog is completely separate from its Open Folder
function. If you want to open a folder instead of a file, you have to
use that dialog. Oh, and there is no keyboard shortcut for this. 

All in all, the operating system's open file dialog is a really weak way
for a programmer to navigate the file system to open the file he needs.
There is nothing in ST2 stopping this from being implemented AFAIK, but
I guess I'm the only one who thinks what is done now is completely
insane. Separating opening files and opening folders and forcing it to
be done via a OS dialog is just not something I'm prepared to live with.

I actually used Sublime Text 2 for about a day and a half until I really
had to edit some Clojure code. There was just nothing to be done. I like
Sublime Text 2, but it is pretty worthless for Clojure. Since RefHeap's
primary market is Clojure programmers and Sublime Text 2 users in the
community are a very, very small minority, I just decided to call it
quits on that. I will absolutely revisit this (it is really easy to
write a RefHeap plugin in Python) if ST2 ever becomes better for Clojure
and I can tolerate it long enough to test such a plugin.

I'd like to add that my opinion of ST2 is *entirely* based around its
support for Clojure. From what I saw, it is an **excellent** editor for
other languages such as Python and Ruby. This makes perfect sense
because the author and majority of contributors (to the parts that you
can contribute to) use these languages and don't use Clojure. I expect
that the Clojure portion of things will get more love as it gets more
popular in the Clojure community.

### Vim

My next stop was Vim. This is where things get interesting. VimL is the
most horrid extension language for an editor that I have had the
misfortune of working with. The thing is, Vim is a *fantastic* editor. I
don't know what the author was thinking when he designed this language,
but I can only assume it was done during a binge drinking contest.

The only salvation is that there are Vim interfaces for a number of
languages, including Scheme, Ruby, and Python. Unfortunately, you still
have to write some VimL to glue it all together, but very little. The
most important stuff can be written in one of those decent languages and
you can generally keep your sanity. Seriously, go look at gist.vim, a
plugin for Vim for the [Gist](https://gist.github.com) API. It is pure
VimL and a little over one thousand lines. No thanks.

I used Vim for a day and then I wrote that damned plugin. I dove in
head first. The most information I could find for the Vim interface to
any given language was for Python, and I was equally oblivious to all of
these languages so I went with it. The actual Python part of things was
dead simple. I spent maybe 3 hours on it (having not written a line of
Python before). Then I spent another day and a half on the VimL glue
part of things and I had a very nice and usable plugin. It was great!

At this point, I stopped caring about editors and was more concerned
with how much fun I had with Python! I don't very often just write shit
in random languages just because I can, so it was nice to do new things.
So I began having crazy thoughts to justify a rewrite in Ruby. Everybody 
loves Ruby! Nobody likes Python<sup>1</sup>! Let's do it in Ruby!

[So I did it in Ruby](https://github.com/Raynes/refheap.vim). This time, I
wrote a [whole API library](https://github.com/Raynes/rubyheap) first because I
knew I'd be using it for more than refheap.vim. That was 6 different
kinds of fun. As a bonus, I [wrote a command-line tool for RefHeap as well](https://github.com/Raynes/refh).
The actual Vim part of things was easy because the VimL glue was already written
from the Python implementation.

I had lots of fun doing this, and it took me several days to do it. I
used Vim for all of it, and that's where things got interesting. I
really liked Vim. I used Vim for about a week and a half probably 6
months before but gave up on it for Clojure development for one simple
reason: it is not as smart as Emacs about indentation.

In Emacs, when you press tab, it indents exactly how much it is supposed
to be indented. You can press tab for a month and it'll only indent to
what is **correct** for that language. Now, this varies for languages
like Haskell because there is more than one correct indentation, but
this kind of variation does not exist in Lisp and thus Clojure. Where
this really begins to show is in an example like this:

```clojure
(defn foo []
  (let [x]
    (y x)
    (z x)))
```

What happens if we want to take the `let` out of this code?

```clojure
(defn foo []

    (y x)
    (z x))
```

Uh oh. Now what? We need to somehow reindent this whole thing so that
that the two forms are lined up against the `defn` now that the `let` is
gone. In Emacs, I would have walked through this hitting tab which would
reindent each line in turn for me. But Vim isn't smart about
indentation it knows what to do when I hit return and insert a newline,
but if I hit tab some more, it'll indent some more. I didn't think Vim
had an answer to this until Chris Granger, a Clojurian vimerman, told me
the secret to life, the universe, everything.

Vim has a concept of 'text objects'. Basically, this is a way to
intelligently select an area of text based on what it is surrounded by.
This is *fantastic* for Clojure and Lisp in general because everything
is surrounded by parentheses. So, to reindent this code, we can do
`=i(`.

```clojure
(defn foo []

 (y x)
 (z x))
```

Bang! And for the line that is left, we only have to hop down a line and
hit `dd`, which deletes the line. This one single command changed my
entire opinion of Vim.

Oh yeah, and I was really attached to paredit in Emacs. [Bang!](https://github.com/emezeske/paredit.vim).
Yes, this works really really well. It has all of the most important
commands (for me, anyways) and it keeps things balanced. It isn't quite
as polished as the Emacs version, but it does work.

So, somewhere along the way, I became a Vim user. I haven't used Emacs
in at least two weeks, probably 3, and I am wildly productive in Vim. I never really
used editor commands in Emacs (because I never remembered them long
enough to use them for anything significant, which is probably because
they took longer to enter than just doing the stuff manually).

Will I continue to be a Vim user forever? I thought I'd always use
Emacs, but that turned out to not be the case. I do not want to go back
to Emacs and do not see myself doing so in the near or distant future. I
might find a reason to do so tomorrow and I might never find a reason to
do so. We'll see.

I've got one thing to add to this: I have nothing against Emacs.
Seriously guys, you don't have to hate one in order to prefer the other.
If I had a good reason to use Emacs now, I would. I just started using
Vim and realized I liked it and that I'm very productive in it. Emacs
did its job just fine and I have no animosity towards it.

## So, my development setup

This was supposed to be a post about my development environment but it
turned into a really long post about how I got here. I think that is
important. I need people to understand *why* I use the things I do
because that is wildly more important than what I actually use. So,
without further adieu, here is what I'm using for development today and
how I've got it set up.

### Software

First of all, Vim. The first thing I did when I started using Vim was
install [janus](https://github.com/carlhuda/janus). It gave me a bunch
of useful keybindings out of the box and a whole lot of useful plugins
that I probably wouldn't have found otherwise. The most useful ones I've
found so far are CtrlP, NERDCommenter, NERDTree, and Fugitive (oh em gee
this is awesome). The most useful thing I've found not included with
Janus is tabular.vim.

Also useful from Janus is that it bundles some plugins for highlighting
markdown, mustache, haml, sass, scss, better Javascript, and git
commits. Janus really is a good way to get started. You get basically
everything you need and you don't have to go hunting for how to do it.

I know what you're interested in is my Clojure setup, and I'm probably
going to disappoint you with it. I use VimClojure, and the first thing I
did was delete anything even remotely related to nailgun. Remember I
said I never really used SLIME in Emacs? I haven't even bothered trying
to do anything similar in Vim. I believe the author is rewriting the
stuff for nREPL, and I'll probably give it a go once that is done, but
I'll probably never use it much. I actually like `lein repl` and I use
it. I do *tons* of interactive development, I just don't do it in my
editor. Am I missing out? Probably. Do I care as long as I still get
things done and am productive? God no.

Besides paredit.vim, that's it for my Clojure setup. Just VimClojure's
syntax highlighting and indentation stuff and Leiningen. Clojure is
simple. Don't need much more than that.

Oh, I also use OS X. The Vim I use is MacVim and my terminal emulator is
iTerm2, simply because it is less quirky than Terminal.app.

My color scheme is solarized dark unless I'm in the sun, in which case I
go with solarized light. I'm not entirely satisfied with solarized, but
there aren't a lot of good color schemes with both light and dark
backgrounds. The color scheme I like the most and used before solarized
was Tomorrow Night. I really liked it, but there are some problems with
the Vim highlighting that I'm not entirely satisfied with, and Chris
Kempson, the author, doesn't seem to care about it anymore
and never responds to pull requests (of which there are many) and
issues. I still use a tomorrow-nightish theme on RefHeap because I can
control it, but I don't use it for my system anymore. If I had more
time, I'd just fork it and work on it myself, but it's quite a task. I
sure wish somebody would though.

Also, iTunes. I consider music a part of my development environment.
Yes, I know about the disdain for iTunes, the reason I use it over other
media players is outside the scope of this blog post.

### Hardware

I use a 2,2 macbook pro from '06 or '07. I did not pay a penny for this.
It was given to me by my job. The company I work for, Geni, gave me this
laptop. It is old, but it still works really well and the hardware isn't
really ancient. It has 3GB RAM and a 2.33GHz intel core 2 duo. The only
issues I have with it is that it is rather unsightly, has a DVI port,
doesn't the newer and better multitouch trackpad, and is big and bulky.

My ideal hardware would be a new 13-15 inch macbook pro, but I think I'd
be fine with a macbook air. In a pinch, I'd appreciate a middle class
thinkpad loaded with arch linux. Unfortunately, I am not a man of means
right now.

## Conclusion

I am currently a Vim user recently converted from Emacs. If you're new
to Clojure and you like Vim more than Emacs or are already familiar with
Vim, please ignore everybody who tells you that you *need* to use Emacs.
You will be **just fine** with Vim. I *chose* Vim after two years of
using Emacs, and I've yet to be dissatisfied. If you want to check out
Emacs, by all means, do so. Having it under your belt and having more
options is always good, but understand that it is absolutely not a
necessity for Clojure.

If you've stumbled upon this blog post trying to find the best way to
get started with Clojure, then you have found your answer. The answer is
"whatever you are comfortable with". Go install
[Leiningen](https://github.com/technomancy/leiningen) and use it instead
of trying to 'install' Clojure, and then use whatever editor you are
familiar with. If you already like Emacs, use Emacs; if you like Vim,
use Vim; even if you'd be more comfortable starting with an IDE, you are
fine. Go get Counterclockwise for Eclipse or La Clojure for IDEA. If
it begins to not be enough for you, check out the editors. There is no
wrong answer here and no silver bullet. My friend Phil Hagelberg says it
best: whatever you do, do *not* let your editor get in the way of you
learning Clojure.

Thanks for reading.

<sup>1</sup> This is a **joke**. Please take it as such. I don't dislike
Python. Python is our friend. Take a deep breath.

Thanks a lot to [Andrew Brehaut](http://brehaut.net/) for proof-reading
and advising me a bit. He made me do this, so if anything I say here
offends you, please direct all furious comments and bashing at his email
address. Thank you.
