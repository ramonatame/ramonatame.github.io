---
layout: post
title:  "How to Develop Your Own NAC"
date:   2019-08-31
---

## 1. In a Nutshell

If your budget is limited and NACs are found to be expensive, you could try to implement a basic NAC system based on other security tools your organisation employs. Many of the EDR, application whitelisting, domain services, asset management tools and firewalls expose a good API. You would take advantage of the firewall API to serve it a list of allowed/ disallowed devices to connect to a specific network. You would use the other security tools to build a list of requirements for a device to connect to a network and to build a list with the devices that meet those requirements. Finally, you would use your scripting skills to put all these together.

## 2. Steps to build your NAC

Here is a short list of actions to follow when taking this approach:
* make a list with all the security tools you employ, even relevant network and sysadmin tools
* for each tool, decide whether it can give you a list with allowed/ safe devices, either because that device is whitelisted wihin the tool, either because it has a good behavior or simply because it has an agent of that tool on the device so it is monitored
* for each tool, find a way to get the output you need from that tool, by calling its API or by sending requests to its server
* find out how you can setup dynamic firewall rules based on some scripting you will give it as input
* you have the output given by your set of tools, you know how to setup dynamic rules in the firewall, now it's time for scripting, the core part. You will put together all the output from your set of rules and write rules (if-else cases) on which conditions should allow a device to connect to the network and which conditions should reject the device. You will then setup firewall allow rules for the allowed devices and firewall deny rules for the rejected case. 
* send the output of this script (the list of rules) as input to the firewall
* reset the script output periodically or at any new connection to re-verify the conditions the device has to meet

