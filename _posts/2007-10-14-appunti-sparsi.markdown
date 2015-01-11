---
author: dema
comments: true
date: 2007-10-14 15:11:29+00:00
layout: post
slug: appunti-sparsi
title: 'Appunti sparsi '
wordpress_id: 119
categories:
- '7.10'
- bluetooth
- compiz
- fon
- gutsy
- macbook
- settimana
- ubuntu
tags:
- '7.10'
- bluetooth
- compiz
- fon
- gutsy
- macbook
- settimana
- ubuntu
---

Settimana tremenda qui . Lavoro in primis e anche sul piano personale bella tosta .

Ho raccolto comunque un po' di cose da scrivere per un post assolutamente della serie who cares. Siete avvisati se avete intenzione di proseguire nella lettura.


#### _La rinascita di turbolearco_


Chi segue questo blog , avrà saputo che settimana scorsa la mia** linuxbox casalinga è morta per cedimento della scheda madre**. Ho provveduto a rimpiazzarla con un nuovo computer decisamente muscoloso e con la scelta di Ubuntu come sistema operativo.

La versione di **Linux per gli esseri umani** scelta è stata la 7.04 Feisty a 64 bit . Non è andato tutto liscio , in quanto la scheda video , una Nvidia 8500 GT non dispone di driver adatti , nemmeno scegliendo i restricted di ubuntu.

Per settare la scheda correttamente , occorre rivolgersi ai [driver proprietari di Nvidia](http://us.download.nvidia.com/XFree86/Linux-x86_64/100.14.19/NVIDIA-Linux-x86_64-100.14.19-pkg2.run)  , con la scocciatura che ad ogni update del kernel bisogna ripetere manualmente la costruzione dei moduli adatti.

Se vi doveste trovare in una situazione analoga , l'howto che mi ha illuminato e guidato passo passo all'installazione dei corretti driver è [qui](http://www.nvnews.net/vbulletin/showthread.php?t=72490) .

Esiste inoltre un altro problemuccio con le versioni a 64bit di Ubuntu ed è il flash player 9 di macromedia.Infatti , non esiste il supporto a 64 bit per il software di adobe per le versioni Linux. Per poter fare in modo che tutto fili liscio , l'howto da seguire è [qui.](http://ubuntuforums.org/showthread.php?t=476924)

Ieri poi ho rotto gli indugi e ho fatto[ l'upgrade a 7.10](http://www.ubuntu.com/testing/710rc) , la Gutsy Gibbon.

[![schermata.png](http://dema.tv/wp-content/uploads/2007/10/schermata1.png)](http://www.ubuntu.com/testing/710rc)

Devo dire che nonostante siamo in release candidate , i passi avanti ci sono eccome. Il **desktop è curato nei minimi particolari** , con [compiz](http://compiz.org/) attivato di default , la [gnome 2.20](http://www.gnome.org/) , la versione di thunderbird 2.0, **nspluginwrapper installato di default** per ovviare al problema flash e 64bit e tante altre chicche. Questo è il risultato del mio giocoso desktop _cubicolare_ , attivato solo per motivi di test . Trovo infatti che in un ambiente produttivo di lavoro , il** desktop in 3d introduce nausee** e mal di mare .

![cubo.png](http://dema.tv/wp-content/uploads/2007/10/cubo1.png)

Sulla parte server , ho attivato come al solito il dominio turbolearco.ath.cx in dyndns , alzato apache e una buona shell ssh  , che però si può raggiungere solo via VPN , oppure in ipv6 all'indirizzo casale.demaitalia.com.


#### _Il problema del Bluetooth su Macbook_


Sempre durante questa settimana d'inferno , mi sono trovato a combattere con un **problema al Macbook****.**

Dopo aver installato Pocketmac for BlackBerry , il macbook ha cominciato a soffrire di **casuale impossibilità di andare in stop**. Se da principio non riuscivo a capire per quale motivo , dopo numerose ricerche , ho scoperto che l'impossibilità di attivare la modalità di sospensione erano dovute ad un **malfunzionamento del modulo bluetooth**. Dal momento che non lo uso , non avrei potuto rendermene conto se non **usando systemprofiler**.

Pensando ad un guaio software , ne ho provate di tutti i tipi : resettato la PMU e PRAM varie volte , niente da fare .Rassegnato a portare il mac in assistenza , ho fatto un ultimo tentativo . Il system check da DVD di ripristino . Procedere è molto semplice .

Per prima cosa bisogna inserire il DVD di Macosx nel lettore del macbook . Riavviare il portatile e premere il tasto C.

Una volta che  il sistema si è avviato da DVD  , scegliere il menù Installer -> Open Disk Utilty e scegliere il disco da controllare nella colonna di sinistra.

Selezionare First Aid e poi Verify Disk per controllare se sono presenti errori e Repair Disk  per correggerli.

Nel mio caso l'utility aveva rilevato alcuni leggeri errori che ha provveduto a correggere.

Beh da quel momento il problema del modulo bluetooth non disponibile è **_quasi_** sparito. Sì , quasi , perchè proprio oggi quando ho iniziato a scrivere questo post dopo sei giorni di uptime e stop e ricariche lisce come l'olio , la maledetta zigzag sull'iconcina del bluetooth è ricomparsa :(

Continuerò nei giorni prossimi a monitorare la situazione , anche se so per esperienza che i casi sporadici e casuali sono di difficilissima risoluzione per i centri di assistenza. Insomma , non vorrei essere obbligato a sostituire il modulo bluetooth per un driver malfunzionante o semplicemente corrotto.


#### _Beta test nel Beta test_


![lama001.jpg](http://dema.tv/wp-content/uploads/2007/10/lama0011.jpg)

Qui devo essere un po criptico. Dopo i francesi che s'incazzano i francesi che socializzano :) Parte oggi un bel programmino di beta test per lo sbloccaggio della Fonera Plus. Io mi cimento e anche [Giorgio](http://www.zarrelli.org/blog/) farà lo stesso , vediamo che salta fuori .

Beh se siete arrivati fino qui vi ringrazio , questa domenica , sono stato un po' preso da suzukimarutite acuta.
