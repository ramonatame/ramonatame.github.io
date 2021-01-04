---
layout: post
title:  "How to Create Custom IDS Models"
date:   2019-11-16
---

Creating your own IDS custom models is not a difficult task, although it might require a lot of patience and time, depending on how well you know your infrastructure and the amount of exceptions lying around between systems. This article provides you with a baseline to have in mind when you create new IDS custom models for your own infrastructure. If what you need is a list of custom IDS models you can start from, check out [7 Ideas of Custom Models for IDS](https://bluehatsecurity.net/intrusion-detection-systems-custom-models/).

## #1 Get comfortable loosing some visibility, but not too comfortable

Ideally you would be able to have the exact amount of visibility in your network: ignore every communication that is safe and alert on anything that shouldn’t happen. In practice, this is a matter of configurations, resources and available features on the tools you are using and a perfect balance is at the very least difficult to obtain. You would always get some false positives and loose visibility in some other places. Whenever you whitelist an IP, a protocol, a port, a user or anything else, you become blind in those scenarios: you loose visibility in case an incident happens using the whitelisted resources. However, if you do not whitelist anything, you will get way too much operational work because of false positives. So get comfortable loosing visibility. But not too comfortable.

## #2 Be specific. The more you generalize, the more you loose visibility

Whenever you whitelist something, you loose all the visibility on that entity/ that group. Instead of generalizing and whitelisting entire networks, try to whitelist only the necessary hosts or sub-networks. Instead of whitelisting a full range of protocols or ports or all the common ones between two endpoints that are allowed to communicate, whitelist only the necessary ones. Always try to instantiate, never to generalize. With one exception: if it’s too much work to do it this way and the risk presented otherwise is an accepted one.

## #3 Make use of the threshold features

Most of the security tools have their notification/ alerting behavior based on thresholds. Think about thresholds as filter levels that would trigger only for important matters, not for noise. Thresholds can also give you the importance of an alert or a device. Try to understand these features your tool provides and make use of them. For something of more importance to you, an alert or a device, you would assign a higher threshold/ priority or exponential increase value every time it triggers. For something that potentially triggers too many false positives but you still want it logged even if you don’t investigate it immediately, you could assign a low threshold or make it decrease logarithmic every time the alert triggers. Also, the more important the device, the higher his threshold/ priority should be, so it gets to be investigated before anything else.

## #4 Whitelist protocols over ports

If your tool supports whitelisting protocols (and it should), use this against whitelisting by ports. Otherwise, the ports can be used for pretty much any other protocol than the ones your company uses them for and again you may have no visibility what they do with those ports. If you whitelist the port, you get your network exposed to whatever the attacker wants to use it for and you won’t even see it because you whitelisted any action happening on that entry.

## #5 Setup alerts over notifications for critical breaches

If something critical happens, you want to know it now and take actions now. Tomorrow during the work hours might be too late. Avoid setting alerts for what causes false positives though, otherwise the importance of the alerts might diminish.

## #6 Collect as much data as possible for every breach

When an alert is triggered, you want to see as much data as possible that would give you the context of what happened. The alerts should display at the very least your application protocol, your level 2 protocol, the source and destination IP and ports, credentials in usage, data volume for both incoming and outgoing connections and the score of the threat. And anything else your tool can show you.

## That’s it. 

Knowing these you should be able to go to your IDS tool and create a few models customized for your own infrastructure. If it doesn’t seem that straight forward, give it time, it gets better with experience. Oh, and if you need some ideas of custom IDS models you can create for your own infrastructure, check out [7 Ideas of Custom Models for your IDS](https://ramonatame.com/intrusion-detection-systems-custom-models/). 