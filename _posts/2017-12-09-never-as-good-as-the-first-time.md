---
layout: post
title: "Never as good as the first time"
date: 2017-12-09 10:00:00 -0500
permalink: /brews/:title
---

In a recent guest post on [Intoli's blog](https://intoli.com/blog/luigi-jupyter-notebooks/), I described the basics of using [Luigi](https://github.com/spotify/luigi) to build data science pipelines.
I also introduced the `JupyterNotebookTask` class that I wrote to run Jupyter notebooks as Luigi tasks.

Building this Luigi extension was a first time in many ways for me.
In particular, this was my first contribution to an open source project, the first piece of (hopefully semi-useful) software that I made available to a larger community, and my first GitHub Pull Request.
And the blog post was my first (ever?) post.

It's interesting how much you learn by writing as little as 200-300 lines of code and sharing it with others.
Here is a summary of my thoughts around this experience.

**Learning from others**

The simple fact that you have other (more expert) developers reading through you code is a great learning experience.
They help you spot bugs and design flaws at an early stage, which ultimately makes your code much more robust.

Also, absorbing their thought process helps you to write better code next time and to avoid making the same mistakes.

**Abstraction and generalization are important**

You typically start writing code to solve a problem.
More precisely, a *variant* of a problem (maybe associated with a work/study/research use case).

You want other users to use your code to solve the same variant of the problem, but also other variants that you simply have not encountered.

In order to make that happen, you'll have to put aside the details of the variant of the problem that brought you to the point of writing code in the first place, and start thinking about the common features between your variant and all other possible variants.

Ideally, you want your final product to be as generalizable as possible.
Sometimes, you just can't make it as general as you wish.
But you should be able to understand what the limitations are so that you can communicate them and help other users saving time and avoiding frustration.

**Readability is important**

Both for code that you write just for yourself and for code that you share with others.

If more experienced coders have a hard time reading your code, your code needs to be improved, simplified, or better commented.
If they don't understand what a block of your code is doing, chances are that neither do you.

**Stay humble and take criticisms positively**

Don't fall in love with your code once it starts to look good.
If at some point you have to change your code to make it better, get your hands dirty again and make the changes.

**Consistency and conventions are important**

Especially in big projects.

Before attempting to share my own code, I read a lot of other code contributions.
Code standards make it possible for everyone to read and understand existing code quickly, and make it possible for you to start contributing with your own ideas as soon as possible.

**Code reviews are fun!**

There is a lot that Academia could learn from the way code reviews are done in open source projects which could be used to improve the quality of the review process for academic papers.
Both from a technical standpoint and, even more, from a human standpoint.

There, I said it.

**You should try to share as much as you can.**

I know several people (including myself) who are code-shy or lack the self-confidence to share their code with other developers (especially with more experienced ones).

This is a shame, because I am sure that folks like us have great ideas in their heads that are too often held back without a rational reason.

I know that many of us think *"It's OK if I don't work on implementing my idea. Someone who has better skills and more experience will implement it at some point, and she will do a better job than me anyway."*

Maybe yes, likely not. The fact is that your idea is in your head, not in someone else's.
If you feel like you need help with the implementation, find a code-buddy and start a collaboration.
A lot of code-buddies out there can offer the coding skills (which you too can develop with time) and they can help you building your product. But only *you* can offer your idea.

Consider the possibility that the fear of people looking down on you or being rude is more in your head than out there in the world of open source projects.

And anyway, what's the worst that can happen?

- Somebody will ask you to make changes before your code is considered acceptable?
- Someone will find a bug in your code at some point and you'll have to fix it?
- Someone will tell you that your code can be further improved or generalized?

If you keep a positive mind, any of these events will lead to an improved version of yourself and of your code.

Easier said than done, you say. That's for sure.
But progress is made step by step, and this little experience made me a lot more confident, a lot less shy, and definitely happier.

So to you shy friends, I say: give it a try.