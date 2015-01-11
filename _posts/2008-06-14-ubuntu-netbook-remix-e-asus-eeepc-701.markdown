---
author: dema
comments: true
date: 2008-06-14 17:25:28+00:00
layout: post
slug: ubuntu-netbook-remix-e-asus-eeepc-701
title: 'Ubuntu Netbook Remix e Asus eeePC 701 '
wordpress_id: 225
categories:
- asus
- eee
- eeepc
- netbook
- ubuntu
- ubuntu netbook remix
tags:
- asus
- eee
- eeepc
- netbook
- ubuntu
- ubuntu netbook remix
---

Avevo letto ieri in vari blog di informazione che finalmente Canonical aveva rilasciato i pacchetti alpha per Ubuntu Netbook Remix.

Il progetto Netbook mira a costruire una Ubuntu tagliata su misura per gli Ultra Mobile PC adattando l'interfaccia utente agli schermi da 7 o 9 pollici e ottimizzando le prestazioni per i prossimi processori Intel Atom.

Proviamo a remixare la nostra Ubuntu 8.04 su eeePC 701 con Netbook?



	
  1. per prima cosa dobbiamo aggiungere delle nuove repository al nostro file /etc/apt/sources. Aggiungiamo alla fine del file le seguenti linee :
_deb http://ppa.launchpad.net/netbook-remix-team/ubuntu hardy main
deb-src http://ppa.launchpad.net/netbook-remix-team/ubuntu hardy main_

	
  2. installiamo i seguenti pacchetti:
_sudo apt-get install go-home-applet human-netbook-theme maximus window-picker-applet_

	
  3. preleviamo i **sorgenti di ume-launcher** , dal momento che **il binario non supporta risoluzioni inferiori a 1024x600** e se lo installassimo sul nostro eeePC 701 otterremmo delle icone troppo piccole come si puo' vedere dallo screenshot sottostante[![](http://dema.tv/wp-content/uploads/2008/06/shift-netbook-remix-02a1.png)](http://dema.tv/wp-content/uploads/2008/06/shift-netbook-remix-02a1.png)
I comandi per ottenere i sorgenti sono :
_cd /tmp
sudo apt-get source ume-launcher
sudo apt-get build-dep ume-launcher_

	
  4. Scompattiamo il file ume-launcher_0.3ubuntu3.tar.gz:
_tar zxvf ume-launcher_0.3ubuntu3.tar.gz_

	
  5. Preleviamo la [patch](http://launchpadlibrarian.net/15187604/ume-launcher-800x480-fix-iconsize.patch) per i sorgenti , rinominiamola come ume-launcher.patch ed applichiamola:
_patch -p0 < ume-launcher.patch_

	
  6. A questo punto creiamo il nostro pacchetto deb personalizzato :
_cd /tmp/ume-laucher-0.3ubuntu3
sudo dpkg-buildpackage -rfakeroot_

	
  7. una volta ottenuto il nostro pacchetto installiamolo :
_dpkg -i ume-launcher_0.3ubuntu3_i386.deb_

	
  8. Cambiamo l'aspetto di gnome con il nuovo tema Human-Netbook su _Sistema->Preferenze->Aspetto_

	
  9. Aggiungiamo le applet window-picker e go home al pannello superiore di gnome e rimuoviamo tutto il resto ad eccezione di Area di notifica , orologio e volume. Rimuoviamo anche il pannello inferiore per avere ancora più schermo a disposizione.

	
  10. Verifichiamo i programmi di avvio sessione con _Sistema->Preferenze->Sessioni_ ed aggiungiamo se non fossero presenti maximus e ume-launcher

	
  11. Facciamo ripartire la sessione di gnome e se tutto è andato liscio dovremmo ottenere una cosa di questo tipo
[![](http://dema.tv/wp-content/uploads/2008/06/demanetbookremix1.png)](http://dema.tv/wp-content/uploads/2008/06/demanetbookremix1.png)


Devo dire che la nuova interfaccia è veramente cool e si presta benissimo con backgroud a tinte fosche . Per quanto riguarda l'usabilità  niente di rivoluzionario ma risulta subito evidente che il pannello di xandros os per eeepc è stato preso ad esempio.

Insomma , per concludere , Netbook non mi ha impressionato in modo particolare. E' indubbiamente un'interfaccia semplice e molto curata graficamente ma non cambia il modo di operare tradizionale di Gnome.

La domanda è quindi : ce n'era veramente bisogno ?
