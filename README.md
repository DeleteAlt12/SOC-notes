A SOC, or Security Operations Center, is the team responsible for keeping an eye on an organization's systems and catching threats before they cause damage. Analysts in a SOC don't build products or write code for the business. Their whole job is defense: watching, investigating, and responding.
The work is split into three tiers. Tier 1 analysts are the first line of defense. They monitor incoming alerts and triage them, meaning they quickly sort through what's coming in and decide what actually needs attention versus what can be dismissed. Tier 2 analysts take the alerts that Tier 1 escalates and investigate them in depth, figuring out what happened, how serious it is, and how to contain it. Tier 3 analysts are the most senior. They hunt for threats that automated tools and lower tiers might miss, handle the toughest incidents, and often improve the detection rules and processes the rest of the SOC relies on.
An alert is simply a notification that something unusual happened, like a login from an unfamiliar location or a spike in network traffic. Alerts come from a SIEM (Security Information and Event Management) system, which pulls in log data from across the network and flags patterns worth a human's attention.
Not every alert is a real threat. A true positive is an alert that correctly identifies an actual problem. A false positive is an alert that looks suspicious but turns out to be harmless. A big part of triage is telling these apart quickly so analysts aren't wasting time chasing non issues while a real threat slips by.

##Networking Fundamentals for SOC Analysts

## IP Addresses
An IP address is the address of a device on a network. Private IPs only work inside a local network, common ranges are 10.x.x.x, 172.16.x.x, and 192.168.x.x. Public IPs are unique across the internet and reachable from anywhere.
Why it matters: internal devices suddenly talking to unknown public IPs is worth a second look.

## Ports
A port is like a room number for a service running on a device. Common ones: port 22 SSH (remote login), port 53 DNS (domain lookups), port 80 HTTP (unencrypted web), port 443 HTTPS (encrypted web), port 445 SMB (Windows file sharing), port 3389 RDP (Windows remote desktop).
Why it matters: ports 22, 445, and 3389 are common brute force targets, meaning someone repeatedly guessing a password. Watch for repeated failed attempts.

## TCP/IP
The basic rulebook for devices talking to each other. TCP is reliable and starts with a three step handshake, SYN then SYN-ACK then ACK, before sending data. UDP is faster but unreliable, no handshake, used for things like streaming and DNS.
Why it matters: a flood of SYN requests with no completed handshake can signal a SYN flood attack.

## DNS
DNS turns domain names like google.com into the IP address computers actually use. It's the phone book of the internet.
Why it matters: attackers abuse DNS through tunneling (hiding stolen data in DNS traffic), spoofing (redirecting a device to a fake IP), and beaconing (malware checking in with an attacker's server through repeated DNS lookups).

## Normal vs Suspicious Traffic
Normal: steady traffic to well known services, expected activity between known devices, traffic matching typical business hours.
Worth investigating: traffic to unfamiliar countries, repeated failed logins, sudden spikes in outbound data, connections to strange or new domains, unusual protocols in the wrong place.
The mindset: you're learning what normal looks like so anything unusual stands out.

## Quick Summary

IP address, unique address of a device on a network.
Port, numbered channel a service uses to send or receive data.
Protocol, agreed rules for communication, like TCP, UDP, HTTP, or DNS.
Packet, a small chunk of data sent across a network.
Handshake, the setup process TCP uses before sending data.
DNS resolver, the service that looks up a domain and returns its IP.
Beaconing, malware regularly checking in with a remote server.
Exfiltration, data stolen and sent out of a network without permission.
Brute force, repeatedly guessing a password to break in.
Spoofing, faking the source of traffic to look trustworthy.

