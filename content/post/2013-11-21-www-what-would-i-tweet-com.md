---
title: www.what-would-i-tweet.com
author: joefkelley
layout: post
date: 2013-11-21
url: /546
sfw_pwd:
  - kp9PsWIwPZtP
categories:
  - Programming

---
I recently finished the first version of a website:

[what-would-i-tweet.com][1]

The idea for it is shamelessly stolen from a website that does the same thing with Facebook. You log in, and then it generates tweets similar to what you would tweet, based on your tweet history. You can also have it generate tweets based on other people's tweets. For example:

[what-would-i-tweet.com/generate/amandabynes][2]

I am bad at front-end web development, so it's extremely bare-bones for now. It also requires you to log in with twitter. The problem is that twitter's API is rate-limited, and it's a limit per app if you don't have the users logged in, or per user if they are logged in. So if I didn't require logging in, I'd much more quickly hit the limit.

I also put the code up on [Github.][3] Check out the source code if you're interested in the algorithm I'm using. Also, I used Scala Play Framework for the server, and twitter4j for the Twitter API, so it might be a decent example if you're looking to learn how to use either of those.

Enjoy!

 [1]: http://www.what-would-i-tweet.com "www.what-would-i-tweet.com"
 [2]: http://www.what-would-i-tweet.com/generate/amandabynes
 [3]: https://github.com/jfkelley/what-would-i-tweet#what-would-i-tweet
