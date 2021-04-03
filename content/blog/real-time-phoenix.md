---
title: Review - Real-Time Phoenix
created_at: 2020-12-26
kind: article
tags:
  - phoenix
  - elixir
---

<a href="//www.librarything.com/work/25306228/">
    <img src="/gallery/real-time-phoenix.jpg" width="150" alt="Real-Time Phoenix" />
</a>

Stephen Bussey wrote a book that explains how to use the Phoenix Web framework to create real-time applications. The basic building blocks, presented early on, are the Phoenix concepts of Channel and Socket. The Socket is responsible for connection handling, while Channels allow users to subscribe to named flows of informations and be updated on changes they are interested in.

After introducing the main tools for the job, the book has a large central part that shows the development of an application that meets the real-time requirements (in a nutshell: the user sees changes without having to refresh the browser). These are clear and easy to follow. For the few times I've got stuck, I could compare my code with the codebase that Stephen made available for download.

A third part is a treatment of ways to deploy real-time applications. This was of limited interest to me as I'm not involved directly in this phase of the life cycle, but it was very interesting to know what is going on at the far end of it (and you never know when you need to deploy your own project).

The last part is an overview of what you can do on the front-end side of things, with a chapter that uses React (not bad) and one on LiveView, a promising front-end technology that allows us to use Elixir to write most of the UI and only relying on a relatively small fragment of ECMAScript.

I find the subject quite advanced for my current level but sure it offered a detailed and interesting treatment of the subject. Recommended if you touch applications written in this techonology.
