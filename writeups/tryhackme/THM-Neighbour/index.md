---
title: "Neighbour WriteUp"
categories: [ctf]
date: 2022/11/19
description: TryHackMe Neighbour WriteUp.
---
# TryHackMe | Neighbour

<p align="center">
  <img src="/img/neighbour_thm/0.png">
</p>

Hello everyone, in this write-up i will explain the solution of the machine named "Neighbor" in tryhackme.

## Scanning

First we do nmap scan

<p align="center">
  <img src="/img/neighbour_thm/1.png">
</p>

Ports 80 (web) and 22 (ssh) are open.

Let's check the website.

## Enumeration

<p align="center">
  <img src="/img/neighbour_thm/2.png">
</p>

We came across the login page. It says something is wrong with the page source. Let's see.

<p align="center">
  <img src="/img/neighbour_thm/3.png">
</p>

It says we can login with guest:redacted credentials.

<p align="center">
  <img src="/img/neighbour_thm/4.png">
</p>

We caught the login process with Burp. Let's burn this.

<p align="center">
  <img src="/img/neighbour_thm/5.png">
</p>

It takes the agent parameter "?user=agent". So the username we gave

## Exploitation

<p align="center">
  <img src="/img/neighbour_thm/6.png">
</p>

Let's see what happens if we change the user to admin

<p align="center">
  <img src="/img/neighbour_thm/7.png">
</p>

Yes, there seems to be an IDOR vulnerability caused by misconfiguration.
