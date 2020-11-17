---
title: Tryhackme - Vulnversity
author: nik0
date: 2020-11-14 14:10:00 +0800
categories: [Write up]
tags: [THM,Vulnversity]
---
![Vulnversity logo!](/assets/img/sample/vulnlogo.png)


## Introducción

En este write up veremos la máquina Vulnversity de Tryhackme, veremos temas cómo reconocimiento activo, ataques a aplicaciones web y escalación de privilegios explotando binarios.

## Reconocimiento

Primero haremos un escaneo con nmap para ello utilizamos el comando ```sudo nmap -sC -sV "ip"``` y el output que nos muestra es el siguiente:

```terminal
Starting Nmap 7.80 ( https://nmap.org ) at 2020-11-13 21:03 -03
Nmap scan report for 10.10.128.154
Host is up (0.27s latency).
Not shown: 994 closed ports
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 3.0.3
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 5a:4f:fc:b8:c8:76:1c:b5:85:1c:ac:b2:86:41:1c:5a (RSA)
|   256 ac:9d:ec:44:61:0c:28:85:00:88:e9:68:e9:d0:cb:3d (ECDSA)
|_  256 30:50:cb:70:5a:86:57:22:cb:52:d9:36:34:dc:a5:58 (ED25519)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
3128/tcp open  http-proxy  Squid http proxy 3.5.12
|_http-server-header: squid/3.5.12
|_http-title: ERROR: The requested URL could not be retrieved
3333/tcp open  http        Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Vuln University
Service Info: Host: VULNUNIVERSITY; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 1h40m19s, deviation: 2h53m12s, median: 19s
|_nbstat: NetBIOS name: VULNUNIVERSITY, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: vulnuniversity
|   NetBIOS computer name: VULNUNIVERSITY\x00
|   Domain name: \x00
|   FQDN: vulnuniversity
|_  System time: 2020-11-13T19:04:45-05:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2020-11-14T00:04:46
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 51.43 seconds
```
Primero probé con el puerto 21, pero no habia ningún exploit para esa versión, luego intenté conectarme para ver si tenía acceso anónimo y nada, después seguí con ssh, y tampoco habían exploits disponibles para esa versión, continué con samba buscando recursos compartidos y estaba todo sin acceso con SMBMAP, luego termino con el puerto 3333 que es http y si pongo la ip de la máquina con el puerto me sale esto:

![1](/assets/img/sample/1.jpeg)

Indagando más a fondo en la página (mirando el codigo fuente etc) me di cuenta de que es solo frontend así que empecé a enumerar directorios con gobuster

## Localizando directorios con Gobuster

Con el comando ```gobuster dir -u http://10.10.128.154:3333 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 150``` enumeramos los directorios escondidos que puede tener la página web, OJO: no es recomendable aumentar los threads porque se podría hacer una denegación de servicio o te podría bloquear un firewall, en todo caso con la herramienta W4fw00f se puede saber si un sitio tiene firewall o no, finalmente este es el resultado de Gobuster:

```terminal
2020/11/13 21:43:55 Starting gobuster
===============================================================
/images (Status: 301)
/css (Status: 301)
/js (Status: 301)
/fonts (Status: 301)
/internal (Status: 301)
===============================================================
```
El directorio ```/internal``` se ve interesante y si ponemos esto en la url del sitio nos lleva a esto:

![2](/assets/img/sample/2.png)

Encontramos una subida de archivos, donde podemos subir una shell y así posteriormente tener conexión reversa con el servidor, o un backdoor así que primero vamos a abrir burpsuite para saber que tipo de extension es admitida para poder subir nuestro backdoor

![3](/assets/img/sample/3.png)

ahí subimos el archivo test.jpg y lo interceptamos con burpsuite y ahora lo mandamos a intruder para posteriormente hacer fuzzing al objetivo

![4](/assets/img/sample/4.png)

Luego nos vamos a payloads y cargamos las posibles extensiones que permitirían la subida del archivo, pueden usar la lista que ustedes les guste más

![5](/assets/img/sample/5.png)
