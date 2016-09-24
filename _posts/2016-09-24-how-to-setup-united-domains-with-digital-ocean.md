---
layout: post
title: How to setup a United Domains Domain with a Digital Ocean Droplet
---

Since I ran into this problem several times, I'm writing it down so I and others don't waste valubale time again.

When hooking up your virtual server or droplet as they are called at Digital Ocean their [manual](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-host-name-with-digitalocean) suggests you first setup Digital Ocean's domain name servers (DNS) at your domain host and then add an A record within the Digital Ocean console.

For United Domains that leads to errors like:

```
Der angegebene Nameserver antwortet nicht autoritativ für Ihre Domain.
```
or

```
Given nameserver does not answer authoritatively for your domain.
```

## Solution
You must first add the A record in your Digital Ocean's console so United Domain's verification process knows that there is a server who can handle requests to your domain. 


