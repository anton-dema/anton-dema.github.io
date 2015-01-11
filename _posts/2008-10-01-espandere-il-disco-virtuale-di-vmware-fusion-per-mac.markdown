---
author: dema
comments: true
date: 2008-10-01 11:08:32+00:00
layout: post
slug: espandere-il-disco-virtuale-di-vmware-fusion-per-mac
title: Espandere il disco virtuale di vmware fusion per Mac
wordpress_id: 428
categories:
- howto
tags:
- fusion
- gparted
- mac
- virtual
- vmware
- windows
---

La scorsa settimana Paco mi chiama e mi dice che ha esaurito lo spazio disco della sua macchina virtuale con Windows Vista .

Durante la fase di setup aveva infatti accettato il valore di default di 20gb e dopo un paio di mesi di utilizzo , non aveva più spazio per poter archiviare i files.

L'assistenza gli aveva prospettato come unica soluzione la distruzione della macchina virtuale ed una completa reinstallazione del sistema operativo hosted.

Soluzione inelegante.

Con una sessione di googling di appena 3 minuti ecco come ho operato per rendere Paco nuovamente felice.

Per prima cosa ho scaricato questo tools di sviluppo per vmware che si puo' trovare [qui](http://communities.vmware.com/thread/88468)

Si tratta di una semplice gui per l'utility da riga di comando di vmware per il ridimensionamento e deframmentazione dei dischi virtuali .

[![](http://dema.tv/wp-content/uploads/2008/10/immagine-11.png)](http://dema.tv/wp-content/uploads/2008/10/immagine-11.png)

Dobbiamo scegliere il disco da ridimensionare e lo dobbiamo cercare in user->documenti>macchine virtuali. Impostiamo la nuova dimensione del disco a 40 gb e lanciamo il tutto con il pulsante "go" . Il processo di ridimensionamento è un po' lungo e puo' durare fino a 20 minuti , è importante non spazientirsi e non spegnere il computer durante questa operazione.

Al termine dell'espansione avremo un disco di dimensioni maggiori , ma la partizione sarà sempre  della dimensione precedente.

Per ovviare a questo dobbiamo munirci di gparted , un tool open source per la manipolazione delle partizioni dei dischi .

Scarichiamo una live image a [questo indirizzo ](http://gparted.sourceforge.net/download.php).

Apriamo il pannello di controllo di vmware e configuriamo i seguenti parametri

[![](http://dema.tv/wp-content/uploads/2008/10/immagine-31.png)](http://dema.tv/wp-content/uploads/2008/10/immagine-31.png)

Dobbiamo ora cambiare i parametri di boot della macchina virtuale ed è una cosa veramente difficile con vmware fusion 2.0 . Se con la versione 1.1.x era sufficiente inserire nel file di configurazione .vmx il parametro bios.forceSetupOnce = "TRUE" , questo parametro sembra non essere più riconosciuto con la versione 2.0 ( almeno a me non ha funzionato ) . Per riuscire a cambiare la sequenza di boot comunque basta avviare la macchina virtuale a pieno schermo , dare reboot e tenere pigiato fn+F2 .

Riavviato con il live cd di gparted ecco la schermata che ci appare :

[![](http://dema.tv/wp-content/uploads/2008/10/immagine-61.png)](http://dema.tv/wp-content/uploads/2008/10/immagine-61.png)

La schermata ci mostra il disco principale /dev/sda1 e dopo aver premuto Ridimensiona/sposta la dimensione massima a cui possiamo espandere la nostra partizione. Con il mouse trasciniamo fino ad espandere la partizione sda1 al massimo consentito .

Al termine dell'operazione riavviamo la macchina virtuale e ci troveremo di fronte a questa schermata da panico

[![](http://dema.tv/wp-content/uploads/2008/10/image481.png)](http://dema.tv/wp-content/uploads/2008/10/image481.png)

Ok niente panico . Rientriamo nel pannello di controllo di vmware fusion e selezioniamo come dispositivo cdrom il lettore fisico del nostro mac , inseriamo il cd di Windows Vista e facciamo il boot da cd .

[![](http://dema.tv/wp-content/uploads/2008/10/image491.png)](http://dema.tv/wp-content/uploads/2008/10/image491.png)

Clicchiamo su next fino a fare apparire questa schermata

[![](http://dema.tv/wp-content/uploads/2008/10/image501.png)](http://dema.tv/wp-content/uploads/2008/10/image501.png)

Scegliamo Repair your computer e a questa schermata :

[![](http://dema.tv/wp-content/uploads/2008/10/image511.png)](http://dema.tv/wp-content/uploads/2008/10/image511.png)

Scegliamo Repair and Restart.

Windows riparerà il tutto ed al prossimo reboot il nostro windows virtuale sarà perfettamente attivo e performante , con tutto lo spazio di cui avremo bisogno .

Credits [howtogeek](http://www.howtogeek.com/howto/windows-vista/using-gparted-to-resize-your-windows-vista-partition/) e [Only talking sense](http://onlytalkingsense.wordpress.com/2007/12/27/vmware-fusion-expanding-a-disk-2/)
