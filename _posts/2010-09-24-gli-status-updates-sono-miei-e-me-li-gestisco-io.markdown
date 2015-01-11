---
author: dema
comments: true
date: 2010-09-24 07:26:44+00:00
layout: post
slug: gli-status-updates-sono-miei-e-me-li-gestisco-io
title: 'Gli status updates sono miei e me li gestisco io. '
wordpress_id: 1276
categories:
- web
tags:
- facebook
- friendfeed
- identi.ca
- microblogging
- social network
- statusnet
- twitter
---

Una delle regioni per cui questo, ma anche moltissimi altri blog non è più aggiornato è l'ormai consolidata affermazione dei sistemi di [microblogging](http://www.microblogging.it/). Quello che prima aveva ragione di essere postato all'interno di un blog ora viene veicolato tramite altri canali. Segnalazioni e quotes su Tumblr o Posterous; stati d'animo, short links e foto estemporanee catturate con lo smartphone su Twitter o Identi.ca; discussioni animate con veloci botta e risposta su Friendfeed (o anche Facebook, al limite, ma anche no..)

Il blog si ritrova quindi abbandonato dal flusso dei pensieri del proprio autore che ci scrive ormai solo approfondimenti o, come nel mio caso, appunti di lavoro da condividere con tutti.

Esiste però un problema di fondo nei comportamenti che ho elencato qui sopra. Tutto quello che scriviamo e lasciamo on line è depositato su server altrui, molte volte senza API efficaci per fare il dump delle nostre conversazioni, insomma, una volta che lanciamo un twit e lo depositiamo nei server del servizio, perdiamo il possesso del nostro contenuto.

E' vero che tramite feed xml possiamo importare in tempo reale e conservare una copia del nostro lifestream, ma con una installazione di [Statusnet ](http://)nel nostro dominio possiamo avere la piena proprietà dei nostri dati.

Per l'installazione di StatusNet sono necessari:
1)PHP 5.2.3
2)MySQL 5.x
3)Apache2 o Lighttpd
In pratica una semplice installazione LAMP. Inoltre sono richieste le seguenti estensioni di PHP:- Curl - XMLWriter - MySQL - GD - mbstring.
Ora, non mi vorrei sbilanciare, ma chiunque abbia un blog hostato dovrebbe avere tutto il necessario, anche se per un fine tuning sarebbe opportuno avere l'accesso ssh. In alternativa si potrebbe comprare un VPS su [Linode](http://www.linode.com/) o su [Amazon](http://aws.amazon.com/ec2/) con tariffe che partono dai 20$ circa al mese.

La soluzione più conveniente è senza dubbio [Dreamhost](http://compl.in/dh2sG1) che con 8.95$ al mese offre uno shared hosting con accesso ssh dalle prestazioni, specialmente lato database, non esaltanti, ma perfettamente adatte al nostro scopo. Dreamhost inoltre fornisce Statunet via [1click install](http://compl.in/cLNVL3) con relativi updates automatici.

Io ho attivato da quasi un mese [cpline.net/ping](http://compl.in/daUAGm) verso il quale faccio affluire tutti i miei status update. Da Cpline Ping tramite Oauth, in tempo reale, lo status update viene pubblicato anche su Twitter e da Twitter viene importato su Friendfeed e a sua volta su Facebook.
[![Status update flow mockup ](http://dema.tv/wp-content/uploads/2010/09/myimage-11.png)](http://dema.tv/wp-content/uploads/2010/09/myimage-11.png)

La conversazione ― sporadica, il mio social mojo è molto basso ultimamente, segno dell'età che avanza, forse ― si snoda principalmente su Friendfeed che possiede l'interfaccia più efficace per discutere un thread.

Io mi trovo bene e sono sereno con i backup giornalieri automatici da Dreamhost verso Amazon S3. Se vi può interessare posso dedicare un post ad un howto passo-passo per installare Statusnet sotto il vostro dominio. Fatemelo sapere nei commenti, nel caso :-)
