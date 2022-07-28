---
title: "Biblioteca TryHackMe"
categories: [ctf]
date: 27/07/2022
description: TryHackMe Biblioteca WriteUp.
---
Hello everyone,

I will explain the solution of the medium level machine as clearly as i work.

<p align="center">
  <img src="https://miro.medium.com/max/620/1*ruqFUGzYnzS0tnhkWbC_zQ.png">
</p>


 1. Scanning
 2.   Enumeration 
 3.  Exploitation 
 4.  Low Privilege Escalation 
 5.   Privilege Escalation

First, after starting the machine and getting the ip, we do a nmap comprehensive nmap scan.
```bash
sudo nmap -sSCV -v -A -T4 -p-
```


- -sS  : syn scan
- -sC : default scripts
- -sV : version info
- -v : verbose (to see the outputs in detail)
- -T4 : time
- -p- : all ports


<p align="center">
  <img src="https://i.hizliresim.com/qalezm4.png">
</p>

Werkzeug http service is running on port 22 (ssh) and 8000.
Let's check the web interface and set the required enumeration
let's do

<p align="center">
  <img src="https://i.hizliresim.com/i293uzf.png">
</p>

We did not encounter any index page, direct login page appeared.
SQLi queries instead of wasting time searching for service-appropriate exploits on Google
Let's see what we get as a response by sending it.

<p align="center">
  <img src="https://i.hizliresim.com/z324dkq.png">
</p>

<p align="center">
  <img src="https://i.hizliresim.com/5f46lbx.png">
</p>

sqli is successful, now i will try to dump sqlmaple database.

```bash
sqlmap -u http://10.10.22.78:8000/login --data="username=admin&password=admin" --batch --dump
```
 

> --data : Data to be received as POST request input
> 
> --batch : default selections
> 
> --dump : db dump


<p align="center">
  <img src="https://i.hizliresim.com/po6s6bz.png">
</p>

Username and password are dumped as clear text.

Now let's try to connect to the ssh service with the credentials we got...


<p align="center">
  <img src="https://i.hizliresim.com/2iyphdd.png">
</p>


Creds fit. We see that we have 2 users in the outputs.

As the main user, we also know that you are not smokey. We somehow named hazel
we need to rise to the user. I went through directories, checked SUIDs, looked in crontabs
but I couldn't get any result. When I ran Linpeas, there was no positive result.
I went through the hazel user directory and there was no id_rsa in the .ssh directory for the link.
The only thing that came to my mind was to brute force the hazel user and find his password.

<p align="center">
  <img src="https://i.hizliresim.com/5b54ie7.png">
</p>

Internet connection. I gave the -t 15 parameter according to the system requirements and vpn will not cause problems.
But even though it took a long time, I couldn't get a result.
It takes a long time to brute force some services.
I thought it wouldn't hurt to try the default passwords while the bf process is going on in the side tab,
because it would take a long time.

![enter image description here](https://i.hizliresim.com/t4vt0wh.png)

The password was very simple.

<p align="center">
  <img src="https://i.hizliresim.com/ij64q8m.png">
</p>

When I read the inside, I saw that you are using the hashlib library.
I tried hijacking the python library here.
However, it was not accepting the hashlib file in the same directory.

When I look back and look carefully
I noticed that I could run it on SETENV environment.

A directory where I can write a file right away, that is /dev/shm.
I made the transition into it, then created my malicious library and
with sudo by setting python path to /dev/shm
I ran the file /home/hazel/hasher.py as root.




<p align="center">
  <img src="https://i.hizliresim.com/oytxfmt.png">
</p>

As a result, privilege escalation was successful.
The command that works in the library I wrote
It allowed me to root.

```bash
cat /home/hazel/user.txt && cat /root/root.txt
```
