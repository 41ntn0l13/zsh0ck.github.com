---
title: "digitalworld.local:snakeoil WriteUp"
categories: [ctf]
date: 2022/10/19
description: VulnHub digitalworld.local:snakeoil WriteUp.
---
# VulnHub | digitalworld.local : snakeoil

<p align="center">
  <img src="/img/digitalworld.local_snakeoil/snake.png">
</p>


Hello everyone, today I will explain the solution of the box named digitalworld.local: snakeoil on the vulnhub platform.

## Scanning

Let's do an nmap scan first.

<p align="center">
  <img src="/img/digitalworld.local_snakeoil/0.png">
</p>

ssh and 2 web ports are open. Let's look at port 80, its content and fuzzing.


<p align="center">
  <img src="/img/digitalworld.local_snakeoil/1.png">
</p>

## Enumeration 

There is no interesting data on the home page and page source. let's get to the fuzzing thing

<p align="center">
  <img src="/img/digitalworld.local_snakeoil/2.png">
</p>

Since there is nothing here, we will continue from port 8080.


<p align="center">
  <img src="/img/digitalworld.local_snakeoil/3.png">
</p>

"Good Tech Inc." compared with the page named. Let's take a look at the links.


<p align="center">
  <img src="/img/digitalworld.local_snakeoil/4.png">
</p>

It looks like a useful link has been left here.
Since we can't find much, let's do fuzzing


<p align="center">
  <img src="/img/digitalworld.local_snakeoil/5.png">
</p>


<p align="center">
  <img src="/img/digitalworld.local_snakeoil/6.png">
</p>

Since the get method cannot be used on the register page, we will try it with post.


<p align="center">
  <img src="/img/digitalworld.local_snakeoil/7.png">
</p>

It asks us for one username parameter. Most likely, it may also ask for the password parameter.

<p align="center">
  <img src="/img/digitalworld.local_snakeoil/8.png">
</p>

Yes, it asks for a password as it appears.


<p align="center">
  <img src="/img/digitalworld.local_snakeoil/9.png">
</p>

He gave us an access_token for signing up. Now, let's make a request to the /secret part with the username, password and token we have.


<p align="center">
  <img src="/img/digitalworld.local_snakeoil/10.png">
</p>

Something went wrong here. Let's take a look at the page given to us for hint.


<p align="center">
  <img src="/img/digitalworld.local_snakeoil/11.png">
</p>

We need to give the token parameter like this.


<p align="center">
  <img src="/img/digitalworld.local_snakeoil/12.png">
</p>

Here we got our secret key. Now let's look at the /run part.

<p align="center">
  <img src="/img/digitalworld.local_snakeoil/13.png">
</p>

As it seems, we need to put the url and secret key in json format here.

<p align="center">
  <img src="/img/digitalworld.local_snakeoil/14.png">
</p>


<p align="center">
  <img src="/img/digitalworld.local_snakeoil/15.png">
</p>

## Reverse Shell

We've verified it's RCE. Now let's get a reverse shell.

<p align="center">
  <img src="/img/digitalworld.local_snakeoil/16.png">
</p>


<p align="center">
  <img src="/img/digitalworld.local_snakeoil/17.png">
</p>

## Privilege Escalation

Now let's see how to do privilege escalation


<p align="center">
  <img src="/img/digitalworld.local_snakeoil/18.png">
</p>

Since we can run it as root, we have to find patrick's password. I scoured the directories a lot and finally found the password for a file:

<p align="center">
  <img src="/img/digitalworld.local_snakeoil/19.png">
</p>

<p align="center">
  <img src="/img/digitalworld.local_snakeoil/20.png">
</p>

Yes, it was here. If you have any questions or difficulties, you can write to me on telegram.
