---
layout: post
title: "How Does The Zero-Trust Architecture Work"
date:   2022-06-20
description: "Zero-Trust is such a buzzword these days and yet there is little documentation online on how to build one. After reading the Zero Trust Networks book by Gilman & Barth (which I recommend 100%), I decided to share my notes. Hopefully you'll find them useful!"
tags: ["Featured"]
---

Hi everyone! It's been a long time since I haven't written anything, so hopefully this blog post makes up for it. Zero-Trust is such a buzzword these days and yet there is little documentation online on how to build one. After reading the Zero Trust Networks book by Gilman & Barth (which I recommend 100%), I decided to share my notes on what I considered the most important ideas in the book and my thoughts blended altogether in this article, should anyone not have the time to go through the entire book. Hope you enjoy it and you find it useful!

## THE ZERO-TRUST MODEL

The Zero-Trust Model is based on the assumption that your internal network is hostile/ compromised or simply not to be trusted, which could be caused by:
* internal threats, such as gruntled employees 
* significant incidents in the past, which you cannot say for sure whether they were eradicated
* improperly designed network, which is either not properly segregated, segmented or simply left too open
* your network resides in a public cloud environment, which you don't trust
* etc.

You can think of the Zero-Trust model as an abstractization applied on top of your network, that, given that the network is not trusted, tries to secure and verify all the communcations, no matter where they occur in your internal network/ between which 2 systems. Given that it's an abstractization, it can work alongside the Perimeter model and complement it. However, if you have a mature Perimeter-based model and you don't have the problems mentioned above, then it's likely that migrating away from it and towards Zero-Trust is not worth it, cost-wise. If you do have the problems above, it may mean that your Perimeter-based model is not the best and you could benefit at least from a hybrid Permiter & Zero-Trust if not entirely from Zero-Trust.
You have to know that the Zero-Trust model is not meant to protect you from APTs or even privileged employees. It's not meant for industries like finance, which experience an advanced level of attacks. But most companies do not really get to experience this level of attacks. If you assessed who your attackers are they are not APTs and you don't worry extremely about your privileged employees, then Zero-Trust can be the right solution for you.
Zero-Trust is largely based on configuration management, as it is only feasible and scalable when fully automated. The devices, just like the users and applications have to be authenticated and authorized. If sniffing is a concern, then encryption is needed too and it's often in the shape of certificates. Authentication in a Zero-Trust is usually based on certificates, too. 
In a Zero-Trust model you mainly care about 3 things:
* the user/ application authentication. Applications are seen as similar to users
* device authentication. You don't authenticate just the user, you authenticate (and trust) the device from where the request comes, too.
* the notion of trust. See Trust Score further down.

And finally, there are 2 main components when building a Zero-Trust model, which are further detailed:
* The Control Plane
* The Data Plane 

<figure>
	<img src="{{ '/assets/img/zero-trust.drawio.png' | prepend: site.baseurl }}" alt=""> 
	<figcaption>Zero-Trust Architecture</figcaption>
</figure>


## THE CONTROL PLANE
The Control Plane handles the requests. It authenticates user or application and the device and then manages the requests for access to protected resources. It is ultimately an authorization system, which will either allow or deny the request.
To authorize the request, a trust score is computed for it, based on the trust the user/ app and the device have. Then the trust score plus other relevant data are compared with a policy and decide based on it if the request is allowed. If the request is allowed, this is often in the format of short-lived certificates or leased access tokens. At a deeper level, this is the component that enforces other measures, such as encryption. Encryption is essential if it is assumed that the internal network is not to be trusted and it is often in the shape of certificates. Having authorization, authentication and encryption ensures that each network flow is authentication and expected.
The security of the Control Plane itself is, of course, essential. 
The Control Plane has 3 isolated components, which will be further detailed:
* The Enforcer
* The Policy Engine
* The Trust Engine


### THE ENFORCER
The Enforcer is typically a very lightweight component that receives requests, asks the Policy Engine if they should be allowed and then enforces the response it received.
The Enforcer should exist in large numbers and should be placed as close as possible to the workload, to ensure low latency. It can either be a proxy, a load balancer, a firewall or it can be a client application installed directly on the systems.
In a mature Zero-Trust design, the Enforcer would apply also on the Zero-Trust services (such as on the Control Plane itself).

### THE POLICY ENGINE
The Policy Engine decides if a request should be allowed. 
Here is how it works:
* it receives a request from the Enforcer
* then asks the Trust Engine to compute the Trust Score for the request and get some other useful data
* it pulls the right Policy for the request
* then it decides if the request should be allowed, based on the Policy and the response it received from the Trust Engine.

A policy is simply a set of conditional statements under which the request is allowed/ denied. It's worth mentioning that the policies can be layered (broader policies, then more and more granular ones) and they should be fine-grained. Given that this can place a burden, the effort should be distributed across teams - each system owner maintains the policies for that system. The impact the policies would have on the network flows should be monitored for some time, for fine-tuning, and not enforced immediately.
Finally, the Policy Engine is modeled as a service, with its own CI/CD pipeline: it stores the policies in a version controlled system and every time a policy is updated, it is promoted up through the CI/CD pipeline. 

### THE TRUST ENGINE
The Trust Engine is a service used by the Policy Engine for risk analysis and it delivers 2 things:
* a Trust Score in a numerical format
* metadata about the user/ application and the device making the requst, ie. outside working hours + different resources than usual +  different location, which all together might indicate some risk

These 2 deliverables are often bundled as one in a lightweight JSON response. (The book calls this JSON "the agent", but personally I do not agree with this terminology - I would call the Enforcer an agent, given that it's a lightweight client app).
The Trust Engine works like this:
* it gets the request
* pulls data about the user/ app and the device from the Data Stores
* computes the Trust Score
* creates the JSON
* sends the JSON back to the Policy Engine

It's worth mentioning that the score should be exposed to end-users, so the cannot maneuvre it. Also, the Trust Engine usually includes conditional logic to handle unknown attack vectors too.


#### THE TRUST SCORE
The Trust Score is purely a number that associates a level of trust with a user, app or a device. If having a trust score for user, app, device seems contradictory for you when discussing a Zero-Trust Networks system, recall that it's the networks that you don't trust, not these entities. You have to trust something for authorization purposes. 
There are 2 main problems a trust score system has to solve:
* sourcing a trust score when there is no trust
* managing trust

##### Sourcing Trust: 
When there is no trust, you need to have a mechanism to source it from somewhere. A common method is trust delegation. Trust Delegation is kind of a transitory relationship: you trust component A, component A spins up component B, so you trust component B. An easy example is trusting a new system because you trust the provisioning system. And about that, a new system is more trusted than one that hasn't been imaged/ rotated for years (less change to be already compromised).
Having a way to source Trust allows us to build scalable and automated systems and to operate without too much human intervention.
Trust may need to be sourced also if the score is too low for some action. Some methods for this are having management approval, visit IT department, additional MFA push etc.

##### Managing Trust:
Unlike firewalls work, Zero-Trust Networks think of trust as a moving target. Trust is dynamic and it can increase/ decrease based on both previous and current actions, much like in real life, so you need to have a way to re-evaluate the trust. You either re-evaluate the trust for each request, or based on a schedule. If scheduled, you might run in the situation where you handle a request differently than you would after the next schedule run.
To compute the trust score, you start by computing a trust score per user/ per app and per device, and then you use an aggregate formula to return a single trust score. You use any data source that you have and it's relevant for the trust score, such as LDAP, certificates, UBA, historical actions, location (but never by itself), request frequency etc.

## THE DATA PLANE
The Data Plane is used mainly to store all the data about the user/ app and the device in a persistent storage, so it can be queried in the future, to form an agent. The Data Plane has just one component type: the Data Stores.
A few things worth mentioning:
* it's very likely that you'll have significantly less data for autonomous systems than you have for humans
* certificates can provide useful data too: a device certificate could include data like the device type, the OS etc.; a user certificate could give you the user role
* there may be some PII you have to manage, by storing user data

### THE DATA STORES
Think of the Data Stores as the source of truth for the current and past state of the system: it holds the inventory databases and the historical databases. The inventory databases are just asset inventory databases and can be built using the configuration management. The historical databases hold anything worth storing about the user/ app or the device, that would influence the trust score, such as patterns, timing, frequency, accessed resources etc. There could be a database of expected flows, too.
The Data Stores are modeled as a service and interact with the Trust Engine. The effort to build it might have to be distributed across the organization.

## LIMITATIONS
The Zero-Trust design does come with a few limitations, which you should be aware of:
* DDOS attacks are not prevented and you should use a specialised system for that
* Although it's using encryption, privacy is not guaranteed: ie. you can still see which package is rooted from where to where, even if you cannot see the package content
* Social engineering should still be addressed outside this system
* And most importantly: invalidation of trust occurring before the policy is pulled. Should you need to revoke the trust for an agent, you can only do it next time the enforcement component checks the policy, not in between, given that it's a polling system. Pushing invalidation adds significant complexity, hence polling is generally preferred for Zero-Trust.

## GETTING STARTED WITH ZERO-TRUST
If you're just getting started with the Zero-Trust model, there are a few things to keep in mind. First, only move to Zero-Trust if it makes sense (see the very first section). Secondly, start small. You don't need to move all your internal networks and network flows at once. Choose a small scope, increment over it, get your system ready and only then scale it to the rest of the networks. Thirdly, if you don't have an asset inventory, use your configuration management to build one and if you don't authenticate devices, start doing it.  Forthly, whenever covering a new network, don't enforce the policies immediately. You should allow for a long period of time just to log the network flows, monitor how the policies would behave and fine-tune them. Otherwise, you'll end up with frequent situations when someone has to intervene, which will make the system fail altogether.
