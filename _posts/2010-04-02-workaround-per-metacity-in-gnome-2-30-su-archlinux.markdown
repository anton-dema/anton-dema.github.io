---
author: dema
comments: true
date: 2010-04-02 08:27:24+00:00
layout: post
slug: workaround-per-metacity-in-gnome-2-30-su-archlinux
title: Workaround per Metacity in Gnome 2.30 su Archlinux
wordpress_id: 1005
categories:
- linux
tags:
- arch
- gnome
- linux
- metacity
---

Ieri è stato rilasciato per Archlinux l'aggiornamento a Gnome 2.30. I [cambiamenti](http://library.gnome.org/misc/release-notes/2.30/), oltre al solito bugfix e un nuovo set di icone, sono incentrati su una migliore usabilità e facilità di accesso alle funzioni più comuni.

Purtroppo esiste al momento un bug di metacity, il gestore delle finestre di default di Gnome, per il quale se si clicca con il tasto destro nella barra del titolo della finestra si blocca tutto il desktop. Per ripristinare è necessario loggarsi in una console tty con CTRL+ALT F1 e uccidere il processo con killall -9 metacity .

Per ovviare a questo fastidio bisogna effettuare un downgrade di metacity, con il solito procedimento " a la arch"



	
  * editare il file /etc/pacman.conf ed inserire la riga "IgnorePkg    = metacity"

	
  * scaricare il pkg tar gz di metacity-2.28.1-1 ([qui](http://schlunix.org/archlinux/extra/os/i686/metacity-2.28.1-1-i686.pkg.tar.gz) oppure presso il mio mirror [qui](http://dema.tv/wp-content/uploads/2010/04/metacity-2.28.1-1-i686.pkg.tar.gz))

	
  * eseguire il comando "pacman -U metacity-2.28.1-1-i686.pkg.tar.gz"


NB: Questo è un instant post che probabilmente perderà di valore tra una manciata di ore quando la community arch renderà disponibile il pacchetto patchato.
