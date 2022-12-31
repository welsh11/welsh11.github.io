---
title: The Latest App Idea
date: 2022-12-31 14:00:00
categories: [Devlog]
tags: [noot,apps,swiftui,swift]
---
I often have ideas for applications, features I want to implement in my jobby-job application, things I want to watch/read, links I come across and a whole host of other small speech bubbles that pop into and out of existence multiple times a day.
I also have a whole host of productivity tools at my disposal to jot these things down in - reminders applications and task managers, note taking apps and text files. But none of these really fit in with the mental model I have for these bubbles.

Recently, I came across the idea (don't ask me where, I didn't write it down) of using something like the iMessage app to write small notes to yourself, and BEHOLD and idea hit me.
What I need to do is definitely build a note-taking app, with the theme of messaging yourself.

The ground-breaking idea is that you take these figurative speech bubbles and turn them into actual message bubbles - but message bubbles that are notes...and that may or may not be bubble shaped.

And lo, **Noot** was born [^1]

So, I immediately did what any sensible person would do, I bought a domain name and hunted down the name in the App store and settled back waiting for some more inspiration.
This time it would be different, this time I would ship.

Actually, *this* time was a little different. I had just watched a really good talk by Jordi Bruin entitled [Shipping Side Projects in 2-2-2 Easy Steps](https://goodsnooze.lemonsqueezy.com/checkout?cart=e66e3f25-858f-46a6-aa28-a46f64254673) and I was pumped.

So I got to work on a prototype, here is a sketch of the initial thing:

![Initial Noot Sketch](/assets/img/2022-12-31-noot-sketch.jpg){: width="350" }

Yeah, impressive! A whole new way of thinking.

In fairness to myself, I did knuckle down and started building an app. As of 31st December (2022, for posterity), it looks a bit like this:

![Current app](/assets/img/2022-12-31-current-progress.png)

The notes bubbles (newest at the bottom, like the messages app) are a mixture of text, single links (using Link Presentation Framework to render) and images.
The data is stored in CoreData locally and there are some little 'play' buttons that I will go through at a later date.

One-screen back is a list of 'Threads', which I might change to 'Folders' or something similar. These are just buckets of notes at the moment.

I'll walk through some code specifics in other posts, especially the interesting stuff like the upside-down message list.

I've been using this prototype on a daily basis and it seems to kind of work for me. I'll think I'll push on and maybe even get a TestFlight out a some stage.

--MrB

[^1]: I'm into Mastadon at the moment since the thing happened with the dude, and Mastadon posts are formerly known as Toots. Couple that with these being Notes and you have a Noot! I have no intention of calling the notes themselves Noots - that would be terrible.
