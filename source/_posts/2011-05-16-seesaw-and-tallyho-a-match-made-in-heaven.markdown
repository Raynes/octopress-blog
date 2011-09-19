---
author: Raynes
date: '2011-05-16 01:18:52'
layout: post
slug: seesaw-and-tallyho-a-match-made-in-heaven
status: publish
title: Seesaw and Tallyho, a match made in heaven
wordpress_id: '146'
categories:
- Clojure
---

I do not like swing. Swing is the bane of my existence. As a matter of
fact, none of the various Java GUI frameworks really make me happy. When
I have to use GUI, I always use Swing regardless of my hatred for it,
simply because it is the most accessible of the GUI frameworks. I
haven't written many GUI applications in Clojure because avoiding Swing
became a goal. After a week or so of work on a desktop wordpress editor,
I had decided that I would, at some point, write a wrapper around Swing
to make it less insane and painful to use. I'm very happy to say that
somebody took it upon themselves to do this, and it wasn't me. Enter
Seesaw There is a God of a man out there named [Dave
Ray](http://github.com/daveray) who is working on a fantastic Swing
wrapper in Clojure called [Seesaw](http://github.com/daveray/seesaw).
Seesaw has a concept of 'widgets', where each Swing component is a
widget and a widget is a much more clojury interface to the underlying
component. The README is fantastic at explaining all of this, and I know
that I won't do it justice, so just read that. Enter Tallyho My family
and I play a lot of card games. Games like UNO and Quiddler where
scoring is essential and tedious. How much paper have you wasted during
your lifetime just by scoring board games and such alone? Quite a lot,
I'd bet. I know we have. Recently, we've been using an Android
application for scoring. It works really well and made things a lot
faster and easier. Our pens and papers rejoiced as the war against UNO
ended and they were free. Now I have a laptop. A macbook pro, to be
precise. When we play games, I tend to carry the laptop in with me and
play music and such with it. It only makes sense that I would not crowd
the table with my phone as well just for scoring when I have a laptop in
front of me, but alas, I could find no decent score keepers like the one
I used on Android. I was sad. I sat and pondered my life and the
universe, cried a few tears, blew my nose, and decided that I would just
write one myself. ![Tallyho](http://raynes.me/hfiles/tallyho1.png) Now
we've already laid down the cold, hard facts, of which there are two:
Swing sucks, Seesaw is awesome. Therefore, it was logicial that I write
Tallyho using Seesaw, and so I did. The result is currently 111 lines of
Clojure (and the majority really is Clojure and looks like Clojure and
not Java!), and is actually pretty nice looking, if simple. There is a
menubar and corresponding right-click menu that all have the same items:
add player, delete player, reset scores, reset all scores. Everything
you need for scoring. The whole main pane is just a giant JTable, a
table with names on the left and scores on the right. To modify a
player's score, you double click on a row (a player) and a box pops up
as seen above. You can enter -number to decrement a score or
+number/just the number itself to increment a score. The most shocking
thing about this is how quickly and easily I was able to write this. I
got the basic thing written in 3 hours, and most of that was learning
Seesaw and looking up JTable documentation. It has a good number of
features, looks good, scales good, is cross-platform, stable, and all in
about an hour a day for 3 days. I would have trouble with just
*finishing* the thing if I had written it in straight Swing. There is
nothing that can make me run screaming away from an application faster
than Swing, but Seesaw made things better. The Code Since the entire
application is weighing in at just 111 lines, I'll go ahead and paste
the whole thing, as it is right now, here: [clj] (ns tallyho.core (:use
seesaw.core [clojure.string :only [join]]) (:import [javax.swing
JOptionPane JTable] javax.swing.table.DefaultTableModel
java.awt.event.MouseEvent) (:gen-class)) (def table-model (proxy
[DefaultTableModel] [(to-array-2d []) (object-array ["name" "score"])]
(isCellEditable [row column] false))) (defn calculate-score [[calc &
digits :as all] old] (let [old (Integer/parseInt (str old)) digits (when
digits (Integer/parseInt (join digits)))] (cond (= \\+ calc) (+ old
digits) (= \\- calc) (- old digits) :else (+ old (Integer/parseInt (join
all)))))) (defn validate [s] (when-not (empty? s) s)) (defn
calc-new-score [old] (when-let [new-score (validate
(JOptionPane/showInputDialog "Enter -number to decrease score.\\nEnter
either +number or number to increase score."))] (or (try
(calculate-score new-score old) (catch NumberFormatException e)) (do
(alert "Enter a real number, dude.") (recur old))))) (defn
on-table-click [e] (when (and (= (.getButton e) MouseEvent/BUTTON1) (= 2
(.getClickCount e))) (let [s-table (to-widget e) row (.rowAtPoint
s-table (.getPoint e))] (when (\>= row 0) (when-let [new-score
(calc-new-score (.getValueAt s-table row 1))] (.setValueAt table-model
new-score row 1)))))) (declare score-table) (defn add-user [e] (when-let
[user (validate (JOptionPane/showInputDialog "Enter the player's
name."))] (.addRow table-model (object-array [user "0"])))) (defn
delete-user [e] (when-let [row (selection score-table)] (.removeRow
table-model row))) (defn confirm [] (= JOptionPane/YES\_OPTION
(JOptionPane/showConfirmDialog score-table "Are you sure?" "Seriously?"
JOptionPane/YES\_NO\_OPTION))) (defn reset-scores [e] (when (confirm)
(doseq [row (range 0 (.getRowCount score-table))] (.setValueAt
table-model 0 row 1)))) (defn reset-game [e] (when (confirm)
(.setRowCount table-model 0))) (def add-user-action (action :handler
add-user :name "Add Player")) (def delete-user-action (action :handler
delete-user :name "Delete Player")) (def reset-scores-action (action
:handler reset-scores :name "Reset Scores")) (def reset-game-action
(action :handler reset-game :name "Remove All Players")) (def
score-table (doto (table :model table-model :listen [:mouse-clicked
on-table-click] :popup (fn [e] [add-user-action delete-user-action
reset-scores-action reset-game-action]) :font "ARIAL-PLAIN-14")
(.setFillsViewportHeight true) (.setRowHeight 20))) (def scroll-pane
(scrollable score-table)) (def menus (menubar :items [(menu :text
"Tallyho" :items [add-user-action delete-user-action reset-scores-action
reset-game-action])])) (def main-panel (mig-panel :constraints ["fill,
ins 0"] :items [[scroll-pane "grow"]])) (defn -main [& args] (invoke-now
(frame :title "TallyHOOOOOOO" :content main-panel :on-close :exit
:menubar menus))) [/clj] The project is also on
[Github](https://github.com/Raynes/tallyho). The most amazing part of
this, IMO, is the menus. Look at them! Holy shit! Have you seen Clojure
code that works with JMenus and JMenuBars directly? You \*have\* to
abstract over it in every application just to be able to look at it and
then look at yourself in the mirror in the morning with pride. With
Seesaw, we have these reusable 'actions'. We use these actions in the
menubar and the popup menu so almost no code is duplicated or
unnecessary. Then look at the frame. The frame is 5 lines of code and
could fit on 2 or 3. With mig-panel, I can create a mig-layout'd panel
so quickly and easily that it shouldn't even be legal, and that applies
to other layout managers as well. Seesaw is making Swing possible to
read and is getting rid of tedium. At some point, it will be a fairly
comprehensive wrapper. Right now, it is still experimental. It is
experimental because we don't really know what we *want* out of a
Clojure Swing wrapper, and we're experimenting to find out. The author
is **very** open to feedback and he absolutely needs it in order for the
project to continue and survive, so please use Seesaw and share your
experiences with him. He is very responsive. The Seesaw project is more
important than I can properly express here. GUI programming is very hard
if you aren't familiar with Java and Swing when you come into Clojure,
and even harder to tolerate once you are. Seesaw has the potential to
mitigate this problem effectively, but the community needs to play a
part. Participate! Interested in Tallyho? I imagine that the people who
read this won't all be programmers. I also expect some people to end up
here as a result of googling for score keeping apps, and that's great. I
hope Tallyho can serve your purpose. If you want to try it out, go to
the project's [Github](https://github.com/Raynes/tallyho) page, click
the big 'downloads' button and then download the latest version of
tallyho.jar. This is a standalone jar that you can use. On Windows, you
can typically just double click such a jar for it to run. Otherwise, you
can run it from the command-line by executing the command \`java -jar
tallyho.jar\`. Usage of the application should be fairly self
explanatory. If you run into any bugs or have any feature requests or
anything like that, create an issue on the github page. Feedback would
be enjoyed.

[Seesaw's Github page](http://github.com/daveray/seesaw) [Tallyho's
Github page](http://github.com/Raynes/tallyho)
