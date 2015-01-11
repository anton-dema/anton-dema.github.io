---
author: dema
comments: true
date: 2012-01-23 12:06:56+00:00
layout: post
slug: cosa-fare-con-lavvento-di-ipv6
title: 'Cosa fare con  l''avvento di IPv6 '
wordpress_id: 1468
categories:
- riflessioni
tags:
- ipv4
- ipv6
- nmap
- radvd
---

Prima o poi ci si arriva, ad IPv6. E se potremmo anche non accorgerci che qualcosa è cambiato, solo quello scatolotto, con le antennine e con le prese dietro, che ci ha rimandato il nostro provider, tutto cambia dentro le nostre macchine.

Manterremo l'IPv4 all'interno della nostra rete locale, non ci piove, chi se li può ricordare gli indirizzi IPv6.

Lo scatolotto al suo interno avrà un bel [radvd](http://en.wikipedia.org/wiki/Radvd) che staccherà gli indirizzi pubblici IPv6 alle nostre macchine client, il NAT funzionerà per gli host ancora in IPv4 e se il sito che vogliamo raggiungere avrà anche un record AAAA, tanto meglio, [vedremo la tartaruga muovere il collo](http://www.kame.net/).

Però i servizi all'interno delle nostre macchine saranno lì, belli esposti e aperti sul mondo. Ecco perché non appena si passerà ad IPv6 occorrerà fare subito un bel


    
    
    # nmap -6 -n -r -v -p1-65535 -sT fcf7:75f0:82e3:327c:7112:b9ab:d1f9:bbbe
    
    Starting Nmap 5.61TEST2 ( http://nmap.org ) at 2011-12-29 20:40 EST
    Initiating Connect Scan at 20:40
    Scanning fcf7:75f0:82e3:327c:7112:b9ab:d1f9:bbbe [65535 ports]
    Completed Connect Scan at 20:40, 4.38s elapsed (65535 total ports)
    Nmap scan report for fcf7:75f0:82e3:327c:7112:b9ab:d1f9:bbbe
    Host is up (0.00073s latency).
    All 65535 scanned ports on fcf7:75f0:82e3:327c:7112:b9ab:d1f9:bbbe are closed
    
    Read data files from: /usr/local/bin/../share/nmap
    Nmap done: 1 IP address (1 host up) scanned in 4.60 seconds
    Raw packets sent: 0 (0B) | Rcvd: 0 (0B)
    


E se dovesse apparire anche solo una voce "Open" , chiuderla appena possibile.
[+Antonangelo De Martini](https://plus.google.com/106700489171066016161/about?rel=author)
