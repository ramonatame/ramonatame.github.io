---
layout: post
title:  "7 Ideas of Custom Models for your Intrusion Detection System"
date:   2019-09-16
---

Intrusion Detection Systems come with a predefined set of models they use as patterns to look out for anomalous network traffic. However, every company’s infrastructure is unique so these default models don’t cover everything that might be of interest to you. If you cannot afford the risk of missing visibility in some critical segments of your network, then you have to create your own custom IDS models. This article presents a few recommendations and ideas on custom models you can create for your own IDS.

## #1 Data exfiltration and data corruption for your backup systems

You need to look out for outbound connections from your backup systems to detect data exfiltration and inbound connection to the backups to detect data corruption on them. You should know the flow of the data from the backup systems to any other systems, whitelist those destinations on the ports you use for this flow and alert on any other communication that has the backup systems as the source. Someone could have the intention to mess with your data if they cannot steal it, so you should keep an eye on unauthorized connections to your backup systems, trying to write data on them. You need to find out which systems can write to the backup systems and on which ports and whitelist those, alert on anything else.

## #2 Connections to and from your OOB

Many companies decide to create an out-of-band channel in case they loose control over their infrastructure using the main management paths. You should find out if your company has one and if it does, then you should monitor it. Unless someone is working on the OOB channel (when you should check and dismiss the false alerts), you should alert on any connection to it and from it as the OOB shouldn’t be used unless there is a massive disaster. You want to know who connected to it and where did they try to go next once they hold that connection.

## #3 External connections to your internal networks

Any basic firewall configuration should have a rule to dismiss any connections from external networks to your internal networks. You might want to create a rule with the highest priority possible to alert you when something like this happens. You should look for both ends of the communications, meaning if any of the source or destination of the connection is your internal network and the other end is not.

## #4 Unauthorized database connections

You know which nodes should talk to your database. Hopefully that’s a really small set of nodes on a really small set of ports. When someone reaches your database level, it might be a bit late for your business. At the very least, you might need to involve your best resources and call for external help eventually as it might be your last chance to save your business. You really want to know when this happens.

## #5 Unencrypted traffic

Ideally, all your internal and external traffic should be encrypted. In the real world, you might need to work with protocols that just don’t speak encrypted and you might not have the resources to make them talk over an encrypted channel. In this case, you should look for unencrypted traffic. You should decide a baseline what is allowed in plain text, what is too critical to be allowed, what it already encrypted and should never be seen in plaintext. Alert on anything that stands out or represents too much of a risk.

## #6 Unauthorized protocols on bastions

If you have bastions or jump hosts, they shouldn’t really do much except allowing SSH to critical infrastructure. As such, you should look for any other kind of connections involving them by looking for any other protocols in use. Even more, you can shortlist who should be allowed to SSH and alert on any other connection.

## #7 Unauthorized DR connections

Your DR infrastructure is as important as your backup system which is as important as your production systems. Find out what is the normal behavior baseline for these systems and try to alert on anything that doesn’t match that behavior. You can go vlan by vlan, find out which connections should be allowed, which servers are critical, how can someone escalate/ get into them and then decide what you can alert on.

## #8 Unauthorized Management connections

You definitely have at least a few internal networks used for management purposes of the corporate, production and any other infrastructure you have. You should really take your time to understand how these ones work, what needs to be protected, who should have access to them and how does someone get access to each of the systems and then write your models, case by case, based on the whitelisted sources, destinations, protocols and users at the very least.

## Conclusion

Keep in mind the main rule: Alert on anything that stands out or represents too much of a risk for your infrastructure. That’s it. By implementing models based on these ideas, you gain more visibility on your critical system, customized for your own infrastructure systems. The key to customize any security tools you have is to understand as much as possible your infrastructure, your systems and procedures and then tailor the configuration for your security tools to match the baseline behavior and alert on anything else. If you have more resources, you can always come with more models and you can even do it iteratively and revisit them in a few months for example.

If you want to read a few recommendations to follow when creating the custom models, please visit [How to Create Custom IDS Models](https://ramonatame.com/how-to-create-custom-ids-models/). 