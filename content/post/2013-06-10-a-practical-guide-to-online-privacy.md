---
title: A practical guide to online privacy
author: joefkelley
layout: post
date: 2013-06-10
url: /420
sfw_pwd:
  - U9ypmgymPJK2
categories:
  - Tech

---
The NSA has been getting quite a lot of flack recently for all of the data it has been gathering from major US companies, and in fact, the US internet infrastructure as a whole. There's a substantial outcry against this on the grounds of privacy, so I want to provide a few tips and tools that you can use if you want to keep your internet traffic secure and private, but first two disclaimers:

First, I don't feel that there is an inherent "right" to privacy of anything sent over the internet. There's no moral or philosophical basis for this, especially considering that there are many many individual parties and corporations that own different sections of the internet infrastructure. It's essentially a public space, so when it comes to privacy "rights" I think its most comparable to talking to someone out loud, in a public space, somewhat quietly. And it clearly is not a legal "right"; there's not any evidence to suggest that the NSA has done anything illegal. Blame the patriot act.

Second, even if you take all of the best possible precautions, I make no guarantees that the NSA can't still see your data. They have a lot of the smartest people in the world working there, and they have almost unlimited resources. Historically, the NSA has been a few years ahead of the rest of the cryptography research world. There's definitely no publicly known way of breaking strong encryption, but it's possible the NSA has a quantum supercomputer running proprietary algorithms that can break the best encryption. Who knows. It's very unlikely, but possible.

Additionally, even if you're encrypting everything, the intended recipient of your data can still decrypt it. Otherwise, it would be worthless. So if the NSA has backdoors into wherever you're sending your data, it doesn't matter if it's encrypted. And as we've recently learned, they do have these backdoors at Google, Facebook, Apple, Microsoft, etc, etc.

However, with all of that said, there are some simple and practical things you can do to protect yourself in a lot of cases. It all relies on basically the same process: encryption. I explained basically how it works in a previous post, so feel free to check it out. But here are some concrete ways to actually use it:

* [HTTPS Everywhere][1]
 
  The "S" in HTTPS stands for "secure", so you can guess it's a good idea to use it whenever possible. A lot of major sites support HTTPS, and this plug-in for Chrome or Firefox automatically uses HTTPS if it can. Watch your address bar; if the URL starts with "https://" and you see the little lock icon, nobody except the actual website will be able to see your communication. This is a great plug-in, and a very nice one to just "install and forget".<br/>
  
* [Tor Browser][2]

HTTPS is a good start, and it will prevent others from seeing what you are sending to the sites you are visiting, but it won't prevent others from seeing that you are visiting them at all. The Tor network is a group of computers used to bounce all internet traffic around a bunch of them before actually sending it to its real destination. The Tor browser is essentially a version of Firefox that is configured to only connect through the Tor network. This is almost certainly overkill for every-day browsing, but might be useful if you have a need to be extremely secure.

* [Pidgin][3] and [Pidgin encrypt][4]

Pidgin is a great chat client that supports all of the major instant messaging systems other than Skype (Google Talk, AIM, MSN, IRC, etc). And Pidgin encrypt is a third-party plugin for Pidgin that allows you to use RSA public-key encryption. It is very secure, but the downside is that it will only work if both you and the person you are chatting with are using pidgin encrypt. So it's probably not very useful for every-day chatting purposes, but if you have a need for a particularly private conversation, it's a nice option.

* Email

Email is a tricky one, since there are so many different email clients, and not a good standardized way to do encryption across all of them. There are a number of private key solutions, but that requires you telling your intended recipient a password, and how to give them that password securely is an entire other problem. Some email clients also support PGP, which is a type of public-key encryption, but it is not extremely secure and I would not be surprised if the NSA and other government agencies were able to crack it. And all of the solutions out there that I've seen are a bit cumbersome and unintuitive. However, it's certainly better than nothing. I recommend you google your preferred email client and encryption (i.e. "GMail encryption") to find out what's out there.

Hope this helps!

 [1]: https://www.eff.org/https-everywhere
 [2]: https://www.torproject.org/projects/torbrowser.html.en
 [3]: http://www.pidgin.im/
 [4]: http://pidgin-encrypt.sourceforge.net/
