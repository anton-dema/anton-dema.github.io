---
author: dema
comments: true
date: 2008-07-01 09:18:15+00:00
layout: post
slug: sono-giorni-un-po-cosi
title: 'Sono giorni un po'' così '
wordpress_id: 230
categories:
- linux
- sysadmin
tags:
- 802.1x
- linux
- port athentication
- port trunking
- sysadmin
- vlan
---

Tipico esempio di post per riempire il blog  e per dire : " Vedi che anche se non ho un c*** da dire scrivo lo stesso !". Mi sa che sto cominciando a riconoscermi [tra le righe scritte dal Tagliaerbe.](http://blog.tagliaerbe.com/2008/06/dieci-modi-per-uccidere-il-traffico-verso-il-vostro-blog.html)

In questi giorni , mi sto rompendo la testa per riorganizzare la Lan di una grossa azienda alimentare della zona. Il cliente ha richiesto funzioni abbastanza avanzate come il **port authentication 802.1X** , il web filtering e molteplici VPN a diversi livelli di accesso.

Grazie alla potenza di linux e al software open source si riesce a fare tutto con agilità , quello che mi preoccupa maggiormente è che il **cliente ha chiesto di poter amministrare** e cambiare i parametri a suo piacimento. Questo comporta una conoscenza , almeno di base , di Netfilter , Squid , Freeradius e di tutti i software installati. Una via potrebbe essere di fornire a corredo Webmin , che grazie ad un'interfaccia amichevole permetterebbe di amministrare il sistema ma al tempo stesso potrebbe creare disastri in mani poco esperte (oltre a tirarsi dietro problemi di exploit per conto suo !)

E' un nodo spinoso che temo possa essere risolto solo con un'attenta analisi delle policy che il cliente intende governare e con la costruzione di tools appositi.

Vi faccio sapere poi come è andata .

Nel frattempo la **richiesta di consulenza** per un eventuale security test a chi ritengo essere persona valida e competente non ha ricevuto nessuna risposta dopo due email. Un " Mi spiace , ma non sono interessato" sarebbe stato sufficiente !
