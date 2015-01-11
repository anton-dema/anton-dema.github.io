---
author: dema
comments: true
date: 2009-04-18 14:06:55+00:00
layout: post
slug: upgrade-da-ubuntu-810-a-904-in-5-facili-passi
title: Upgrade da Ubuntu 8.10 a 9.04 in 5 facili passi
wordpress_id: 719
categories:
- howto
tags:
- ext3
- ext4
- jaunty jackalope
- ubuntu 9.04
---

[![Ubuntu: For Desktops, Servers, Netbooks and in the cloud](http://www.ubuntu.com/files/countdown/static.png)](http://www.ubuntu.com/)

Con conversione del filesystem da ext3 a ext4 . Let's go !



	
  * Passo 1 :
Apriamo la console e digitiamo
**_sudo update-manager -d_** .
Accettiamo il cambio di versione proposto . Attendiamo la fine del processo di upgrade. Riavviamo.

	
  * Passo 2 :
Procuriamoci un'immagine di[ Jaunty Jackalope live iso](http://cdimage.ubuntu.com/daily-live/current/) .
Masterizziamo o creiamo un usb stick bootabile. Rifacciamo il boot da live image.

	
  * Passo 3 :
Una volta all'interno della live session apriamo il terminale e digitiamo
**_sudo su
_**e poi di seguito
**_tune2fs -O extents,uninit_bg,dir_index /dev/sda1_**
sostituendo ovviamente a sda1 la partizione che volete convertire in ext4 .
Ancora un comando:
**f_sck -pf /dev/sda1_**
che puo' durare fino a 10 minuti , dipende dalle dimensione della partizione da controllare.

	
  * Passo 4 :
Montiamo la partizione appena convertita con
**_mnt -t ext4 /dev/sda1 /mnt_**
e cambiamo il tipo di filesystem in
_**mnt/etc/fstab**_
da ext3 a ext4 come indicato nella figura sottostante (clik per ingrandire)
[![change](http://dema.tv/wp-content/uploads/2009/04/change.png?w=300)](http://dema.tv/wp-content/uploads/2009/04/change1.png)

	
  * Passo 5 :
Reboot , sfiliamo la stick usb o il cdrom e rifacciamo il boot da disco fisso .


Il nostro sistema ora vola via felice grazie alla [lepre cornuta](http://it.wikipedia.org/wiki/Jackalope) ed ext4

