---
author: dema
comments: true
date: 2012-10-26 11:20:09+00:00
layout: post
slug: font-mangiucchiati-arch-intel-945
title: Font mangiucchiati su Arch Linux con Intel 945
wordpress_id: 2227
categories:
- appunti
tags:
- arch
- font mangiucchiati
- fonts
- linux
- workaround
- xorg
---

Se usi linux (arch nel mio caso), se hai una scheda grafica Intel così identificata da lspci 

    
    00:02.0 VGA compatible controller: Intel Corporation Mobile 945GM/GMS, 943/940GML Express Integrated Graphics Controller (rev 03)
    


e vedi i font mangiucchiati, devi aggiungere questo file nel path /etc/X11/xorg.conf.d .

    
    nano -w 10-debugwait.conf
    Section "Device"
        Identifier "Intel"
        Driver "intel"
        Option "DebugWait" "true"
    EndSection
    



Riavvia il server grafico X e niente più font mangiucchiati.

[+Antonangelo De Martini](https://plus.google.com/106700489171066016161/about?rel=author)
