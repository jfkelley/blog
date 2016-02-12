---
title: Quick ssh/tmux tip
author: joefkelley
layout: post
date: 2013-10-01
url: /514
sfw_pwd:
  - gKezpiu2jNbH
categories:
  - Programming
  - Tech

---
I used to have the bad habit of ssh-ing into a machine, starting some long-running process, and then being stuck wherever I was (at work, wanting to go home, or at home, wanting to go to work) because disconnecting from wifi would mean my ssh session would die, and take down the long-running process with it. This is easily solved with something like tmux, but I would often simply forget to start a tmux session after ssh'ing in. I solved this with a little script that may be of use to someone else:

~~~bash
ssh "$@" -t 'tmux a || tmux || bash'
~~~

I put that in a file named "ssht", dumped it in bin, and can do something like:

~~~bash
ssht user@remote
~~~

And I never forgot to tmux again!

Maybe this kind of thing is obvious to the unix/linux gurus out there, but I've been making an effort to make small changes like this to optimize my workflow, and it's very satisfying!
