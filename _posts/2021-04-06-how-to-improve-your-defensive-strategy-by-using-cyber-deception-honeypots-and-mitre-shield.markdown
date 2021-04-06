---
layout: post
title: "How to Improve Your Defensive Strategy by Using Cyber Deception, Honeypots and MITRE Shield"
date:   2021-04-06
tags: ["Featured"]
---

Defending against attackers can feel at times a lost cause - motivated and well-funded attackers have good tools, impressive budgets, well-trained people and above all they have patience. They rarely run out of time and need to rush their attacks. On the opposite side, we, the defenders sit. We have limited budgets and as everything depends on the budget, we have some good tools, but some less good, some well-trained people and no patience: we do run out of time. There is always so much to do to prevent broad ranges of attacks and so little resources. Which makes it incredibly important to deploy effective controls and use well-grounded security strategies.

<figure>
	<img src="{{ '/assets/img/chess.jpg' | prepend: site.baseurl }}" alt=""> 
<!-- 	<figcaption>Image by jplenio from Pixabay</figcaption> -->
</figure>

We use many tools. But we rarely use strategies. We rarely use deception - a critical strategy used since the oldest times in battlefields. Deception doesn't have to completely deter enemies or prevent their attacks, but they can simply be "canaries in coal mines", letting you know when your enemies have infiltrated in and all your other security tools and controls failed to notify you.
Deception doesn't mean honeypots. Honeypots and all their subcategories are a subtype of deception, but many more can be employed. However, deception is one of those topics many can theorize on, but few tell you how to apply it in practice. Here are some thoughts.

## Part 1: Your Crown Jewels

If you have basic security controls in place - and you should before thinking about deception - then your problem isn't random kiddies attack. Your problem is organized groups of attackers, highly resourced, such as APT groups. They're not going to launch random attacks against you or settle for the least they can get once they compromise your system - no. They are going for your crown jewels - be them financial, personal or client data, your company's strategy, trade secrets or any other critical assets. Get in the same room with the COO, CRO and/or CIO and put together a list of all the crown jewels. Keep it safe.

## Part 2: Know Your Enemies

They know you, there is a reason why you are on their target. It's so much more important that you know who they are, their goals and how they reach them. Hence, you need your Threat Intelligence/ Threat Hunting team to keep an eye on them and work with you to establish effective deception controls. Know your enemies and know them well. Most important of all, know how they think - what they look for, the conclusions they get to when analyzing your system, how they decide which path to approach, what catches their eye and what would be simply unquestionable and trusted by them. You don't want to raise their suspicions and have them decide whether to risk it or not. You want them to not even think about your deception system, so you want to make it 100% real for them.

## Part 3: Working on Your Strategy

If you don't have a well-defined security strategy, you should. In this way, you make sure all your efforts are justified and fit together. You cannot just pick tools from the market and expect them to protect you, as much as you cannot shoot in the dark and expect to hit. Base your decisions on a good strategy, centered on your crown jewels and on your enemies and have them in mind at any point you extend the strategy, such as when adding deception techniques. 

## Part 4: The Deception Story

Surely you can deploy scarce decoys around your networks, such as dummy files and fake systems or fake networks and monitor them for any interaction. That's always an option. However, far more effective is having a good story that backs up all your decoys. A good deception story would be one that doesn't forget who it is intended for (your attackers) and what they will be looking for (your crown jewels). A good deception story could invent a new crown jewelry asset, one that makes total sense for your company to have and that you don't actually have simply because of the company's business strategy or lack of resources. Finally, a good deception story could be one that stretches from your potential compromised nodes throughout the system up to the decoy crown jewels; and a story that is backed up by information in different (searchable) channels, that claim its existence and functionality. It's important to find a good deception story.

## Part 5: The Decoys, Honeypots and MITRE Shield

There are a variety of decoys that can be used to deliver the deception story(es), of which the most well-known are honeypots as in fake files placed on real servers and desktops, fake servers placed in real networks or fake networks communicating to real networks. But fake networks (honeynets) are seldom used in practice, beside for research practices or used by huge corporations - they require plenty of effort and resources, which most companies do not have. 

Decoy types can be countless. The limit is simply your imagination. However, if you are out of practice, check out [MITRE Shield](https://shield.mitre.org/matrix/). MITRE Shield is a knowledge base that maps out a broad range of defensive tactics and techniques to [MITRE Att&ck](https://attack.mitre.org/matrices/enterprise/). It can be useful for many defensive purposes, including as a source for decoy examples and cyber deception techniques - the best one I found out there. Recently, they mapped these defensive techniques by APT groups as well, which can only help you tighten your deception plan around your enemies.

Use decoys as a means to deliver your deception story. Place them as "breadcrumbs" throughout your system, starting from the likely points of compromise and continuing up to the place where your fake crown jewel sit. The decoys should always be consistent with the story you deliver and with the rest of your administrative and technical controls.

## Part 6: Stakeholders Buy-In

You already invested quite a lot of effort in planning if you reached this point. However, the real effort just begins. This will take internal resources, but you may need some COTS products to help you speed up the delivery and efficiency of the deception system, though you should check the [open-source solutions](https://github.com/paralax/awesome-honeypots) as well. Get your stakeholders approval before you move forward.
 
## Part 7: Performance, Effectiveness and Monitoring

Monitoring your decoys system is essential - you cannot just leave it there and check it from time to time. Its value comes from giving you a timely alert when there is an intruder. Hence, you need a way to tell when one of your decoy is tripped and you need to be able to uniquely identify the decoy and the system where it triggered. You should integrate your deception system with the rest of your security tools and use them to deploy the decoys, monitor them, alert on them or to correlate them with other events in your system.

Unlike other security systems, assessing the effectiveness of a deception control can be difficult. If a decoy does not get triggered, it doesn't mean you system is intruder-free. It may mean the intruder was not interested in the decoy, he detected it as a decoy or he simply didn't see it. You can only assess effectiveness in time, when real attacks happen or when doing external red teaming or penetration testing exercises. An effective deception system alerts on the highest criticality level as it should have zero or a very low false alert count. For that, employees need to be trained to not trip the decoys and that can be a real challenge.

Performance simply put means how fast the decoy was triggered after breaching the system. This is a simple metric, given that OS logs can provide enough information for this assessment. Performance and effectiveness should be assessed and improved with every opportunity.

## Summing Up

Designing defensive deception controls is as much a science as it is an art. You start from defining your crown jewels and understanding the way your attackers think, then you move into defining fake crown jewels that don't pose any questions to their existence and create a story around them. Understanding the likely points of compromise in your network, you start placing decoys as breadcrumbs (consistent with the story) throughout your system, on the paths that could lead to the decoy crown jewel. Make sure you don't forget documenting this crown jewel and mentioning it through various communication channels. Finally, monitor all the decoys, be prompt in investigating them and improve them with every change you get.

## Further Resources

* *Crafting an Effective Cyber Deception by Adam Tyra, Cyber Magazine* - An incredible article on cyber deception, walking you through what deception means, the mindset you need, practical steps to follow. I also consider this article one of the best writings in security altogether.
* *[MITRE Shield](https://shield.mitre.org/matrix/)* - Knowledge-base for blue teams on defensive tactics and techniques they could employ, including cyber deception techniques
