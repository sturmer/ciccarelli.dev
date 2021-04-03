---
title: Designing Data-Intensive Applications, by Martin Kleppmann (5/10)
created_at: 2018-02-17
kind: article
---

This book is a survey of the staggering amount of pieces of technology that can be used to build data-intensive applications, defined (in my rough interpretation) as user-facing systems that treat significant amounts of data and are expected to do it quickly. Contains reflections on trade-offs to take into account when choosing one solution over the other.

## Recommended to...
Read this if you work with distributed systems and want a fairly exhaustive overview of what's going on in your codebase. If you know your (Apache) Kafka and your Hadoop and your ZooKeeper, you might as well spend your time in better reads.

## A somehow longer version
The undeniable merit of this book is to bring together in a reasonable space a wealth of research on distributed systems. Allow me a hazardous analogy. Reading about NP-hard problems makes you aware that there's a class of problems that you can't solve efficiently, unless you come up with an improvement on the state of the art; reading this book helps you get acquainted (I won't say familiar) with situations in which a problem is hard and/or has been solved using a given class of technology.

In general, I don't like surveys. They're sometimes very useful though.

I don't like them because the bad ones tend to fall into what could be called the _shopping list trap_: just keep listing names to show that you've done your research. "Problem X is hard. Product 1 solves it by using methodology M, but gives up on this functionality; product 2 offers it but lacks this other feature. My dear friend at company C integrates 1 and 2 and offers this, but then your circumstances might not require the expense." Sounds familiar?

But surveys are sometimes very useful, because you get exposed to lots of ideas, and hopefully you'll be able to recognize a problem when you see it and know how to look for a piece of software that solves it (a shopping list might be useful in that case), or you know that you need to write one.

I consider this book a rather good survey. I've read my fair share about problems in distributed systems before, but I was lacking the bird's eye view that is very important, and that you acquire only after much experience. I don't really believe in condensing experience, but this book is a good attempt at that. You still need to sweat blood on real systems in order to appreciate the beasts you're dealing with.

The last chapter was horribly hard to read. The idea of giving a personal interpretation of trends is commendable, but in this case, the execution just felt unnecessarily heavy. Then, an unexpected and appreciated reflection on the surveillance state in the very last section of the book, which is an eye-opener. If it wasn't for that, you could skip that chapter. (Well you still can, and only read that section.)
