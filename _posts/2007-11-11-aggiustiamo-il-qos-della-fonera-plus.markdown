---
author: dema
comments: true
date: 2007-11-11 09:42:24+00:00
layout: post
slug: aggiustiamo-il-qos-della-fonera-plus
title: 'Aggiustiamo il qos della Fonera plus '
wordpress_id: 139
categories:
- bandwidth shaping
- fon
- fonera plus
- hack
- net neutrality
- qos
tags:
- bandwidth shaping
- fon
- fonera plus
- hack
- net neutrality
- qos
---

La Fonera plus possiede un discreto qos a bordo . E' possibile cambiare infatti la quantità di banda da mettere a disposizione per la wlan pubblica attraverso la user-zone , muovendo la levetta sul valore desiderato.

![](http://dema.tv/wp-content/uploads/2007/11/immagine-2.png)

E' possibile restringere il valore fino ad un minimo di 512 Kbs  che è mediamente accettabile per un utente ADSL medio.

Ma se si posiziona la fonera in un sito particolarmene affollato , con molti foneros connessi al nostro hotspot  , e magari abbiamo stipulato con il nostro provider adsl un contratto a consumo , questo puo' essere un problema.

La soluzione è liberare la nostra fonera plus e ottimizzare il qos per restringere ulteriormente la banda da condividere.

Dobbiamo editare il file _/etc/config/qos_ . Ho zippato il file per un conveniente download, potete prelevarlo da [qui](http://www.zshare.net/download/47821928c6ff49/).

Ecco i parametri che dovrete cambiare :
[sourcecode language='cpp']# QoS configuration for OpenWrt
# INTERFACES:
config interface hotspot
option classgroup	"Default"
option enabled		0
option upload    	512
option download		512
option device		tun0

config interface wan
option classgroup  "Default"
option enabled      0
option overhead 	1
option upload       128
option download     1024[/sourcecode]
Apriamolo con il nostro editor preferito ed assegniamo il valore di banda desiderato. Inoltre dal sample file che avete scaricato , mettete a 1 il parametro _option enabled_.   Ricordate , tun0 hotspot è la rete FON_AP pubblica , wan è il gateway verso la rete internet.Una volta impostati i valori desiderati , occorre caricare il file sulla fonera plus.Questo dovrebbe essere sufficiente per raggiungere i nostri scopi. Sfortunatamente , l'hack per liberare la fonera corrompe alcuni files di init , fra cui il trigger per qos .

Nessun problema , andiamo su _/etc/rc1.d_ e creiamo un link simbolico allo script _S50qos_ posto in _/etc/rc.d_ .


<blockquote>ln -sf /etc/rc.d/S50qos S50qos</blockquote>


Diamo un reboot alla fonera plus e da questo momento possiamo dire di appartenere al club dei nemici della Net Neutrality. Possiamo dispoticamente rallentare , bannare o reindirizzare ogni pacchetto vogliamo :)

Io ho assegnato alla wlan pubblica 100kbs che sono più che sufficienti per la navigazione web e anche per una chiamata Voip . Guardare un video o ascoltare una radio in streaming potrebbe essere un problema , ma credo che un fonero dovrebbe essere responsabile , e non dovrebbe saturare la banda altrui , anche senza la limitazione del qos.
