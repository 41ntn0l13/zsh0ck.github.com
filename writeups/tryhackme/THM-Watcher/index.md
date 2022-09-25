---
title: "Watcher WriteUp"
categories: [ctf]
date: 2022/07/22
description: TryHackMe Watcher WriteUp.
---
# TryHackMe | Watcher

Hello everyone, in this article, i will explain the ctf solution named watcher on the tryhackme platform.

<p align="center">
  <img src="/img/watcher_thm/rsz_cover.jpg">
</p>

First we do an nmap scan.

<p align="center">
  <img src="/img/watcher_thm/0.png">
</p>

Ports 21, 22 and 80 are open.
We are looking at the web page as there is no old version.

<p align="center">
  <img src="/img/watcher_thm/1.png">
</p>

Since there is no interesting link or button, we will do directory enumeration with fuzz.

<p align="center">
  <img src="/img/watcher_thm/2.png">
</p>

Let's check that robots.txt is live

<p align="center">
  <img src="/img/watcher_thm/3.png">
</p>

There is one interesting file. let's see the content

<p align="center">
  <img src="/img/watcher_thm/4.png">
</p>

We can't see it because it gives 403. so we don't have permission.

Let's go back and examine the homepage source

<p align="center">
  <img src="/img/watcher_thm/5.png">
</p>

Line 80 has interesting file and parameter. Let's send a request by executing the lfi payload here.

<p align="center">
  <img src="/img/watcher_thm/6.png">
</p>

We can read the /etc/passwd file. so there is lfi. let's try to read the previous file
 
<p align="center">
  <img src="/img/watcher_thm/7.png">
</p>

ta daa! We found the ftp files location and the ftpuser user password.

Let's connect via ftp and see what's up.

<p align="center">
  <img src="/img/watcher_thm/8.png">
</p>

We see that we have permission to write to the files folder. Since the web page is written with php, we can place a php reverse shell here.

<p align="center">
  <img src="/img/watcher_thm/9.png">
</p>

Now let's listen with netcat and run the shell we placed on the server.

<p align="center">
  <img src="/img/watcher_thm/10.png">
</p>

We can use the find command to browse where the flags are located.

```bash

find / -name "flag*" 2>/dev/null

```

<p align="center">
  <img src="/img/watcher_thm/11.png">
</p>

In order to do the privilege escalation part in order, we need to proceed as in the picture above.

toby --> mat ---> will ---> root

<p align="center">
  <img src="/img/watcher_thm/12.png">
</p>

We saw that www-data user has permission to run commands with toby user authority, and by using this, we obtained a bash session on toby.

then let's see what there is to rise to the mat user.

<p align="center">
  <img src="/img/watcher_thm/13.png">
</p>

As cronjobs, mat user can run cow.sh file and we have permission to write to this file. let's use this

<p align="center">
  <img src="/img/watcher_thm/14.png">
</p>

<p align="center">
  <img src="/img/watcher_thm/15.png">
</p>

next user will user.

<p align="center">
  <img src="/img/watcher_thm/16.png">
</p>

We can increase the privilege of the above file by using sudo over will with mat.

<p align="center">
  <img src="/img/watcher_thm/17.png">
</p>

Since it has write permission to cmd.py file, we placed a reverse shell in it and let's create a connection by listening.

<p align="center">
  <img src="/img/watcher_thm/18.png">
</p>

we got the connection. Now let's see how we become root.

As seen above, the user belongs to the "adm" group.

<p align="center">
  <img src="/img/watcher_thm/19.png">
</p>

When we search the files belonging to the group, we see that there is a base64 encoded key. we think this is the last point because for ssh connection it can be root id_rsa file.

<p align="center">
  <img src="/img/watcher_thm/20.png">
</p>

Let's solve this and try to root our machine

<p align="center">
  <img src="/img/watcher_thm/21.png">
</p>

That was it. Although the road was long, it was a good practice :)
