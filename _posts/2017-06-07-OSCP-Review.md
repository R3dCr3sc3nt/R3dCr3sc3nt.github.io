---
layout: post
title: PWK/OSCP Review
tags: [infosecrambling, certifications]
---

For the past 60 days, R3dCr3sc3nt has been taking a break from CTFs and Vulnhub VMs to develop their hacking skills in another way: the [Penetration Testing with Kali Linux course](https://www.offensive-security.com/information-security-training/penetration-testing-training-kali-linux/) offered by [Offensive Security](https://www.offensive-security.com/).

Before I get into a review of the course, here is a bit of background about myself. I have ~6 years of professional experience working as a software engineer and sysadmin. Prior to OSCP, I had never touch a Windows command prompt, or ever worked professionally in a security context. In November 2016, I began taking the [Coursera cryptography course](course://www.coursera.org/learn/crypto). In December 2016, I attended [33C3](https://events.ccc.de/congress/2016/wiki/Main_Page) and played in my first CTF. In March 2017, I pwned my first [Vulnhub](https://www.vulnhub.com/) VM. My lab time for PWK began on April 1, 2017.

## Overview

There are 3 components to the PWK course: a PDF, lecture videos, and the lab network. The PDF and lecture videos are the main educational materials provided by Offensive Security, though don't be fooled. Significant outside research is required to pwn all of the machines in the lab network. The lab network contains ~50 servers in several different subnets, all of which have multiple vulnerabilities. After working through the training materials, I was able to gain root/administrative privileges on 44 of the 50 machines in a 60 day period. Towards the end of this period, I took and passed the OSCP exam.

## Preparation and Helpful Links
Playing in Capture the Flag competitions and doing Vulnhub VMs were excellent preparation for PWK. They put you in the right mindset and get you thinking like a hacker. I started working on CTF-like challenges at [RingZeroTeam](https://ringzer0team.com/) and [OverTheWire](http://overthewire.org/wargames/), and signed up for real CTFs on [CTFTime](http://ctftime.org/). Shortly after creating an account there, I was contacted by [@charix](https://github.com/charix46) and we formed [R3dCr3sc3nt](https://ctftime.org/team/32761) and started competing together.

For Vulnhub VMs, the [Kioptrix series](https://www.vulnhub.com/?q=Kioptrix&sort=date-asc&type=vm) provide a gentle introduction to pwning, thought sometimes it's a little tricky to get these running because the images are quite old. A more comprehensive list of machines that machines that are similar to the PWK lab servers is maintained by the the NetSec Focus community [here](https://docs.google.com/spreadsheets/d/1dwSMIAPIam0PuRBkCiDI88pU3yzrqqHkDtBngUHNCw8/edit#gid=0). I personally didn't invest very much time in rooting these VMs prior to the beginning of PWK, but I did find reading the walkthroughs to be very insightful. They helped me a sense of the typical enumeration steps taken in a pentesting scenario.

Finally, it's important to get plugged into the right communities. Start reading the [netsec subreddit](https://www.reddit.com/r/netsec/) and join the [NetSec Focus room](https://netsecfocus.slack.com) on Slack. Once registered for PWK, request to be added to the #oscp channel.

Coming from a software background, I was concerned that my networking and Windows knowledge wouldn't be enough to be successful in this course. For the networking, I found the [wikipedia article on CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) to be helpful in explaining how IP address blocks work. For Windows, lots of googling was required. Here are a list of links that were particularly useful references for me:
- [Fuzzy Security Windows Privilege Escalation Fundamentals](http://www.fuzzysecurity.com/tutorials/16.html)
- [Windows pentest commands](http://www.networkpentest.net/p/windows-command-list.html)
- [Windows Privilege Escalation Methods for Pentesters](https://pentest.blog/windows-privilege-escalation-methods-for-pentesters/)
- [Becoming NT AUTHORITY\SYSTEM on Windows](https://security.fnal.gov/cookbook/LocalSystem.html)
- [Stored Credentials](https://pentestlab.blog/2017/04/19/stored-credentials/)

For Linux, my prior work experience, combined with the CTFs and vulnhubs proved to be adequate preparation. During the course, these links also came in handy.

- [Basic Linux Privilege Escalation](https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/)
- [linuxprivchecker.py](http://www.securitysift.com/download/linuxprivchecker.py)

There are a veritable plethora of resources available to people interested in learning about security topics more broadly. The last resource that I'll mention is a list of resources that might be a little easy to get lost in.

- [NetSec Focus Learning Resource](https://docs.google.com/spreadsheets/d/1TD8KTRXvXwy1yU6s7Nz_JuNh7b7fa7pINZuHOVjtAAg/edit#gid=745476740)

## Praise

The breadth of material covered in this course is impressive. To get an idea of all of the topics covered, take a look at the [course syllabus](https://www.offensive-security.com/documentation/penetration-testing-with-kali.pdf). It took me 2 50-hour weeks to work through all of the content and exercises, and I kept re-visiting it throughout the course. The discussion of buffer overflows is extremely thorough and easy to understand, a true feat for such a complex and multi-faceted topic. Much of the Windows-centric content was also new to me, but the lab guide did a awesome job of getting me up to speed on the details of SMB vulnerabilities and file transfers.

The lab environment is also second to none. The scope of vulnerabilities available for exploit is as broad as the content in the lab guide, giving you ample opportunity to practice various exploits and techniques. As mentioned earlier, I purchased 60 days of access to the lab environment, which is the recommended amount of time. I only worked during the week (40-50 hours each week) on the course content and the labs, keeping my weekends free for other activities. This was the perfect amount of time for me to get through all of the course material and pwn most of the lab machines. Had I worked through a weekend or two, I'm confident I could have rooted every machine.

When exam time came around, I was definitely nervous but felt well-prepared. After 18 straight hours of hacking, it wasn't looking good. I took a brief nap, and then woke up with a brilliant insight. In the end, I snagged enough points to pass. In my opinion, some of the lab machines were actually harder than the exam machines. The most challenging part of the exam is the 24 hour time limit, but with a little persistence and a "Try Harder" attitude, anyone who has worked through the course content and rooted most of the labs should be able to get a passing score.

## Criticism

My criticisms of the course content and the labs fall broadly into 3 categories: old applications and OSes, treatment of passwords, and the forums.

We live in an exciting time for information security. Even some non-technical people have heard about [Shellshock](https://en.wikipedia.org/wiki/Shellshock_%28software_bug%29), [Heartbleed](https://en.wikipedia.org/wiki/Heartbleed), or [WannaCry](https://en.wikipedia.org/wiki/WannaCry_ransomware_attack). Unfortunately, in the course content there wasn't mention of any modern technique or vulnerability from the past 4 years. In the labs, almost all of the operating systems were at least 4 years old, and most were much, much older. The vulnerable versions of the applications hosted on these machines were also extremely old. I've never been employed professionally in information security, but having worked at several large tech companies, it's extremely unlikely that one would encounter these OSes and applications in 2017 in the wild. On the other hand, it is the primary job of a penetration tester to exploit vulnerabilities in old software and kernels, but several years behind on a patch is a bit ridiculous.

Throughout the course, there also seemed to be a reliance on uncovering passwords and brute-forcing logins. The trends in information security have been moving away from simple passwords for quite a while, to key-based authentication schemes, or passwords with an additional factor (e.g. time-based or HMAC-based one-time passwords). Further, most systems that do require only a simple password for access typically have a hard limit on the number of failed login attempts before locking that account or blocking the offending IP address (à la [fail2ban](http://www.fail2ban.org/wiki/index.php/Main_Page)). No mention of this was made anywhere in the course material, or demonstrated in any of the labs, much to the course's detriment.

Finally, a note about the forums. Offensive security hosts a forum where students can discuss course content and lab machines. For the most part, the forums are a helpful resource, but, plainly stated, they are chockfull of spoilers for the lab machines. There is some moderation of the forums by Offensive Security staff, but it doesn't go far enough in my opinion. The spoilers undermine the learning that comes from struggling, an important feature of this course. An over-reliance on the forums could spell disaster for students striving for the OSCP certification. My advice would be to avoid them entirely. For non-spoiler hints/gentle nudges, you can contact the Offensive Security support staff instead.

When I received the email from Offensive Security confirming that I had passed the exam, they notified me that they would also send along a printed certificate with my name on it. The correct spelling of my name contains a non-ascii character (ñ). I indicated this in an email reply, and their response was that they could not accommodate any non-ascii characters in the printed name on the certificate, which is kind of a bummer.

## Conclusions

Overall, I found the course content and exam to be fun, addictive, challenging and rewarding. However, it wasn't the life-changing, brain-melting experience that I had heard/read about elsewhere. Maybe this is because I didn't have to balance the demands of a full-time job while taking the course. Maybe it's because the CTFs had primed me to expect the worst. Either way, it was a good experience that I would recommend to anyone interested in breaking into the security field or simply interested in hacking.
