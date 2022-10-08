---
title: "pWnOS 2.0 WriteUp"
categories: [ctf]
date: 2022/10/08
description: VulnHub pWnOS 2.0 WriteUp.
---
# VulnHub | pWnOS 2.0

<p align="center">
  <img src="/img/pwnos2-0_vulnhub/0.png">
</p>


Hello Everyone

Today I will explain the solution of the machine named pWnOS 2.0 on the vulnhub platform.

Let's do nmap scan first

<p align="center">
  <img src="/img/pwnos2-0_vulnhub/1.png">
</p>

Ports 22 and 80 are open. let's access the web page

<p align="center">
  <img src="/img/pwnos2-0_vulnhub/2.png">
</p>

The web page consists of these. login and register. Now let's try directory fuzzing.

<p align="center">
  <img src="/img/pwnos2-0_vulnhub/3.png">
</p>

The directory named blog exists now let's go here

<p align="center">
  <img src="/img/pwnos2-0_vulnhub/4.png">
</p>

We came across the blog page. Ordinary posts seem to be shared and when we look at the page source

<p align="center">
  <img src="/img/pwnos2-0_vulnhub/5.png">
</p>

We see that "Simple PHP Blog 0.4.0" is running. Let's do a research to see if there are any exploits about it.

<p align="center">
  <img src="/img/pwnos2-0_vulnhub/6.png">
</p>

There is also 1 RCE exploit written in perl and an exploit written in ruby embedded in the metasploit framework.

I'll show you how to use the first one manually.

<p align="center">
  <img src="/img/pwnos2-0_vulnhub/7.png">
</p>

When we run the script, it informs us about usage. I used the second option and uploaded web shell for rce.

<p align="center">
  <img src="/img/pwnos2-0_vulnhub/8.png">
</p>

Now we can get a reverse shell from here.

<p align="center">
  <img src="/img/pwnos2-0_vulnhub/9.png">
</p>

Let's see what we can find now.

<p align="center">
  <img src="/img/pwnos2-0_vulnhub/10.png">
</p>

I found a php file like this after some browsing. First I went to the rabbit hole and then I found it. Let's connect with mysql and see what's up

<p align="center">
  <img src="/img/pwnos2-0_vulnhub/11.png">
</p>

I don't think this will do much. Let's use this password to connect via ssh

<p align="center">
  <img src="/img/pwnos2-0_vulnhub/12.png">
</p>

Ta Daa! It was here. If you have any questions, you can reach me on Telegram.
