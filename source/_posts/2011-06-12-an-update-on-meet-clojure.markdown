---
author: Raynes
date: '2011-06-12 06:20:28'
layout: post
slug: an-update-on-meet-clojure
status: publish
title: An Update on Meet Clojure
wordpress_id: '153'
categories:
- Clojure
---

Introduction Recently, I announced that my book, Meet Clojure, was
officially being published. This is true. Contract is signed, sealed,
and delivered. It even has its very own
[website](http://meetclj.raynes.me). The contract states that our
initial soft deadline is November. I have no clue when it will actually
be finished or when it will be in print, but I hope to have something
that I can bring to the Conj with me. Early release copies or something,
but it almost certainly won't be in print by then. I'll do something!
I'm very excited that people are excited about the book, and I haven't
updated on my progress in a while, so that's the purpose of this post.
First of all, there hasn't been much progress up to this point. I
recently started a job writing Clojure at [Geni](http://geni.com), so I
took a little time off from the book to focus on that. This is my very
first job in the world, and it is very important to me that I am able to
give them my best and still be able to do this book. I know I can. I've
got a rhythm now, and I'm starting, slowly but surely, to continue work
on the book. I want my progress to be an open thing. Writing a book is
hard work, and a lot of people are confused about how the process works.
I know I was. I'm going to talk about what is going on, how I'm
approaching things, what exactly needs done, and what I'm working on
now. More importantly, I'm going to talk about the problems I'm facing.
Problems A very important part of writing a book is facing the problems
that come with it. As with programming, there are always issues and
'bugs' that you have to deal with. I've went through 3 different editors
and 3-5 different formats while writing this book. You know you've went
through hell when you can't remember what all you've written it in. I
started out in Lyx, because it was the most approachable. Understand
that I never intended on this book getting published. I figured that it
would just be a totally free thing that I might self-publish dead tree
copies of. Therefore, I had to use the things that I could most easily
produce a nice looking PDF file from. Lyx was okay, but its editor is
really terrible to write in IMO, so I eventually migrated the entire
book to pandoc markdown. That was great. It was perfect. Unfortunately,
my publisher, No Starch, requires that I write the book in OpenOffice or
Word in order to take advantage of their stylesheet. I ended up spending
a sickening amount of time migrating the book to OpenOffice. Each time I
migrated the book to a new format, I had to do it by hand. The book
would probably be done by now if not for that. OpenOffice is where I'm
stuck, against my will. I'm dying for an asciidoc or markdown workflow
from No Starch at some point in time, but if it comes, it will almost
certainly be after my book is done with and finished. I could write
something to take a plaintext format such as markdown or asciidoc and
produce the ODT necessary, but with the stylesheet and such, I'd
probably spend more time with that than actually writing the book. So,
my biggest problem thus far has been facing OpenOffice (or LibreOffice,
which is actually what I'm using). I simply cannot stand writing in this
monstrosity. I spend more time clicking buttons and making sure my
styles are correct than writing my book. Even the keybindings are
awkward, since there is some bug in Open/LibreOffice that makes the ctrl
key useless on OS X. I feel I have enough experience right here and now
to declare that OpenOffice is simply not a good thing to have to write a
book in. Especially for a programmer who is used to Emacs. But bitching
about OpenOffice for three hours in a blog post isn't going to get this
book written. I do not blame No Starch for this. They have their
workflow and it has worked fine for them before. They've just got a
whiny Emacs user on their hands. There really aren't many more clear
'problems'. OpenOffice is one gigantic problem that I wish somebody
would fix. If somebody feels like writing a plaintext format -\> ODT
converter that somehow magically supports OpenOffice stylesheets, I'll
name my first born after you. I shit you not. Moving on. What needs to
be done, and what is being done Lots. Right now, I'm going through a
phase of "well, I wrote all of this 6 months ago. I want to do x and y
differently, so I need to change z and b and do n". I find myself
frequently not satisfied with what I've done, and thus rewriting an
insanely large part of what is already done. This might sound like a
great thing for the eventual reader, and it is, but imagine what kind of
torment I have to face because of it. Right now, I'm reading the book
from cover to cover (at least, what is already written) and I'm
restructuring parts, rewriting parts, fixing parts, etc. There is a
\*lot\* of work left to do here, because a lot of the book is already
written. However, this is an important thing and would need to be done
regardless of whether it is now or later. It needs to be now because
then I can focus on how to structure the rest of the book. Next, there
are several chapters that aren't finished or haven't even been started
yet. Those need to be finished. Finally, I need to get up-to-date with
Clojure 1.3 so that I can be sure that this book is entirely Clojure 1.3
compatible. There is no sense in another book for an outdated Clojure.
Finally, on what needs to be done, I need to become accustomed to
OpenOffice so that I can get work done. I'm not sure it is possible to
be productive in this thing, but surely I can be useful. If I can keep
from hanging myself first. Now, about this book Some people seem to have
misunderstood me saying "This book is for complete beginners." as
meaning that the book is for complete programming beginners. It
absolutely, positively, 100% is not. If you don't have any programming
experience, you almost certainly won't get through the first chapter.
This book is for complete beginners to *Clojure*, not programming in
general. I'm not going to explain what control flow means, or the
difference between an integer and a float. You need to have experience
in approximately one programming language, and the fundamentals of
programming should be clear to you. That said, this book is aimed at a
very wide audience. Anybody who programs should be able to use this
book. I'm working very hard to make this so, but it is a very difficult
goal to achieve and if it doesn't pan out, I apologize in advance. One
thing you can feel safe knowing is that No Starch is very hardcore about
technical and developmental review. They're going to kick my fat ass all
over Alabama until this book is worthy of reading, and I'm going to very
willingly take that punishment. Conclusion Writing a book is hard.
Writing a book in an environment you are unfamiliar with and hate is
even harder. But I'm being whiny. I can do this thing, I just need to
stop being lazy and focus. When I finish publishing this post and
tweeting about it and such, I am going to spend an hour or two on my
book. This whole thing is a lesson in discipline that I am grateful for.
