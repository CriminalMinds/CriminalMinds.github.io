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
