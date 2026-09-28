A SOC, or Security Operations Center, is the team responsible for keeping an eye on an organization's systems and catching threats before they cause damage. Analysts in a SOC don't build products or write code for the business. Their whole job is defense: watching, investigating, and responding.
The work is split into three tiers. Tier 1 analysts are the first line of defense. They monitor incoming alerts and triage them, meaning they quickly sort through what's coming in and decide what actually needs attention versus what can be dismissed. Tier 2 analysts take the alerts that Tier 1 escalates and investigate them in depth, figuring out what happened, how serious it is, and how to contain it. Tier 3 analysts are the most senior. They hunt for threats that automated tools and lower tiers might miss, handle the toughest incidents, and often improve the detection rules and processes the rest of the SOC relies on.
An alert is simply a notification that something unusual happened, like a login from an unfamiliar location or a spike in network traffic. Alerts come from a SIEM (Security Information and Event Management) system, which pulls in log data from across the network and flags patterns worth a human's attention.
Not every alert is a real threat. A true positive is an alert that correctly identifies an actual problem. A false positive is an alert that looks suspicious but turns out to be harmless. A big part of triage is telling these apart quickly so analysts aren't wasting time chasing non issues while a real threat slips by.



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


# Security Logs Notes

## Why logs matter

A log is a written record of something that happened on a computer, like a login or a program starting. Logs are like security camera footage for a computer.

SOC analysts (the people who watch for attacks) rely on logs to spot alerts, figure out what happened, and prove how an attacker got in. If something is not logged, nobody can see it. That is why attackers try to clear logs.

Every log entry helps answer: Who? What? When? Where from? Did it work?

## Windows logs

Open **Event Viewer** to read them. Files are saved as `.evtx` in `C:\Windows\System32\winevt\Logs\`

The three main logs are **Security** (logins and account changes, the most important), **System** (Windows itself), and **Application** (installed programs). Some events only appear if auditing is turned on.

### Key event IDs

**4624:** Successful logon.

**4625:** Failed logon.

**4634:** Logoff.

**4672:** Admin level privileges given to a logon.

**4720:** User account created.

**4732:** User added to a security group (watch for Administrators).

**4740:** Account locked out.

**4688:** New process started.

**7045:** New service installed.

**1102:** Security log cleared (big red flag).

### Reading 4624 and 4625

Check these fields: **Account Name** (who), **Logon Type** (how), **Source Network Address** (which IP), and for failures, **Sub Status** (why).

Common logon types: **2** at the keyboard, **3** over the network, **10** Remote Desktop (RDP).

Common failure reasons: **0xC000006A** wrong password, **0xC0000064** username does not exist.

### Patterns to watch for

Many 4625 events then a 4624 could mean a brute force attack finally worked.

Many 4625 events across lots of accounts from one IP looks like password spraying.

A 4624 with type 10 from an unknown IP means someone may be using Remote Desktop who should not be.

A 1102 means someone is covering their tracks.

## Linux logs

Most logs live in `/var/log/`.

`/var/log/auth.log` (Debian and Ubuntu) or `/var/log/secure` (Red Hat and CentOS) records logins and sudo use.

`/var/log/syslog` or `/var/log/messages` holds general system messages.

Use `last` to see login history and `sudo lastb` to see failed logins. Use `journalctl` to read the systemd journal.

``bash
tail /var/log/auth.log
grep "Failed password" /var/log/auth.log
grep "Accepted" /var/log/auth.log


**"Failed password"** is the Linux version of 4625. **"Accepted"** is the Linux version of 4624. **"sudo ... COMMAND="** shows a command run with admin rights.

## Takeaways

Learn 4624 and 4625 first, then build outward. Log clearing is always suspicious. Windows uses Event Viewer, Linux uses plain text files in `/var/log/`.
