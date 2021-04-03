---
created_at: 2015-11-17
title: Twelve weeks of Racket and CS:APP
kind: article
---

[As promised][track-lifetime-classes-post], this is a wrap up of my experience concentrating for (slightly more than) 12 weeks on only 2 subjects. For this first stint, I chose to learn [Racket][racket-website] and to read [Computer Systems: A Programmer's Perspective][csapp]. Not long ago I have lost interest in providing ultimate motivations for what I do in my free time; I'll just stick to what happened. (I can't promise I won't motivate everything else, which is not ultimate.)

## Starting Racket, and why I will go on

Racket is sensational. Historically, I have come from C++ and object-oriented programming, which turned into a love for Java, only to then run away disgusted, full of awe before the purity of C (sic). Functional programming is something I have run into only after my college graduation, and for a few years, it remained a buzzword in the back of my head reminding me of something I wanted, one day, to try.

Some time ago, I read a [very enthusiastic post on Racket][pollen-rkt-post], who got me excited. Since roughly the end of August, I have dedicated 45 minutes a day, every working day, to learn Racket. I've done a few things, and I am amazed of that, even though it might seem peanuts to the casual reader (and maybe to the causal one too).

### Great community, early contributions

I knew that the Haskell community was very welcoming; I can say the same of the Racket one. I'll give an example. I have found these (supposedly) easy [Intro projects][intro-prjs] to get my feet wet, and I saw someone posting his very first experiences with Racket on the [Google group][rkt-grp], so I picked one of the projects (I wrote a very simple program for finding anagrams), and posted on the group [only to say that][letter-to-grp]. A few hours later, I get a reply from Greg Hendershott, a very kind person with lots of impressive contributions to the community, among which [Frog][frog], a blog software written (obviously) in Racket, and [Fear of the macros][fotm], a great reading on Racket macros.

I have also contributed a [couple][acc] of [patches][etl] to the Racket's track of [exercism.io](https://exercism.io/), having the chance to collaborate with other spectacular people who managed to suggest me a couple of very instructive improvements on my code.

I will also cite my good experience with [HackerRank's functional programming challenges][hr-fp], who made me try the basics and keeps me company when I want to do some katas.

### The way people can use Racket is awe-inspiring

[The Racket Way][rkt-way] has impressed me. Imagine a language with which you can do everything, including slides for presentations and writing books. A language that gives you the chance to write other languages, which are still Racket, no, _a_ Racket, that you can use to other things.

With C++, no, actually with any Turing-complete language, you _could_ do everything. To the risk of repeating what other more brilliant people have already said with better-chosen words, however, it is not the same to write something in assembly or in Python.

But there's more. Here's a personal anecdote: I have intermittently written software in OO languages since 2000, and have never been able to write something with a GUI. I know, it's silly; I could've taken the time to try. I did. Several times. I have tried Qt, then wxWidgets with C++; then said to myself, why not Python? I don't want to write a lot of boilerplate code, I want just to see a damn thing on the screen. Maybe I wasn't constant enough on these endeavors; but after a week of reading and trying, if you only see the result of some tutorial on the screen, and you are not creating your own thing yet, you start getting demotivated.

This morning in 50 minutes I have written a [basic, buggy calculator in Racket][calculon].

### Racket: approved!

I have this dream of being able to write purely functional programs not so late from now (whatever that means!). And I am amazed at the possibilities. This is why I have decided to use Racket for another 12 weeks.

## CS:APP, and why I'll do something else instead

(My interpretation of) the promise of _Computer Systems: A Programmer's Perspective_ is quite captivating: revise your knowledge of the whole system, concentrating more on which aspects impact a programmer writing his program.

The book starts from a general introduction to systems, and then progresses from arithmetic representation to assembly to processor design, up until virtual memory, concurrency, and other high-level constructs.

I was thrilled because during my last job search I have run often into the wall of "Design a possibly big system made by interconnected subsystems, to solve problem X", which is a skill I feel I don't own yet. The foreword of the book and the index got me hooked, as I thought that a good revision would do me good and _teach_ me the skill.

### Why I stopped

I think I can no doubt learn this skill, but not by reading a book; or at least, not only. What I need is direct experience building systems, which is exactly what I am doing now.

It is of no use to read about virtual memory, like I did in college, and never try to write a virtual memory system for the need of it. Nothing is comparable to the way we learn when in real need. At that point, maybe after the mess that got to the delivery of a product, I might consider reading a well-organized treatise, that would then click with steps I followed _in a totally different order, possibly with a totally different focus_.

The book is just great; it is not for me now though. If I want to focus on concurrency, I read a book on concurrency and at the same time I work on a project using it. I have found no other method working for me. In other words, I only have praises for a book; but it is only a book.

## The next 12 weeks

These first 12 weeks started somewhere in late August and officially ended on November 8. I'm not sure I'll start 12 more right away, since I am in a peculiar time of the year, and I'll have to stop 3 weeks for vacation at some point.

The plan so far is to fill this time with a lot of Racket and as much thinking, and come up with a project I want to focus on for the next 12-weeks stretch.

[track-lifetime-classes-post]: /blog/track-lifetime-of-std-objects/
[racket-website]: //racket-lang.org/
[csapp]: //www.amazon.co.uk/gp/product/013409266X
[pollen-rkt-post]: //practicaltypography.com/why-racket-why-lisp.html
[intro-prjs]: //github.com/racket/racket/wiki/Intro-Projects
[rkt-grp]: //groups.google.com/forum/#!forum/racket-users/
[letter-to-grp]: //groups.google.com/d/topic/racket-users/UGE4kn21xBg/discussion
[frog]: //github.com/greghendershott/frog
[fotm]: //www.greghendershott.com/fear-of-macros/
[acc]: //github.com/exercism/xracket/pull/25
[etl]: //github.com/exercism/xracket/pull/27
[rkt-way]: //www.infoq.com/presentations/Racket
[calculon]: //github.com/sturmer/calculon
[hr-fp]: //www.hackerrank.com/domains/fp/intro
