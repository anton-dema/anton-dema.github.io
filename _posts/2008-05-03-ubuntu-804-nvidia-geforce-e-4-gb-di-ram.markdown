---
author: dema
comments: true
date: 2008-05-03 18:44:49+00:00
layout: post
slug: ubuntu-804-nvidia-geforce-e-4-gb-di-ram
title: 'Ubuntu 8.04 , Nvidia Geforce  e 4 Gb di Ram  '
wordpress_id: 195
categories:
- hardy
- hardy heron
- linux
- ubuntu
- ubuntu 8.04
- ubuntu hardy heron
tags:
- hardy
- hardy heron
- linux
- ubuntu
- ubuntu 8.04
- ubuntu hardy heron
---

Problema:

Dato un computer con 4 Gb di memoria Ram e scheda video Nvidia Geforce , si installi l'ultima distribuzione Linux Ubuntu 8.04 , conosciuta anche come Hardy Heron , senza rinunciare all'utilizzo di tutta la memoria disponibile o agli ultimi drivers di Nvidia per linux 169.12.

Risoluzione del problema:



	
  1. Installare la versione server di Ubuntu 8.04 per risparmiare il più possibile tempo.

	
  2. Installare i seguenti componenti :
_** sudo apt-get install linux-kernel-devel fakeroot build-essential**_

	
  3. Prelevare i sorgenti del kernel :
_** sudo apt-get install linux-source**_

	
  4. Posizionarsi dentro la directory /usr/src e scompattare i sorgenti :
**_ tar xjvf linux-source-2.6.24.tar.bz2_**

	
  5. Linkare /usr/src/linux ai sorgenti :
**_ ln -sf /usr/src/linux-source-2.6.24 linux_**

	
  6. Copiare il .config esistente :
**_sudo cp -vi /boot/config-`uname -r` /usr/src/linux_**

	
  7. Installare le libncursers per usare make menuconfig per scegliere le opzioni  kernel :
**_sudo apt-get install libncurses5 libncurses5-dev_**

	
  8. Posizionarsi dentro la directory /usr/src/linux e lanciare :
**_sudo make menuconfig_**

	
  9. Ecco in schermata il parametro da cambiare
Bisogna deselezionare Xen guest support che ci impedisce di compilare il module nvidia per la nostra Geforce. Per giungere a deselezionare Xen guest support : **_Processor type and features -> Paravirtualized guest support -> Xen guest support_**
[![](http://dema.tv/wp-content/uploads/2008/05/makemenuconfig1.png)](http://dema.tv/wp-content/uploads/2008/05/makemenuconfig1.png)

	
  10. Uscire e salvare.

	
  11. Iniziare la compilazione del kernel :
**_sudo su_**
**_ make-kpkg clean_**
**_ fakeroot make-kpkg --initrd --append-to-version=il-vostro-nome-o-stringa kernel-image kernel-headers _**

	
  12. Andare a fare uno spuntino o rilassarsi con una puntata di una serie preferita , insomma cercarsi uno svago per i 40 minuti circa che ci vorranno  ad ultimare il processo.

	
  13. Posizionarsi in /usr/src e constatare l'esistenza di due pacchetti .deb  linux-headers e linux-image , dopodichè installarli :
**_ sudo dpkg -i linux-image-2.6.24.3dema_2.6.24.3dema-10.00.Custom_i386.deb_**
**_ sudo dpkg -i linux-headers-2.6.24.3dema_2.6.24.3dema-10.00.Custom_i386.deb_**

	
  14. Riavviare la macchina.

	
  15. Assicurarsi che grub faccia partire il nuovo kernel customizzato

	
  16. Installare l'ambiente desktop:
**_sudo apt-get install ubuntu-desktop_**

	
  17. Prelevare gli ultimi drivers di [nvidia ](http://www.nvidia.com/object/unix.html)

	
  18. Ottenere una console con Ctrl+Alt+F1 ,loggarsi e terminare gdm :
**_ sudo /etc/init.d/gdm stop_**

	
  19. Rendere eseguibile il pacchetto .run ed eseguirlo :
**_ sudo chmod a+x NVIDIA-Linux-x86-169.12-pkg1.run_**
**_ sudo ./NVIDIA-Linux-x86-169.12-pkg1.run_**

	
  20. Far ripartire il login manager grafico :
**_ sudo /etc/init.d/gdm start_**

	
  21. Fatto !! I driver di Nvidia daranno il benvenuto mostrando una splash image e l'accelerazione 3D sarà finalmente piena , avendo anche il supporto di tutti i 4Gb di RAM.


Rimarrebbe da sistemare ancora un paio di cosucce per rendere tutto perfetto , tra cui i driver alsa per la scheda audio e il completo sradicamento di pulse audio che fa crashare Firefox quando si guardano filmati in Flash.

Ci dedico un piccolo post domani.
