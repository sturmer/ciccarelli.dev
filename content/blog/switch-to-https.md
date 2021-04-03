---
title: Switching this site to https
kind: article
created_at: 2017-02-01
---

Today I ran into an article by one [Troy Hunt](//www.troyhunt.com/https-adoption-has-reached-the-tipping-point/) that talks about the fact that https is now past the status of exception, and is becoming the new norm.

Not long ago, I had thought about switching this website to the secure protocol, but was intimidated by the prospect of work and bureaucracy that I needed to get into. Troy's post was enough to make me give it a serious try; I must admit, it was so much simpler than I expected, that I could easily have done it earlier.

Apparently, GitHub pages allows you to automatically switch to https, if you don't use a custom domain name, but _yourname_.github.io. Come on, GH! I was so close! As the most astute reader might suspect, I do use a custom domain name. What to do?

I googled a bit more and found [this page](//sheharyar.me/blog/free-ssl-for-github-pages-with-custom-domains/) (kudos!), that introduced me to the amazing Cloudflare, that offers a zillion of services to improve websites (I was impressed by the CDN that serves your page faster). The pricing plans for businesses appear intimidating, but for personal web pages, they have a free one. I was sold on the spot!

I needed to change the DNS on my domain registrar, and everything was setup, but, uh, my website itself.

I went through all the links, and removed the _http:_ and _https:_ parts. This works ── I didn't know. I also included [the simple script](//github.com/sturmer/sturmer.github.io/commit/eb30c5b70d1ecfbec1939d9b49dd3ef6b46b1d8f#diff-15ca5b452b8160308132063abf26b8a7) found in the post above, to redirect anything to https that comes from http.

And the trick is done! This site is now served via https.
