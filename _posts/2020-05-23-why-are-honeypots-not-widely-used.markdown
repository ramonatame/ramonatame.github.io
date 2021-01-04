---
layout: post
title:  "Why are Honeypots Not Widely Used"
date:   2020-05-23
---

*A few weeks ago a friend of mine approached me to discuss on honeypots. He couldn’t understand why nobody seems to use honeypots if they are so good as his professors say. Well, here are my thoughs, after having a look at a few server honeypots, service honeypots and honeytokens listed on [Paralax](https://github.com/paralax/awesome-honeypots).*

## 1. Honeypot Types

There are 3 main types of honeypots, based on files, servers and networks. The server ones are commonly called honeypots, the file ones are called honey files, honey tokens, even shortly baits and lastly the network ones are called honeynets. This article refer only to the file and the server ones as these are the common solutions out there, open source or commercial. The honeynets ones seem to be used mostly for research by security companies and universities, to understand the attackers’ behavior, but not so much for other purposes or by other companies, as far as my knowledge goes.

## 2. Deception as a Strategy

Deceiving your adversay is generally a good strategy, no matter if we talk about wars or cybersecurity. In fact, deception is the main concept behind honeypots. You use honeypots to deceive your adversary, make him think he is on solid ground or he got your crown jewells, when in fact you are well aware he is there and he plays on your music without knowing: he just triggered an alert by RDP/ SSH into a fake server you purposely placed in his way, or that file he fought to capture contains only false data you deliberately provided him to keep him occupied or make him trigger a few more alerts depending on the path he chooses.

So how do you deceive the intruder? The common methods out there to use honeypots for deception are the following:

* place some random resources (servers or files) with catchy names all over the network and hope they will catch his eyes and will trigger an alert on your side so you know he is there, without triggering too many false positives;
* create a story around some of your most wanted assets, your crown jewells and then make that story believable by placing all kinds of desirable resources and data and in such a manner that it can be captured but it’s not obvious it’s a trap as they will trigger alerts when captured.

## 3. So… Where is the problem with honeypots?

Deception sounds useful and even exquisite, who wouldn’t want to use it to improve their security and fool the adversary? Then why isn’t it used?

There are a few solutions out there but many (maybe all of them? certainly all of those I looked into) have a couple of issues that don’t make them approachable (just yet):

* they are not able to let you know anything around who triggered your alerts; they can only tell you which resource was triggered which means you may need other security tools to work around this issue and also you may need a way to distribute your fake resources without having resource collusion (the same resource placed in multiple locations) or otherwise you wouldn’t know where did the alert trigger;
* their configuration cannot be customized enough. They come with predefined signatures, banners and maybe a few options to customize them, but those who have enough interest in honeypots can still identify them, based on their look and by analysing the resource before touching it;
* they are not easy to deploy and to configure without exposing your network to other risks and without making it obvious that those systems are just there waiting for intruders. Also, there are no doable ways to hide the manner you deploy and maintain your honeypots without making it obvious, again;
* while they may seem to add a slight improvement on your security for some platform/ OS, they just don’t work on others;
* if by luck or by deploying a well planned deception you would get the intruder to access one of your fake resources, there is no guarantee the corresponding alert would get triggered. The alerting mechanisms honeypots employ would only make them trigger if they are opened in a precise way on a precise setup.

## Summing Up

Deception is good, but honeypots are still work in progress. So what does that mean? Should you give up on them? Here are my thoughts: if you have the resources and the right people, invest in implementing some level of deception around your network, without using any dedicated honeypots. You know your crown jewells, think of paths an attacker would cross to get to your crown jewells and deceive them on their way there, by planting fake alert-triggering resources (on their usage), but without exposing your network to further risk. That doesn’t mean you shouldn’t invest in acquiring honeypots. It just means that in my opinion, just not yet. Honeypots are getting there, they just need some more time.