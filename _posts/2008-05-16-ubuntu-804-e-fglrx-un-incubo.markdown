---
author: dema
comments: true
date: 2008-05-16 16:56:08+00:00
layout: post
slug: ubuntu-804-e-fglrx-un-incubo
title: 'Ubuntu 8.04 e fglrx , un incubo '
wordpress_id: 208
categories:
- ati
- fglrx
- hardy heron
- linux
- ubuntu
- ubuntu 8.04
tags:
- ati
- fglrx
- hardy heron
- linux
- ubuntu
- ubuntu 8.04
---

Ci risiamo .

Dopo gli [impazzamenti della scorsa settimana](http://itfonblog.wordpress.com/2008/05/03/ubuntu-804-nvidia-geforce-e-4-gb-di-ram/) per conciliare versione a 32bit di Ubuntu 8.04 , 4 Gb di ram e drivers di Nvidia , stamane mi sono imbattuto in un'altra rottura di scatole con l'[airone ardito](http://www.ubuntu.com/).

In ufficio il computer di Sauro , il nostro designer , ha sempre avuto a bordo una versione di Ubuntu .

L'altro giorno ho provveduto all'**upgrade di Gutsy Gibbon 7.10** tramite update manager. Tutto è proceduto senza intoppi ma al primo reboot i driver restricted per la Radeon 9550 non andavano per niente bene. Accelerazione hardware assente con inceppamenti di X .

Deciso a non perdere tempo ho seguito la guida di [Unofficial Ati Linux Driver Wiki](http://wiki.cchtml.com/index.php/Ubuntu_Hardy_Installation_Guide#Method_2:_Manual_Method) ed ho provveduto ad installare i driver proprietari di Ati. Anche qui il processo si è svolto nella maniera più liscia possibile , e dopo il secondo reboot la situazione _sembrava_ radicalmente cambiata . Dico sembrava perché controllando se avessi ottenuto il direct rendering , mi veniva risposto maledettamente :

_$ fglrxinfo
display: :0.0  screen: 0
OpenGL vendor string: Mesa project: www.mesa3d.org
OpenGL renderer string: Mesa GLX Indirect_

Anche spulciando il forum di Ubuntu e il forum di Phoronix.com non riuscivo a trovare il bandolo della matassa.

La cosa che mi è saltata subito agli occhi era che il module fglrx non venisse caricato all'avvio di X e nemmeno manualmente con il comando modprobe fglrx. Eppure il modulo fglrx.ko era correttamente in /lib/modules/2.6.24-*/fglrx/.

Alla fine di un estenuante Google Fighting mi sono imbattuto in [questo thread di ottobre 2007](http://www.phoronix.com/forums/showthread.php?t=6017) per Ubuntu 7.10 che suggerisce il seguente workaround :



	
  1. verificare con il comando _sudo modprobe -vf fglrx_ venga restituita la seguente stringa :
_install /sbin/lrm-video fglrx_

	
  2. modificare il file /etc/modprobe.d/lrm-video come segue :
_sudo nano -w /etc/modprobe.d/lrm-video
#install fglrx /sbin/lrm-video fglrx $CMDLINE_OPTS_

	
  3. dare il comando
_sudo modprobe -vf fglrx_
e controllare il corretto output : _insmod /lib/modules/2.6.24-16-386/updates/dkms/fglrx.ko_

	
  4. far ripartire X con CTRL+ALT+Backspace


Fatto questo , è andato tutto a posto ; _fglrxinfo | grep direct_ restituiva la stringa yes. Alleluya !

Finalmente piena accelerazione 3D anche con scheda Ati e Ubuntu 8.04 Hardy Heron.

Ora non per dire , va bene che l'upgrade che ho fatto da 7.10 partiva da un'installazione un po' pasticciata , sempre per via dei driver di Ati , ma **Ubuntu non dovrebbe essere la distribuzione Linux per esseri umani , quella senza intoppi e via lisci come l'olio** ?

Dalle mie ultime esperienze con 8.04 sembra proprio di no !
