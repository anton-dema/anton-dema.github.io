---
author: dema
comments: true
date: 2009-08-31 08:01:58+00:00
layout: post
slug: fonera-2-0n-recensione-parte-prima
title: Fonera 2.0n recensione parte prima
wordpress_id: 911
categories:
- recensioni
tags:
- fon
- fonera 2.0n
- openwrt
- ralink
- router
- usb
- wifi
- wireless
---

Mi è arrivata grazie a [Michelangelo](http://blog.fon.com/it) la nuova Fonera 2.0n.

Vediamo subito di cosa si tratta.


## Aspetto esterno


[![fonera 2.0n](http://dema.tv/wp-content/uploads/2009/08/3861113939_3c60a4379a.jpg)](http://www.flickr.com/photos/dema/3861113939/)

La nuova Fonera 2.0n è cresciuta nelle dimensioni. Ora misura 16x3x10 cm , circa un 40% in più del volume della precedente Fonera 2.0 .

La porta USB è ora frontale ,  le porte lan passano da 1 a 4 e le antenne diventano 2.

[![fonera retro](http://dema.tv/wp-content/uploads/2009/08/3861114391_e7db0baa72.jpg)](http://www.flickr.com/photos/dema/3861114391/)

Nel pannello posteriore , come si puo' notare , è comparso un interruttore per lo spegnimento della wifi e un bottone del quale non ho capito le funzionalità . Per intuito direi che è un pulsante a doppia funzione , se premuto a lungo effettua un reset dell'unità , se premuto una volta sola attiva la funzione [WPS](http://www.wi-fiplanet.com/news/print.php/3652651) per permettere l'accoppiamento di apparecchi wireless in autoapprendimento WPA/WPA2.

L'alimentazione è fornita da un trasformatore wall plug da 12 volts 1.0A . I consumi dovrebbero quindi essere intorno ai 10 watt.

Nella scatola non è presente la consueta patch ethernet quindi ricordatevi di averne una sottomano prima di aprire la scatola ed effettuare l'installazione altrimenti rimarrete tutta la sera a guardare i led che si accendono senza poter usare la Fonera :P


## Specifiche Tecniche


L'hardware è basato su chip ralink RT3052F a 300 Mhz

[![](http://dema.tv/wp-content/uploads/2009/08/cpuinfo.jpg)](http://dema.tv/wp-content/uploads/2009/08/cpuinfo.jpg)

Il chip in questione fornisce la parte AP in 802.11 b/g/n , 5 porte 10/100 switch e interfaccia per USB OTG, RGMII, SPI, PCM, I2C e UART.

A corredo troviamo un supporto di memoria ram da 64 Mb

[![](http://dema.tv/wp-content/uploads/2009/08/free.jpg)](http://dema.tv/wp-content/uploads/2009/08/free.jpg)

Il sistema operativo è un openwrt kamikaze 8.09.1 pesantemente adattato da Fon , sia per la parte del consueto captive portal/heartbeat , sia per il supporto al chip Ralink , non direttamente supportato da OpenWRT.

L'interfaccia utente si raggiunge via web collegandosi attraverso la LAN cablata o la WLAN Myplace all'indirizzo 192.168.10.1 , la password di default è "admin".

Si tratta di un'interfaccia basata sul [Luci project di freifunk](http://luci.freifunk-halle.net/News) , già vista sulla fonera 2.0.

Di fabbrica la fonera 2.0n era flashata con il firmware 2.2.5.0 flipper enduser , la release stabile al momento in cui sto scrivendo.

Per accedere alla console in ssh ho flashato l'ultima release per sviluppatori , la 2.3.0.0 RC1 DEV che si puo' prelevare da [http://download.fonosfera.org/RC/](http://download.fonosfera.org/RC/) , scegliendo l'immagine compressa in tgz e caricandola direttamente dall'interfaccia web.

Qui la nuova fonerina mi ha subito impressionato , compiendo l'intera operazione di reflash in 3,5 minuti.

I benefici del processore assai più vispo del precedente e forse di una memoria flash più veloce sono molto evidenti .

L'interfaccia web è veloce , reattiva , intuitiva e completa , con settaggi avanzati anche per l'utente più navigato.

Ecco le schermate più significative dell'interfaccia d'amministrazione.


###### Welcome page


[![](http://dema.tv/wp-content/uploads/2009/08/main.jpg)](http://dema.tv/wp-content/uploads/2009/08/main.jpg)


###### admin panel




###### [![](http://dema.tv/wp-content/uploads/2009/08/admin.jpg)](http://dema.tv/wp-content/uploads/2009/08/admin.jpg)




###### firmware update


[![](http://dema.tv/wp-content/uploads/2009/08/firmware.jpg)](http://dema.tv/wp-content/uploads/2009/08/firmware.jpg)


###### Configurazione Plugins (qui nella schermata Twitter plugin )


[![](http://dema.tv/wp-content/uploads/2009/08/luci-twitter.jpg)](http://dema.tv/wp-content/uploads/2009/08/luci-twitter.jpg)


## Impressioni d'uso


Il primo aggettivo  che viene in mente usando la nuova fonera 2.0n è : veloce !

Dall'accensione impiega solo 25 secondi per alzare entrambe le WLAN  , già pronte con il dhcp , il chilli per l'autenticazione e il corretto routing e firewall. E' senza dubbio merito del nuovo processore e della dotazione di ram.

Di fabbrica la Fonera 2.0n è preimpostata con una chiave WPA di 10 caratteri piuttosto complessa ( numeri e lettere , maiuscole e minuscole ) e in modalità mista b/g/n .

Non ho potuto provare per ora le performance della wireless in draft n , in quanto non sono ancora in possesso di un'apparecchiatura wireless 802.11N , ma come per tutti i dispositivi consumer presenti sul mercato non mi aspetto di ottenere un transfer rate maggiore di 50Mbit/s . Purtroppo anche se i nuovi chip fanno progressi non sono ancora in grado di sostenere velocità maggiori.

La portata del segnale è nella norma ed è stabile ed efficace , sia sulla wlan criptata che sulla wlan pubblica.

Una delle novità introdotte dalla Fonera 2.0n è la possibilità di disabilitare completamente la parte radio tramite l'interruttore on/off posteriore oppure semplicemente escludere la wlan pubblica , trasformando la fonera in un qualsiasi router wifi consumer.

Quest'ultima caratteristica secondo me ci dice qualcosa circa la rinnovata filosofia di Fon , forse.

Se ti do la possibilità di disattivare la funzione più importante del mio dispositivo , quella inerente allo sharing fra la community , significa che la mia aspirazione è quella di diventare (anche) un produttore hardware .

E non è una cosa sbagliata, credo . Sul mercato si trovano decine di prodotti simili alla nuova fonera 2.0n  ma quasi tutti con un firmware limitato che offre poche possibilità all'utente smanettone , obbligandolo ad usare software di terze parti.

Questo dispositivo nasce con già a bordo Openwrt , sviluppato e seguito costantemente da un team di hacker di prim'ordine ed è aperto ad ogni personalizzazione. Io dico che se il prezzo retail si assesterà sui 69 euro potrebbe dare del filo da torcere ai big , Netgear e Linksys in prima fila.

Altra grossa novità sono i plugin uploader e il file sharing.

Queste funzioni però meritano una prova approfondita che mi appresto a fare nei prossimi giorni .

Mi aspetto che se messa sotto stress la fonera 2.0n cominci a fare  il fiato grosso ;)

Staremo a vedere !
