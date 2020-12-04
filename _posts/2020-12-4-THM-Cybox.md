---
title: Tryhackme - Cybox (CTF UPSA 2020)
author: nik0
date: 2020-11-14 14:10:00 +0800
categories: [Write up]
tags: [THM,Vulnversity]
---
![CYBEX](/assets/img/sample/cybox/CYBEX.png)

## Introducción

En ese write up, veremos la máquina creada por [@takito1812](https://twitter.com/takito1812?s=20) Sirve para Virtual box y VMware, yo lo hice en VMware y usé tryhackme para poder conectarme a la máquina, aquí les dejo el código de la room: ```1337ctfupsa2020cybox``` muy entretenida máquina, la verdad me tuvo un rato peleando con los dominios que habían etc, pero eso lo aclaramos más adelante ;)


## Datos importantes

Para este CTF es muy importante tener en cuenta el email del administrador y el dominio de la página, se pueden encontrar en el footer de la página.

Email: admin@cybox.company
Dominio: cybox.company

## Añadir dominio a /etc/hosts

Primero necesitamos añadir a /etc/hosts la ip y el dominio del sitio

Ej: <ipMáquinaCybox> <dominio>
  
## Enumerando subdominios con gobuster

con el comando ```gobuster vhost -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt --url http://cybox.company/```

Encontramos los siguientes subdominios: 

![1](/assets/img/sample/cybox/1.png)

los subdominios encontrados los añadimos al /etc/hosts para luego poder ingresar a cada uno de ellos y poder interactuar con ellos

esto se ve en: ftp.cybox.company
![2](/assets/img/sample/cybox/2.png)

esto se ve en: monitor.cybox.company
![3](/assets/img/sample/cybox/3.png)

esto se ve en: dev.cybox.company
![4](/assets/img/sample/cybox/4.png)

esto se ve en: register.cybox.company
![6](/assets/img/sample/cybox/6.png)

esto se ve en: webmail.cybox.company
![5](/assets/img/sample/cybox/5.png)

## Encontrando la vulnerabilidad

Primero nos metemos a los subdominios y vemos que en register.cybox.company hay un sistema para crear usuarios, en este caso me creo un usuario llamado "nadie"
y luego que le damos a create, nos da un correo nadie@cybox.company y unas credenciales "nadie:nadie"

![7](/assets/img/sample/cybox/7.png)

Ahora nos vamos a monitor.cybox.company y vemos un panel donde hay un login, procedemos a registrarnos con el correo que anteriormente obtuvimos, una vez registrados le damos click a "forgot password" e ingresamos nuestro correo, en este caso sería "nadie@cybox.company"

![9](/assets/img/sample/cybox/9.png)

Luego nos llegará al correo un mensaje para poder cambiar la contraseña, para esto vamos a webmail@cybox.company, nos logueamos con las credenciales que nos dieron en register.cybox.company

![10](/assets/img/sample/cybox/10.png)

Luego clickeamos el link:
![11](/assets/img/sample/cybox/11.png)

Vemos que se puede cambiar la contraseña, aunque si nos fijamos en la URL ```http://monitor.cybox.company/updatePasswordRequest.php?email=nadie@cybox.company``` en la parte del usuario "nadie" la podemos cambiar por el email del admin para ingresar una nueva contraseña, esto es un fallo de programación, entonces la URL quedaría así ```http://monitor.cybox.company/updatePasswordRequest.php?email=admin@cybox.company```

Ahora tenemos que cambiar la contraseña del admin, a cualquiera que nosotros queramos

Luego volvemos al panel y nos logueamos cómo admin:

![13](/assets/img/sample/cybox/13.png)

Si todo salió bien, deberíamos haber ingresado cómo Admin:

![14](/assets/img/sample/cybox/14.png)

Ingresamos al admin panel y nos sale esto:

![15](/assets/img/sample/cybox/15.png)

Vemos el código fuente:

![16](/assets/img/sample/cybox/16.png)

Observamos un link ```styles.php?style=general``` clickeamos y nos sale esto:

![17](/assets/img/sample/cybox/17.png)




