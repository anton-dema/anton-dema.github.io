---
author: dema
comments: true
date: 2007-10-30 09:05:38+00:00
layout: post
slug: il-mio-approccio-a-leopard-who-cares-series
title: 'Il mio approccio a Leopard (who cares series ) '
wordpress_id: 135
categories:
- apple
- bluetooth
- hibernation
- leopard
- mac
- macbook
- suspend to ram
- update
- upgrade
tags:
- apple
- bluetooth
- hibernation
- leopard
- mac
- macbook
- suspend to ram
- update
- upgrade
---

![](http://dema.tv/wp-content/uploads/2007/10/1734271592_6faa890f42_o.jpg)

L'hanno fatto in molti , di postare le prime impressioni d'uso del nuovo OS di casa Apple.

Anche io preso da una irrefrenabile smania di consumismo avevo pre-ordinato il dvd presso l'apple store e venerdì 26 Ottobre , puntuale come l'orologio atomico di [Mainflingen](http://www.dcf77.com/) , il cofanetto era sulla mia scrivania.

Ho aggiornato Tiger al volo **senza nessuna precauzione** , pulizia di applicazioni , backup di dati , solo schiaffato dento il dvd e via. Dopo un'oretta e mezza , **Leopard era lì in bella mostra sul mio macbook**.

La prima impressione , lo devo ammettere **è stata negativa** . Le applicazioni si aprivano molto più lentamente e addirittura gli **effetti exposè creavano degli artefatti nelle animazioni**. Poi , il modulo bluetooth , che come chi mi segue sa , già dava problemi , ha smesso completamente di funzionare.

Ho provveduto quindi ad un **reset della PMRAM e NVRAM** e anche se il bluetooth è definitivamente andato , il sistema si è subito svegliato  ed ha cominciato a comportarsi come si deve.

Non mi addentrerò sulle incompatibilità riscontrate , cosa che tra l'altro sta affrontando anche Gioxx egregiamente [sul suo blog](http://gioxx.org/2007/10/29/aggiornamento-a-leopard/) , ma mi soffermerò sul **fottuto bluetooth**.

Ora è manifesto che si tratta di un problema hardware . Un problema probabilmente dovuto ad interferenze interne , o anche solo ad un **cavetto allentato** . Ma la riparazione comporta il disassemblaggio del notebook e portarlo in assistenza e vedersi cambiare la logic board con conseguente salasso finanziario non è nelle mie intenzioni. E poi , chi lo ha mai usato il bluetooth !

Il grosso problema che causa la perdita del modulo , è che il laptop si rifiuta di andare in stop . Chiudi il latch , e dopo 15 secondi invece di attivare il suspend to ram , il computer si rianima.

Per ovviare a questo problema ho cambiato i parametri di sleep del sistema. Infatti se invece di attivare un **(safe) suspend to RAM** , si istruisce di fare un dump della RAM su HD (hibernation) , si riesce finalmente a mettere il laptop in stop.

Ecco la stringa da dare su terminale per attivare l'ibernazione su disco


<blockquote>$ sudo pmset -a hibernatemode 1
$ sudo nvram "use-nvramrc?"=false</blockquote>


Dopo un reboot , quando si chiude lo schermo , il laptop entra in stop . Quando si vuole continuare a lavorare , occorre premere il pulsante power e attendere circa una trentina di secondi per avere il resume.

Sì , lo so è un workaround fastidioso , ma non mi va di spendere nell'assistenza Apple , e sono troppo _faint-hearted_ per provare a [ripararlo da solo](http://www.ifixit.com/Guide/Mac/MacBook/Bluetooth-Antenna/86/21/).
