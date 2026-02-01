---
title: "Pairing"
date: 2023-04-15T11:11:06-04:00
draft: false
tags: ["software-development", "pair-programming", "collaboration", "engineering-practices", "teamwork"]
---

For the past few years I have drifted away from working on software regularly
in my day job.  My new role gives me an opportunity to work on code regularly
again so I've been looking for reasonable projects to jump into.  There is no
shortage of work, but I want to make sure that I don't get too much into the
critical path since I won't be able to devote my full attention to working on a
single task for long.

So, to summarize, I have time to work on software, my team has plenty of
software work to go around, and I haven't done it in a while.  My conclusion is
that I need a plan to be impactful, learn, and not get in the way.

I have done a good bit of pair programming in the past and have always found it
to be enjoyable and a productive method for my work projects. I think that if I
can find the right opportunity to pair on a high-impact issue that it would be
ideal, so I pitched it to a colleague. 

The person I reached out to is an archtect on one of the major systems in my
organization and the conversation went something like this:

> me: Hey, do you work on issues regularly?
>
> them: No, not really.
>
> me: Do you want to?
>
> them: Sure!
>
> me: Have you ever tried pair programming?
>
> them: I've shared my screen and had someone look over my shoulder and
>       criticize me as I tried to work on a bit of code before, so yeah.
>
> me: Oh, that's not what I was thinking...

I think it's pretty common for someone who has never paired before to have some
anxiety about the activity.  It's really easy to feel judged or insecure when
you know someone is watching you work very closely.  So I think it's really
important to build a bubble of psychological safety

Luckily, my colleague is a good sport and agreed to try pairing with me even
after his less than ideal previous experience.

We used [tmate](https://tmate.io/) to share a tmux session.  I had my colleague
host the session so that we'd be using his tools on his system, and before we
got started I told him "the rules":

1. There is a driver and a navigator
2. Before switching roles, the navigator will say something like "let me drive for a bit".

We worked for about two hours and were able to make enough progress to reach a
stopping point we were both happy with.  After, my colleague mentioned that it
was much more enjoyable than he expected and he was glad we decided to do it.

The experience got me to thinking about what I believe are the benefits of the practice.

- Reduces the "bus factor", meaning more than one person now understands how somethe thing works
- All participants learn and teach each other things about how they work
- Improved quality for the next person(s) to touch the project

I'm sure there are some other ones, but to me, the above points are hard to deny.
