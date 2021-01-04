---
layout: post
title:  "Guidelines to Configure Your EDR"
date:   2019-08-02
---

## 1. Introduction

An Endpoint Detection and Response (EDR) system is similar with a personal anti-virus (EDR). However, instead of managing each computer individually, you get a central console (server) and a software you need to install on each computer (endpoint, client). This software is called agent or sensor and it will talk directly to the console: it will grab a list with malware signatures, it will grab policies (lists with rules mentioning which behavior is allowed for programs and how to address unallowed behavior). The sensor will also let the console know of any actions taken on the endpoint where it resides. If a sensor does not talk with the console in a specified amount of time (heartbeat-like behavior), then the console knows the sensor is no longer active and that endpoint is not protected anymore.

## 2. How it works

An EDR, as any endpoint tool, is usually based on the Agent Programming Paradigm, meaning that it has:

* a Server/ Management Console which defines the behavior of the EDR for each of the endpoints, using policies and application whitelisting/ blacklisting.
* an agent (named “sensor”) running on each of the endpoints; it does all the practical work of the EDR on the particular endpoint where it is installed, independent of what happens on other endpoints. It connects periodically to the Server component to get any updated requirements and definitions and to let the server know about any incidents and that the sensor itself is still alive on that endpoint.

## 3. Decision Taking

It takes a decision if that behavior should be allowed, based on:

* a series of metrics named TTPs
* configured policies
* application reputation

The decisions an EDR can take usually vary depending on the tool, but you would want to look for these ones: terminate the application, do not terminate the application but deny the action, allow the action, allow the action but log it.

## 4. Whitelisting and Blacklisting

You would also want to be able to whitelist applications on top of the policies set. Some endpoint EDR tools apply the policies on top of whitelisting, meaning even if you whitelist a tool, if it does something unusual, its execution will still get blocked. You do not want that. You do not want loads of false positive. You should be able to whitelist/ blacklist on top of policies based on: process path, process reputation, certification used to sign the process, hash, parent process, parent certifications. In this way, you should be able to avoid getting legitimate processes blocked, such as the unsigned ones used by Windows or drivers. If you don’t have these capabilities and you want to lock the processes down, you will have a lot of nuissance. If the tool supports wild cards, you want to be sure you don’t use them when whitelisting certificates and as much as possible you will avoid using them when whitelisting applications by path. In fact, you should try to avoid them at all costs, otherwise you will leave holes you will forget and not maintain. You could also benefit by having the possibility to automatically blacklist applications that have a malicious score above a threshold you predefine.

## 5. Reputations

Each application will have a reputation associated with it. If the process is trusted, then the reputation is good. If it is untrusted, the reputation is bad. The exact implementation varies on the tool you use. You would find it useful to be able to set policies based on the reputation.

## 6. TTPs

TTPs are metrics used by an endpoint EDR to determine if a process behaves anomalous or it has patterns that identify with malicious reasons. They can also be seen as descriptors of the process behavior that led to raising an alert.

Some TTPs you can look for: buffer overflow, beaconing, code drop, company blacklist, compromised process, code injection, memory snapshot/ dump of another process, email client behavior though non-email app, enumerate process, impersonating app, attempting to transfer files over network, file-less script, dynamic code, hidden processes, input injection into another process, modify permissions of another process, dropping a new app, connection with a peer of low reputation, attempt to control a Windows Service, using non-standard ports, attempt to monitor device inputs, attempt to start a less trusted app etc.

## 7. Policies

A policy will be applied to each endpoint. In that policy you will define the behavior of the EDR on that sensor, meaning you will tell it which applications or behaviors to allow, which to deny, which to terminate and so on.

Shortly, the policy is used to check the consequences for the behavior an application has. For example, if the application behaves as a malware, adware or untrusted app/ blacklisted app you might want to terminate it or deny its actions. You might want to be able to set policies also based on the application behavior, path, hash, reputation, parent, parent hash, parent reputation and so on.

## 8. Operations

You should be able to choose what happens with an application depending on the kind of operations it does. You may look for the following:

* behaving as a malware/ adware
* communicating across the Internet
* trying to modify the memory or the execution of another process
* calling a process that is less trusted
* trying to execute scripting or code interpreters

## 9. Deny vs Terminate

The difference between these two is mainly if you would still allow that process to run or not. The deny value denies the specific operation the process does at some point, but it allows it to continue its execution even if it altered that bit. The terminate will kill the process. You may want to terminate the process when you’re sure they are highly malicious and whenever you can in general as this is the safest option. On the other side, if the process is legitimate, by choosing to terminate it, you will seriously affect that application and eventually block it during critical business moments.

## 10. Other Features

You should look for the following settings and features as well:

* time or interval to update the tool and to update the signatures
* paths to be completely overlooked though this should be avoided at all costs because you will not have visibility on them
* seeing and alerting on misconfiguration such as sensors getting deactivated on some systems
* some basic templates for policies to help getting you started
* a way to notify you immediately if something of interest happens (based on actions, TTPs, the alert rating)
* a way to quarantine devices at request and automatically if malicious behavior is identified already running and not have been stopped because of any reasons
* eventually a way to get yourself into that device without infecting the rest of the network (by still keeping it isolated)
* you should be able to see details of the event with as much detail as possible; at the very least, the device name, the logged in user, the application name, the parent application name, the date and time, the reputation, the reason of raising the alert and so on

## 11. Conclusions

These should be enough to get you started to configure your EDR properly. Once configured, your EDR should be good for handling operations and if you chose a good EDR it may help you quite a bit in monitoring the events and correlating them for you. Otherwise, you may need extensive effort to triage the events. If you find yourself in the latter case, you could definitely use having a SIEM tool to correlate what your EDR says with what else happens in your environment and highlight chains of events that seem potential dangerous.