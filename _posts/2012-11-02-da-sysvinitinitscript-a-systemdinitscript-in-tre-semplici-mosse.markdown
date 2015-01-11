---
author: dema
comments: true
date: 2012-11-02 17:33:15+00:00
layout: post
slug: da-sysvinitinitscript-a-systemdinitscript-in-tre-semplici-mosse
title: Archlinux e systemd/initscript in tre semplici mosse
wordpress_id: 2239
categories:
- appunti
tags:
- daemons
- initscript
- linux
- systemd
- sysvinit
---

Se usate archlinux come me e se, compulsivamente, date, anche più volte in un giorno, _yaourt -Syua_, allora siete già pronti per passare ad un init misto systemd/initscript.

Ecco i tre passi da compiere per completare il passaggio:




    
  1. Andiamo su /etc/default/ e modifichiamo il file _grub _come segue

    
    nano -v grub
    GRUB_CMDLINE_LINUX_DEFAULT="init=/usr/lib/systemd/systemd quiet"


Ricreiamo il file _/boot/grub/grub.cfg_ con i seguenti comandi (ridondanti, ma nel più ci sta il meno):

    
    grub-install /dev/sda #o l'unità disco dal quale il vostro sistema fa il boot



    
    grub-mkconfig -o /boot/grubgrub.cfg



    
    mkinitcpio -p linux


Riavviate

    
  2. Il nostro sistema ora parte con un sistema di init misto, systemd/sysvinit/initscripts.
Dobbiamo controllare quali demoni partono all'avvio con il vecchio file _ /etc/rc.conf _e cercare di rimpiazzarli con systemd.
Apriamo due emulatori di terminale come illustrato qui in basso; in uno modifichiamo il file _/etc/rc.conf _in un altro attiviamo i comandi _ systemctl enable_[![terminali emulazione dema](http://dema.tv/wp-content/uploads/2012/11/parpost2-1024x554.jpg)](http://dema.tv/wp-content/uploads/2012/11/parpost2.jpg)Per ogni demone lanciato precedentemente dagli script sysvinit ed indicati nella riga DAEMONS in rc.conf dobbiamo trovare l'analogo per systemd.Possiamo aiutarci con la seguente tabella: (courtesy of [archwiki](https://wiki.archlinux.org/index.php/Daemons_List))

    
  3. Una volta compiuto questo passo, siamo pronti per togliere di mezzo sysvinit/initscript e passare a systemd/initscript

    
  4. Installiamo il pacchetto systemd-sysvcompat

    
    pacman -S systemd-sysvcompat
    risoluzione delle dipendenze in corso...
    ricerca dei conflitti in corso...
    :: systemd-sysvcompat e sysvinit vanno in conflitto. Vuoi rimuovere sysvinit? [s/N]


Rimuoviamo la riga da _/etc/default/grub_ GRUB_CMDLINE_LINUX_DEFAULT="init=/usr/lib/systemd/systemd, riconfiguriamo grub come al punto 1 e riavviamo.



In un prossimo post vedremo come passare ad un sistema con init in puro systemd.

[Follow @dema on ADN](https://alpha.app.net/dema)

[+Antonangelo De Martini»](https://plus.google.com/106700489171066016161/about?rel=author)
