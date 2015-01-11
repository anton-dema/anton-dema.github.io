---
author: dema
comments: true
date: 2008-04-28 15:16:03+00:00
layout: post
slug: sincronizzare-iphone-con-amarok
title: Sincronizzare Iphone con Amarok
wordpress_id: 189
categories:
- apple
- linux
- wifi
- wireless
tags:
- amarok
- apple
- i fuse
- iphone
- ipod
- linux
- sshfs
- wfi
- wireless
---

![](http://dema.tv/wp-content/uploads/2008/04/731269699_ecfbab54a3_m.jpg)[![](http://dema.tv/wp-content/uploads/2008/04/garland_logo1.jpg?w=128)](http://dema.tv/wp-content/uploads/2008/04/garland_logo11.jpg)

Ho ricevuto l'iPhone giovedì scorso direttamente da Hong Kong , già bello che scarcerato e aggiornato alla 1.1.4.

Tra le prime cose di cui ho sentito la mancanza è stato ovviamente **uno scarso supporto per Linux** di questo meraviglioso gingillo.

In particolare mi premeva riuscire a riempire di canzoni gli 8 Gb a disposizione , visto che sul Macbook non ho nessun file mp3. Dopo brevi ricerche sono approdato ad un [bel tutorial per Ubuntu](https://help.ubuntu.com/community/PortableDevices/iPhone) che ho seguito , adattando qualche passo per applicarlo anche alla mia distro attuale , ossia Fedora.

Ecco per passi come operare per **sincronizzare ottimamente via wireless il nostro iPhone con Amarok** , l'amato player del lupo ululante.

Per prima cosa dobbiamo sbloccare l'iPhone , ma credo che questo ormai sia di una semplicità assoluta. Basta procurarsi [Ziphone](http://download.ziphone.org/) per Windows o Mac ed il gioco è fatto.

Una volta jailbreakato il nostro iPhone , installiamo mediante Installer.app _BSD Subsystem e OpenSSH_.

A questo punto dobbiamo agire sul nostro router wireless in modo che **assegni un indirizzo ip definito per il nostro iPhone**. Io uso dd-wrt su wrt54gs di Linksys e per assegnare sempre lo stesso indirizzo ad un apparecchio utilizzo dnsmasq , aggiungendo le seguenti stringhe nel campo dei comandi custom :

_dhcp-host=xx:xx:xx:xx:xx,192.168.0.60_

Alle x va sostituito il mac address dell'iPhone che si puo' recuperare da _Impostazioni->Generali->Info->Indirizzo Wi-Fi._

In questo modo , ogni volta che accederemo al nostro network wifi casalingo , il router assegnerà sempre l'indirizzo 192.168.0.60 all'iPhone.

Ora agiamo sulla nostra linuxbox. **Se siamo già passati ad Ubuntu 8.04** , sarà sufficiente installare il pacchetto _ipod-convenience_ che contiene tutte le dipendenze e gli script per montare file systems remoti via ssh.

Io invece ho usato Fedora ed ho installato _gtkpod e fuse-sshfs_.

Una volta installati i pacchetti , aggiungiamo il nostro account al gruppo fuse e creiamo una directory _/mnt/iphone_ con permessi 777.

Lato Amarok nelle impostazioni alla sezione device , clicchiamo su add device .  Il tipo di device da scegliere è ovviamente _apple ipod_ , il nome _Iphone_ e il punto di montaggio _/mnt/iphone_.

Salviamo e accediamo alla configurazione avanzata e immettiamo i seguenti parametri :

in pre-connection:

_sshfs -o uid=500  root@192.168.0.60:/private/var/mobile/Media /mnt/iphone_

(uid=500 è l'uid sulla mia linux box , scoprite il vostro con il comando _id username_ )

In post-disconnection:

_fusermount -u /mnt/iphone_

Chiudiamo Amarok e proviamo se funziona il giochino.

Accendiamo l'iPhone , **attiviamo la WIFI e il server ssh** . Attendiamo una trentina di secondi e lanciamo Amarok . Dovrebbe presentarsi **una mascherina che ci chiede una password**. Immettiamo la parola _alpine _(password di default per il firmware 1.1.4 dell'iPhone) .

Et voilà , il contenuto del nostro iPhone apparirà come per magia nella colonna sinistra del playlist manager di Amarok.

A questo punto , basta selezionare una playlist di nostro gradimento da Amarok e con il tasto  destro del mouse scegliere _invia a dispositivo multimediale_. Tornando alla sezione device di Amarok , far partire la coda di trasferimento e attendere la (piuttosto lenta a dirla tutta) copia dei brani dal nostro pc linux all'iPhone.

Riferimenti :

[https://help.ubuntu.com/community/PortableDevices/iPhone](https://help.ubuntu.com/community/PortableDevices/iPhone)

[http://gentoo-wiki.com/HOWTO_Using_an_iPhone_With_Gentoo_Linux](http://gentoo-wiki.com/HOWTO_Using_an_iPhone_With_Gentoo_Linux)
