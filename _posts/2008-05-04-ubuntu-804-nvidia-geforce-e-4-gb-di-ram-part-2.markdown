---
author: dema
comments: true
date: 2008-05-04 09:14:27+00:00
layout: post
slug: ubuntu-804-nvidia-geforce-e-4-gb-di-ram-part-2
title: Ubuntu 8.04 , Nvidia Geforce e 4 Gb di Ram - part 2
wordpress_id: 197
categories:
- adobe
- alsa
- flash
- hardy
- hardy heron
- linux
- ubuntu 8.04
tags:
- adobe
- alsa
- flash
- hardy
- hardy heron
- linux
- ubuntu 8.04
---

Ero rimasto con i drivers di alsa da installare.

Prelevare i sorgenti di alsa :
**_ sudo apt-get install alsa-source_**

Posizionarsi in /usr/src e scompattare i sorgenti:
**_ sudo tar xjvf alsa-driver.tar.bz2_**

Entrare in /usr/src/modules/alsa-driver , configurare e compilare i driver:
**sudo su
./configure
make
make install
depmod -a**

Ora alsa è correttamente installato. C'è bisogno però di sbarazzarci di Pulse Audio , se non vogliamo che Firefox crashi in continuazione usando flash o che l'audio di skype si senta a balzelli.

[![](http://dema.tv/wp-content/uploads/2008/05/schermata1.png)](http://dema.tv/wp-content/uploads/2008/05/schermata1.png)

Per farlo usiamo Synaptic e deselezioniamo tutte le voci relative a Pulseaudio.

Siamo arrivati in fondo , pare.

Esiste però ancora una macchia su tutte le Ubuntu che ho avuto modo di installare :** Flashplugin-nonfree è instabilissimo !!!**

Sul forum di Ubuntu ci sono decine di thread che discutono di questo argomento sin dalla 6.10 , molteplici voci sono state aperte su bugzilla , ma ad oggi la distribuzione di Canonical non è arrivata ad alcuna  soluzione.

Ok ,** il problema è di Adobe , che rilascia (intenzionalmente ?? ) un prodotto bacato per Linux** , ma le altre distribuzioni come Fedora , Mandriva , Gentoo , Arch etc. sono molto meno soggette a crash del plugin , almeno così si evince dando una rapida occhiata ai forum di discussione.

La speranza è che nei prossimi mesi , grazie a [Open Screen Project di Adobe](http://www.adobe.com/aboutadobe/pressroom/pressreleases/200804/050108AdobeOSP.html) che apre pubblicamente le specifiche di flash , si possa arrivare ad un livello di usabilità soddisfaciente di [Gnash](http://www.gnu.org/software/gnash/) o [Swfdec](http://swfdec.freedesktop.org/wiki/) ed abbandonare definitivamente il maledetto flashplugin-nonfree.
