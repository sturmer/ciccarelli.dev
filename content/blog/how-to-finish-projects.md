---
title: How to Finish Projects
kind: article
created_at: 2020-04-24
summary: "Where I discuss what I think are the key elements to finishing projects"
tags:
 - project management
 - personal projects
---

In this post, I want to detail some of the criteria that have helped me pull through the effort of creating a small budgeting application. I also mean to explain why it's called Budgeter here, but `capital` on GitHub, and about the benefit of inconsistencies and how this is related to _The King's Speech_.

I like reporting, and I do it in several domains. One of them is budgeting. I use a spreadsheet to note down the expenses I have each month and try to separate them into categories, so I know that a given month I've spent, say, 100 euros in restaurants. I like to see trends and to take action in case something is not going according to the plans.

In the spirit of learning, and since I love to have side projects, I started writing an application to do the exact same thing.

I don't need to school you on the benefits of reinventing the wheel[^1] : you get to pick whatever technology you want to understand better, and try to develop a small product, so you understand what type of choices go into making it.

This is what I set up to:

> I want an app that allows me to report expenses.
> I should be able to add an expense, inserting date, amount, category, and a small description.
> The app should be able to show some reporting on them.

Easy. How do we go about it?

I wanted to try React. I've used it in the past but never brought a project to completion (if you disregard, like I do, the output of video courses). What goes into bringing it up until you start writing a line of code is the subject of another post; here I only want to say something about the scope and the lessons learned.

## In whatever project, however small, complexity is your main enemy.

For any feature you can think of, there are many ways of making it better, or more thorough, or more visually pleasing. The first thing you need is focus. The second thing you need is a plan with priorities, and a way of making sure that you are focusing on something important.

I don't like making plans a lot in advance, before I even know whether or not I'll be working on something for a few weeks. So I started writing code to achieve the few goals I stated above. After the first week, I started moving the biggest TODO/FIXME notes I had been sprinkling in the source code to create GitHub issues. I grouped them into Milestones and decided that before making this project public, I wanted some things fixed.

To stay focused, I use short timers usually called _pomodoros_. I set a timer for 15 minutes and try to do one thing only[^2]. At the end of the 15 minutes, I try to think if I'm focusing on the most important thing to do; if so, I set another one.

> Resist the temptation of working on something else, _especially_ if it's another bug that you'd rather fix now.

I found myself drifting away from the reporting functionality in order to make the `node_modules` a single directory shared by client and server, rather than two separate ones. It's a secondary issue, yet I wanted it solved because it bothered me. Don't do that. Fixing the double `node_modules` directory was relatively easy... locally. Later I found out I had screwed up the Heroku deployment.

Another example of minor issue: I wanted to have a project with a clear, unique name. I thought of the not so original `Budgeter`, but it was used somewhere else, so I needed to call the project `capital` on GitHub. I'm not sure how long it takes to fix this inconsistency. I only know that it's not a big deal, and I'd rather write this blog post than dedicate any time to that.

**However, there's an exception:** Consider working on less important issues if you're bored or stuck and need some virtual fresh air.

Your life is not a dictatorship and you can digress and not follow your own rules from time to time.

## The dangers of Refactoring

Refactoring is sometimes a minion of complexity.

Don't get me wrong: refactoring is a noble activity. It becomes a problem when you let it take all your time.

It's attractive because many times you don't need great focus to do it. When you're tired, taking the logic out of an Express route and putting it in its own middleware seems very important. But you can forfeit it to a later time. This is extremely important if your daily time budget for your project is, e.g., around 1 hour.

> Don't let the small things distract you from the big ones.

## Cut scope

I set myself a deadline. If you don't, you risk of going on forever until the app is perfect.

Pro tip: It never will.

Just list the features that you want to see, prioritize them, and work on them. The deadline is too close and you underestimated the effort? Enter **Scope reduction**. Instead of pulling all-nighters, _cut the feature to the bare minimum that can be shown to let you say that your software does X_.

I wanted the reporting to be way more sophisticated than it ended up being, but I my application didn't allow a user to log out. What comes first? I decided to strip the reporting feature to its essential, postpone the frills for later milestones, and get the logout feature in.

## Perfect is the enemy of the good

In _The King's Speech_, the therapist does something interesting to help his patient, the king.

He believes that part of what makes the king nervous is that the act of delivering a speech is seen as too perfect, and the king is afraid of perfection and thus stutters. To reduce this, he suggests that he scribbles over the speech on the paper. Seeing the page tainted helps lowering the tension the patient feels for it and helps him stutter less.

The same happens in software. You want to show something you're not embarrassed of. You want to get the perfect directory structure, never repeat your code, follow all the best practices. All of this is blocking. What you need is:

> Put the damn thing out there in the open

And to do that, you need to accept imperfections. You need to be able to say, _This is what I have done, given the constraints I had. If I had more time, I'd have done a better job_. That's it.

This might make the difference between delivering something and always stopping before the finish line.

[The overview here.](/blog/budgeter-overview/)

[^1]: Don't reinvent the wheel at work!
[^2]: The Pomodoro Technique suggests 25-minutes long pomodoros, and 5-minutes breaks between them. I tweaked it to make it work for me.
