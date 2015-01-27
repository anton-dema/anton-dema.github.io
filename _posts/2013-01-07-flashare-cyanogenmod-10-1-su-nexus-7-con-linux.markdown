---
author: dema
comments: false
date: 2013-01-07 09:37:40+00:00
layout: post
slug: flashare-cyanogenmod-10-1-su-nexus-7-con-linux
title: Cyanogenmod 10.1 su nexus 7 con Linux
wordpress_id: 2307
categories:
- appunti
tags:
- adb
- android
- clockworkmod
- cyanogenmod
- fastboot
- linux
---

Scrivo questa guida passo passo per installare cyanogenmod 10.1 su Nexus 7, come promemoria, spero omnicomprensivo, dal momento che su internet non ho trovato una spiegazione chiara ed esaustiva, almeno in lingua italiana.

La guida presuppone che venga usato linux come sistema operativo per procedere allo sblocco del bootloader e android-sdk come unico tool necessario.

A bomba sui


## Requisiti preliminari


Procuriamoci il pacchetto android Sdk. Con archlinux dovremo prelevare i seguenti pacchetti AUR: android-sdk e android-sdk-platform-tools. Per le distribuzioni debian-centriche, nello specifico Ubuntu, il pacchetto è disponibile tramite [PPA W8](https://launchpad.net/~nilarimogard/+archive/webupd8/+index?batch=75) . Per altre distribuzioni basate su rpm è sufficiente consultare le guide di vostra appartenenza. Qui, ad esempio, [c'è una pagina wiki](https://fedoraproject.org/wiki/HOWTO_Setup_Android_Development) che spiega come installare l'android development kit su Fedora.

Lo scopo sostanziale dell'installazione di android-sdk è di avere due strumenti da console: _adb e fastboot_, che ci consentiranno di procedere all'unlock ed al trasferimento delle immagini di cyanogenmod all'interno del nostro nexus 7.

Ora procediamo all'operazione, passo passo.


## Passo 1:


(nexus7)
Assicuriamoci di avere l'ultima versione di Android 4.2.1 stock installata tramite aggiornamento OTA di Google.


## Passo 2:


(nexus7)
Abilitiamo il debug usb in Nexus 7. Per farlo, scegliamo: _impostazioni->informazioni_ sul tablet e facciamo tap per 7 volte su Numero Build. Ottenuti i permessi di sviluppatore, scegliamo _opzioni sviluppatore_ e selezioniamo _debug USB_.


## Passo 3:


(computer Linux)
Assicuriamoci che fastboot e adb siano stati installati correttamente e che il PATH sia stato esportato, tramite i comandi _adb_ e _fastboot_, che, lanciati senza argomenti, dovranno restituirci il solito prontuario di help dei parametri disponibili.


## Passo 4:


(computer Linux)
Scarichiamo l'immagine della [recovery di Clockworkmod](http://download2.clockworkmod.com/recoveries/recovery-clockwork-6.0.1.9-grouper.img)


## Passo 5:


(computer Linux)
Scarichiamo i pacchetti necessari per Cyanogenmod 10.1 per [Nexus 7 wifi (grouper)](http://get.cm/?device=grouper) e le [Google Apps](http://goo.im/gapps/latest) per Jelly Bean


## Passo 6:


(computer linux)
Colleghiamo via usb il nostro computer al nexus7 e verifichiamo che venga riconosciuto da adb, tramite il seguente comando:

    
    sudo su
    [root@badapple ~]# adb devices
    List of devices attached 
    015d24bc5d641414	device
    


Se otteniamo, come qui sopra, il seriale del dispositivo, siamo pronti per procedere.


## Passo 7:


(computer linux)
Trasferiamo le immagini precedentemente scaricate nel Nexus7. Possiamo farlo o via mtd o tramite console con

    
    adb push cm-10.1-20130105-NIGHTLY-grouper.zip /sdcard/
    adb push gapps-jb-20121212-signed.zip /sdcard/
    




## Passo 8:


(computer linux)
Otteniamo i permessi di root e posizioniamoci nella directory dove abbiamo scaricato la recovery Clockworkmod. Diamo il comando

    
     
    adb reboot bootloader
    


per mettere il nexus7 in modalità fastboot. Una volta ottenuta la schermata di fastboot sul nexus7, verifichiamo che il dispositivo venga riconosciuto dal nostro tool con

    
    fastboot devices.
    


Anche qui, questo comando dovrà restituirci il numero seriale del nostro nexus7.
Passiamo quindi a sbloccare il bootloader del nostro dispositivo:

    
    fastboot oem unlock
    


Apparirà un disclaimer sullo schermo del nexus7, al quale dovremo rispondere yes. I bootloader ora è sbloccato e possiamo installare la Clockworkmod recovery


## Passo 9:


(computer linux)
Dopo il riavvio del dipositivo ormai sbloccato, ripetiamo la procedura di fastboot mode per installare clockworkmod:

    
    adb reboot bootloader
    


La schermata di fastboot, ormai familiare, ci dirà che il dispositivo è sbloccato e che quindi ci sarà possibile flashare Clockworkmod.

    
    fastboot flash recovery recovery-clockwork-6.0.1.9-grouper.img
    


La procedura durerà alcuni secondi. Una volta terminata, tramite i pulsanti volume e power del nexus7 potremo riavviare il terminale in recovery mode.


## Passo 10:


(nexus7)
Dalla schermata di Clockworkmod recovery, tramite i pulsanti volume e power scegliamo nell'ordine:
wipe data/factory reset
install zip from sdcard
install cm-10-XXX.zip
install gapps-jb-XXX.zip
reboot.

Fatto, il nostro Nexus7 è ora cyanogenmoddato. :-)

Come ho detto nella premessa, ci sono tonnellate di guide in rete, anche in italiano, ci sono toolkit che fanno tutto in un click, ma questo approccio, che ho usato anche ieri per sbloccare il mio Nexus7, mi è sembrato il più lineare e semplice, con il totale controllo delle operazioni.

Se avessi commesso degli errori o se aveste bisogno di quelche chiarimento, sono sempre disponibili qui nei commenti e su google plus.

[Follow @dema on ADN](https://alpha.app.net/dema)

[+Antonangelo De Martini»](https://plus.google.com/106700489171066016161/about?rel=author)
