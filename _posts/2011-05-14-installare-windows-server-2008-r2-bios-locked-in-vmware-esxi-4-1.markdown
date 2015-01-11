---
author: dema
comments: true
date: 2011-05-14 14:22:16+00:00
layout: post
slug: installare-windows-server-2008-r2-bios-locked-in-vmware-esxi-4-1
title: Installare windows server 2008 r2 bios locked in vmware esxi 4.1
wordpress_id: 1383
categories:
- tips
tags:
- bios
- bios locked
- esxi
- oem
- server 2008 r2
- vmware
- windows
---

Un breve tip che penso possa aiutare chi sta cercando di installare windows server 2008 r2 bios locked su virtual machine gestita da vmware esxi 4.1.

I media di installazione bios locked sono dei supporti forniti dal produttore hardware che impediscono l'installazione su altre macchine, facendo un controllo sul bios della macchina. 

Ovviamente se cercheremo di installare il sistema operativo mediante uno di questi media su una macchina virtuale, l'installazione fallirà informando che l'installazione è possibile solo su hardware di $produttore_hardware. 

Un rimedio potrebbe essere quello di usare un media non OEM per l'installazione e poi usare il codice prodotto allegato ai DVD del produttore, ma con vmware esxi è possibile usare un metodo più elegante. 

E' possibile in vmware esxi infatti copiare l'identificativo del bios dell'host fisico su quello della macchina virtuale.
Per configurare una macchina virtuale con questi parametri è sufficiente inserire la seguente stringa alla fine del file .vmx 

SMBIOS.reflectHost = "TRUE"

Possiamo farlo o tramite la console di esxi con vi o tramite vsphere client stesso, accedendo alle configurazioni avanzate della macchina virtuale.
