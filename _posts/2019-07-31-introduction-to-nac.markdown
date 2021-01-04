---
layout: post
title:  "Introduction to Network Access Control System"
date:   2019-07-31
---

Though Network Access Control (NAC) systems are not as common as IDS and EDR solutions, they are in fact incredibly useful to control who and which devices are allowed to connect to your network, be them BYOD, personal devices or legitimate corporate devices connecting to an unauthorized network.

Therefore, NAC's purpose is to manage access to the corporate network or even access to each of the network segments. They work based on policies, as typically security tools do, and they can also be integrated (inbound) with other tools, such as asset management, firewalls, directory services and so on. If integrated, they can use the information they get as part of the decision making process. An example of a rule within a policy would be: "allow only the devices that have an AV installed to connect to the network X". In reality, these rules can be rather complex, the more granularity is applied to controlling access.

## Configuration

Though NAC systems are often seen as difficult to configure, modern solutions ease this aspect and the effort is similar to configuring a firewall or an IDS system, with more options to allow for fine tuning. They can be either hardware or software appliances. They might have some specific prerequisites for setting them up, so you should check with the vendors.

They are usually placed at the same level or even the same VLAN as the firewall as NACs are often directly connected to the firewall. It is important that the chosen NAC system supports the vendor and firewall type you use.

Most of the NACs provide RESTful API or at least OpenAPI to ease the integration between their product and other security tools you use and they often have some default integrations. Now considering the variety of tools on the market, there is a big chance that whatever NAC you choose it won't be able to integrate with all the requied tools, so you should keep in mind you might need some coding.

## Agent vs Agentless

NAC systems use at least one of the following approaches to collect data from the device based on which it will decide whether the device is compliant with the policies and can connect to the network or not:

* **agent**. Agents are small pieces of software that are installed usually on company owned devices. Most often they require the 802.1X protocol and a RADIUS server.
* **agentless**. These ones do not require software installation on the endpoints at all. They are used for less smart or userless devices or for devices that the company does not own and therefore cannot install even a dissolvable agent on that device. In this case, the common solutions are either to take decisions based on an usual network scan, either by registering that device’s MAC address, although it can be spoofed.
* **dissolvable agent**. These are often called agentless as well though they refer to a piece of code that gets installed temporarily on the user device every time they try to connect to the network. Then it does the checks, reports to the NAC server the results and then immediately auto-removes itself.

## Known vs Unknown Devices

The policies for known devices are usually different than those for unknown devices. Known Devices include any company owned device, no matter the type of connection they use. NAC systems can connect with CA servers as well, so if the client certificate can be recognized by the CA server, this can speed up the decision making.

Unknown devices basically include personal devices, contractors’ devices and guests devices. A common way to use NAC systems for these devices is through captive portals, to be able to authenticate the person connecting to the network as this type of data couldn’t be obtained in an agentless manner. If the device cannot be associated with a certain user, then it could be treated as a “guest” user and give it browsing access only or as limited internal access as possible, preferably none. A combination between captive portals and registering the device seems to be the preferred manner of allowing access to contractors and temporary employees.

## NAC capabilities

When you choose a NAC tool, there are many features you can look for. Depending on their cost, some of them have more or less of this list of features that I consider important:

* be able to manage requests for any segment of the network as different subnetworks have different requirements. It should be able to control access at the subnetwork level rather than as the entry point unless they are only configured to control access to the corporate network
* be able to take decisions for both company owned devices and BYOD (personal devices) if these are allowed to connect to some parts of the network. You don't want to miss visibility on the personal devices traffic.
* support policies based on the connection type (BYOD, VPN, guests/ contractor, wired), device type (server, PC, mobile,…), on the running operating system and most important based on the software running on it and user identity/ role/ ACLs (Access Control Lists). Policies are a set of rules that define authorized and unauthorized behavior and allow the NAC to take decisions. Being able to control access based on the present software means being able to decide whether a device is allowed to connect if it contains some software or more important if it misses some software such as the antivirus you require. Having policies based on ACLs means being able to decide if someone is allowed to connect considering who they are, the job role they have or the team they are part of. I find this feature really useful as it allows a granular control over the networks, no matter the IP the users come from.
* be able to take an autonomous decision given the above list of specifications whether to allow the device, disallow it or quarantine it 
* be able to automatically “repair” compliant devices, but to have the option to postpone or allow manual repair as well. If a device goes to the quarantine network, it will generally be provided with a list of tools that need to be installed or upgraded/ updated on their device to become compliant. It is important to be able to do this manually, but a lot of the employees might not have the knowledge to take care of these, even if they seem trivial. For this reason, having a tool that does it automatically helps. Though you have to make sure it can postpone them for a bit, to avoid interrupting people during their business hours or during important business meetings.
* support both agent and agent-less mode. You can install agents on all devices that you own. But if you allow devices that you don’t own, such as BYOD, contractor devices then you need to be able to interrogate them and take a decision if you would allow them without intruding.
* be able to notify/ alert or support any kind of integration for alerting in case manual take over is needed or there are some anomalies. The more tools you add to your system, the more difficult is to maintain them and to have a broad picture given the data you take from each of them. For this reason, the tool has to be able to integrate with a tool that correlates data from all the different tools you have and to raise alerts if immediate action is needed.

## Policies

ACLs and Policies define the conditions under which someone can connect to a network and the range of actions they are allowed once connected. LDAP and Active Directory can be directly connected to the NAC system and the NAC system can be used to enforce ACLs. Policies can be defined and correlated with ACLs to treat each possible use case. Some decision-making features that policies often include are the following:

* if the device can be authenticated or if the client certificate is valid
* if the device contains disallowed apps or misses necessary agents or apps or contains apps that are not reviewed
* the time the device was inactive
* password compliance
* operating system, ACLs based on user/ job role/ team/ group, device type and connection type

## Which NAC to choose

I came across a few different ones, of which the best ones I found are ForeScout and PulseSecure. They are quite similar and any of them may work for you. To me, ForeScout seemed a bit more mature, PulseSecure seemed to have more future potential. If I were to choose a NAC, I would definitely check out both of these. If budget is an issue, you can always create your own basic NAC (checkout How to develop your own NAC on this website).


## Summary

NAC systems are not as common as firewalls, IDS and EDR systems, but they are as necessary as most of the security tools an organisation already uses. They control which devices and users have access to a specific network and under which conditions. You can buy one or develop a basic one on your own.