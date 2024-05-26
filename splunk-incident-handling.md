---
description: 05/25/24 - This room covers an incident Handling scenario using Splunk.
---

# Splunk - Incident Handling

## Intro

A Big corporate organization Wayne Enterprises has recently faced a cyber-attack where the attackers broke into their network, found their way to their web server, and have successfully defaced their website http://www.imreallynotbatman.com. Their website is now showing the trademark of the attackers with the message YOUR SITE HAS BEEN DEFACED as shown below.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/dcc528c218e8dda78504f55f58188575.png)

They have requested "**US**" to join them as a **Security Analyst** and help them investigate this cyber attack and find the root cause and all the attackers' activities within their network.



The good thing is, that they have Splunk already in place, so we have got all the event logs related to the attacker's activities captured. We need to explore the records and find how the attack got into their network and what actions they performed.



Note: All the event logs that we are going to investigate are present in `index=botsv1`

## Lab

### Reconnaissance

We will start our analysis by examining any reconnaissance attempts against the webserver `imreallynotbatman.com`

<figure><img src=".gitbook/assets/image (7).png" alt=""><figcaption><p>index="botsv1" imreallynotbatman.com</p></figcaption></figure>

This query shows all traffic logs to the web server. Logs including `imreallynotbatman.com` come from 4 sources:

<figure><img src=".gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

Lets analyze the `stream:http` source, using the following query:

`index="botsv1" imreallynotbatman.com sourcetype="stream:http"`

<figure><img src=".gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

There's are two different source IPs recorded. Let's explore HTTP logs from these IPs, starting with 40.80.148.42:

`index="botsv1" imreallynotbatman.com sourcetype="stream:http" src_ip="40.80.148.42"`

This log looks like a possible SQL injection. We know for sure that this IP attacked the server, but first, we will find how they did their reconnaissance

<figure><img src=".gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

Let us check suricota logs:

`index="botsv1" imreallynotbatman.com sourcetype="suricata" src_ip="40.80.148.42"`

<figure><img src=".gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

[Acunetix](https://www.acunetix.com/) is a web app scanning software

### Exploitation



