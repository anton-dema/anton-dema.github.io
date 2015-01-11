---
author: dema
comments: true
date: 2006-12-29 15:04:39+00:00
layout: post
slug: il-collegamento-seriale-per-la-fonera
title: Il collegamento seriale per la fonera
wordpress_id: 1555
categories:
- fon
- fonera
- fonera hack
- hack
- hacking la fonera
- max232
- serial console
- ttl
---

Molto è stato scritto sulla possibilità di avere una console seriale per poter accedere alla fonera , senza l'hacking del form injection della gui web.
Mi sono accorto però che tutti i tutorial sono scritti in inglese ed alcuni , sebbene molto completi , non prevedono un troubleshooting in caso di difficoltà da parte dei meno smaliziati.
Ho deciso quindi di scrivere questo piccolo tutorial per il collegamento via console seriale ed eventuali procedure di resetting/debricking.

<!-- more -->

**Primo Disclaimer :** Se non siete tanto pratici , non provate a farlo , anche se è tutto abbastanza semplice , tutto quanto è descritto in questo post , richiede un po di pratica con i comandi *nix/bsd.

**Secondo Disclaimer** : se vi ritrovate con un grazioso mattoncino bianco tra le mani dopo aver seguito quanto qui scritto, non ritenetemi responsabile , tutto quello che andrete a fare è sempre e solo sotto vostra unica responsabilità

L'uscita prevista dalla fonera è del tipo ttl , una sorta di seriale su 12 volt , una porta molto usata un  tempo su molti controller o sulla quasi totalità dei PLC. Ora tutti i suddetti controlli logici si sono evoluti e tutti escono di fabbrica con la loro bella presa usb e il loro bel software che come al solito gira solo su macchine Win.

Ma veniamo al nostro connettore 10 pin TTL sulla fonera . Eccone una foto in dettaglio .

[![piedinatura](http://dema.tv/wp-content/uploads/2006/12/337287463_d8a743bd44.jpg)](http://www.flickr.com/photos/dema/337287463/)

Notate che la connessione per lo scambio dei dati è posta al secondo e terzo piedino , mentre al quinto è posta la massa (negativo) Esiste anche un'uscita a 3,3volts ed è posta al quinto piedino della seconda file , immediatamente dietro il piedino di massa.
I due condensatori posti immediatamente dietro alla piedinatura ,vi danno l'idea di come è stata presa la foto , ossia dal fondo dove sono posti i led di controllo verso l'alto , dove c'è la porta rj45 e il bocchettone d'antenna per intenderci.
Io in questo tutorial non prendo in considerazione l'alimentazione interna della fonera , dal momento che ho comprato un max 232 converter da 5volts che quindi ha necessità di essere alimentato con una fonte esterna.
Il convertitore max 232 l'ho acquistato su ebay , pagandolo decisamente una follia , rispetto al valore della fonera . Per risparmiare , l'alternativa è costruirselo da soli , e allora il costo dei componenti non credo possa superare i 5 euro. Ma io sono decisamente imbranato per l'elettronica e quindi ho proceduto all'acquisto . Magari potrò ammortizzarlo per lo sblocco di fonere in cambio di un pugno di euri :).
Ecco la foto del mio convertitore max 232 a 5volts

[![schedina](http://dema.tv/wp-content/uploads/2006/12/337287320_484737e0ec.jpg)](http://www.flickr.com/photos/dema/337287320/)

La descrizione della corrispondenza dei quattro fili esposta in foto è relativa al modello in mio possesso .Ovviamente fate riferimento allo schema elettrico del vostro convertitore per i collegamenti.
Per collegare la basetta alla porta della fonera ho usato un connettorino a 10 pin , sradicato da una scheda firewire fulminata che sembra proprio fatto apposta.

[![fonerars232](http://dema.tv/wp-content/uploads/2006/12/337287184_a1e7ddb059.jpg)](http://www.flickr.com/photos/dema/337287184/)

Ho poi collegato i fili ( di molto avvolticciolati come si dice dalle mie parti) come illustrato qui sotto

[![fili](http://dema.tv/wp-content/uploads/2006/12/337286950_ffb24764c7.jpg)](http://www.flickr.com/photos/dema/337286950/)

Bene , quando tutto è pronto , dotiamoci di un cavo seriale e colleghiamolo ad un computer.

Ora per stabilire una connessione di console seriale abbiamo vari programmi , a seconda del nostro sistema operativo.
Per windows , useremo hyperterminal , con questi settaggi :
Bit per secondo= 9600
bit di dati =8
parità =nessuno
bit di stop =1
Controllo di flusso=nessuno

Per macosx io uso di solito Zterm che ovviamente va settato con gli stessi parametri di sopra , per linux potete usare gtkterm.

Ora tutto è pronto , collegato e si spera funzionante , possiamo accendere la fonera ed osservare i messaggi che appariranno nella nostra console.


<blockquote>+PHY ID is 0022:5521
Ethernet eth0: MAC address 00:18:84:1b:b9:a8
IP: 0.0.0.0/255.255.255.255, Gateway: 0.0.0.0
Default server: 0.0.0.0

RedBoot(tm) bootstrap and debug environment [ROMRAM]
Non-certified release, version v1.3.0 - built 16:57:58, Aug 7 2006
Copyright (C) 2000, 2001, 2002, 2003, 2004 Red Hat, Inc.
Board: ap51

RAM: 0x80000000-0x81000000, [0x80040450-0x80fe1000] available
FLASH: 0xa8000000 - 0xa87f0000, 128 blocks of 0x00010000 bytes each.
== Executing boot script in 1.000 seconds - enter ^C to abort</blockquote>


La fonera continuerà il booting , fino a che ci ritroveremo di fronte al classico prompt di linux


<blockquote>BusyBox v1.1.3 (2006.08.17-19:56+0000) Built-in shell (ash)
Enter 'help' for a list of built-in commands.

_______  _______  _______
|   ____||       ||   _   |
|   ____||   -   ||  | |  |
|   |    |_______||__| |__|
|___|

Fonera Firmware (Version 0.7.1 rev 1) -------------
*
* Based on OpenWrt - http://openwrt.org
* Powered by FON - http://www.fon.com
---------------------------------------------------
root@OpenWrt:~#</blockquote>


A questo punto , possiamo avere un controllo completo sul  nostro router , possiamo infatti decidere di aprire la porta 22 editando il file firewall.user come descritto da giorgio [qui](http://www.zarrelli.org/blog/index.php/2006/11/23/hacking-la-fonera-step-by-step-guide/) , possiamo aggiungere delle regole di port forwarding di nostro piacimento , possiamo evitare che fon aggiorni la nostra fonera da remoto etc etc.

Insomma , costruire un cavo seriale per avere una shell sulla fonera può fare dannatamente comodo , dal momento che qualora perdeste ogni segnale di vita , a causa di bad flash , oppure di uno smanettamento nei file di init o qualsiasi altro malestro combinerete , potrete sempre recuperarla.

In particolare , ecco alcune istruzioni per recuperare una fonera incriccata dura e affetta da kernel panic.

Se durante il boot della fonera nella vostra console appare quanto segue :


<blockquote>Code: 8c920028 00809821 02408021 <8e020018> 8e030010 30420006 00621825 1460002a 00000000

Kernel panic: Attempted to kill init!</blockquote>


o qualcosa di simile , ma sempre maledettamente minaccioso , fate un passo indietro . Spegnete e riaccendete la fonera e quando vedrete al prompt


<blockquote>== Executing boot script in 1.000 seconds - enter ^C to abort</blockquote>


Affrettatevi a premere CTRL+C . Così entreremo nel menu di redboot e provvederemo a riflashare il gingillo.

Per fare tutto questo , abbiamo bisogno di un server tftp da montare sul nostro laptop o computer che dir si voglia.  Illustrerò la procedura per Mac osX , ma puo essere portata su tutti i sistemi operativi , usando il software opportuno.

Per Macosx , prendiamo un server tftp molto facile ed intuitivo [tftpserver](http://ww2.unime.it/flr/tftpserver/) (che fantasia) che altro non è che una gui per il server tftp gia incluso in macosx.

Una volta sistemato il server tftp , dobbiamo preparare un'immagine da flashare nel dispositivo . Scarichiamo quindi l'ultima immagine ufficiale del firmware FON da [qui](https://en.fon.com/downloads/downloads.php). Ilfile si dovrebbe chiamare fonera_0.7.1.1.fon.  Per creare un bel tar.gz che asserve ai nostri scopi dobbiamo tagliare i primi 519 bytes del file e per fare questo , usiamo il seguente comando :

`dd if=fonera_0.7.1.1.fon of=fonera.tar.gz bs=519 skip=1
`

Prendiamo il file risultante fonera.tar.gz e decomprimiamolo nella directory /private/tftpboot.

Ritorniamo alla nostra console seriale e dal prompt di redboot digitiamo quanto segue
`
RedBoot> ip_addr -h 192.168.6.2 -l 192.168.6.100/24
IP: 192.168.6.100/255.255.255.0, Gateway: 0.0.0.0`

Abbiamo dato indicazione dell'indirizzo ip del nostro server tftp ossia 192.168.6.2 (che puo' essere qualsiasi altro indirizzo ip del vostro laptop)  e forniamo un indirizzi ip a la fonera 192.168.6.100.

Colleghimo il cavo ethernet alla fonera e scarichiamo l'immagine dal nostro server tftp digitando quanto segue

`RedBoot> load -r -b %{FREEMEMLO} rootfs.squashfs
Using default protocol (TFTP)
Raw file loaded 0x80040800-0x801c07ff, assumed entry at 0x80040800
`

a questo punto creiamo un nuovo rootfs con il seguente comando`:`

`RedBoot> fis create rootfs
An image named 'rootfs' exists - continue (y/n)? y`

``
Facciamo reboot e siamo a cavallo.
Credits:



	
  1. [L'articolo recovering a bricked fonera by tiagra](http://log.tigerbus.de/?p=89)

	
  2. Giorgio Zarrelli con il suo [post](http://www.zarrelli.org/blog/index.php/2006/11/23/hacking-la-fonera-step-by-step-guide/)  per  attivare l'accesso ssh editando i files firewall.user e thinclient e quant'altro necessita.

	
  3. [Jauzsi](http://jauzsi.hu/2006/10/13/inside-of-the-fonera) per la piedinatura ttl della fonera.

	
  4. Il grande Giuseppe "Cidio" che mi ha guidato nei collegamenti elettrici e procurato il connettorino a 10 pin


