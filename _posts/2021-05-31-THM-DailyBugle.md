---
title: Tryhackme - Daily Bugle
author: nik0
date: 2021-05-31 23:38:00 +0800
categories: [Write up]
tags: [THM,DailyBugle]
---

![XD](/assets/img/sample/DailyBugle/0.png)


## Introducción

En este write up, veremos temas cómo vulnerabilidades SQL, crackear hashes y escalar privilegios via yum.

## Reconocimiento

Utilizaremos la herramienta Nmap para este cometido con el comando ```sudo nmap -sS -sV -n --script=http-enum IP```, también podríamos utilizar el scrip

![XD](/assets/img/sample/DailyBugle/1.png)

Podemos observar que hay tres puertos abiertos, 


