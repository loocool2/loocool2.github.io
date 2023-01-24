---
layout: post
title:  "Importance of Using a Registrar with Proper whois Protection"
date:   2023-01-23
categories: rants
introduction: ".tech domains accidentally doxxed me"
read_time: true
---

# Intro 

There are lots of articles about why it's important to have whois protection on your domain if you want to remain anonymous as a site owner, but making sure 

# The problem

You might check public whois repositories and see that it's all fine, but in my case, it wasn't. A friend informed me that I was actually leaking my email address in the dns records when they ran `host -v <domainname.tld> <dns server>`

# The solution

The solution is to move to a completely different domain registrar that actually does anonymize your whois correctly, or register with a burner email. Cloudflare has one of the better domain management systems and conceals whois information well. If you're stuck with a domain registrar because they have a policy of not allowing domain transfers for the first couple months or some such, you may want to keep that domain inactive.