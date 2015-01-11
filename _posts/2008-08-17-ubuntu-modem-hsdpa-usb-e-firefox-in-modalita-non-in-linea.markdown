---
author: dema
comments: true
date: 2008-08-17 08:47:18+00:00
layout: post
slug: ubuntu-modem-hsdpa-usb-e-firefox-in-modalita-non-in-linea
title: Ubuntu , modem HSDPA usb e Firefox in modalità non in linea
wordpress_id: 290
categories:
- linux
- ubuntu
- workaround
tags:
- firefox
- hsdpa
- huawei
- offline
- ubuntu
- usb
---

In questa settimana di ferrragosto sono stato un po' a Venezia per trascorrere una mini vacanza . Ho portato con me **l'Asus eeePC** ed un modem **Huawei e220 usb** .

Non avendo a disposizione nessuna wireless e men che meno una connessione ethernet l'unica connettività possibile è stata tramite hsdpa con Vodafone. La connessione in tutta onestà si è comportata abbastanza bene con una velocità di tutto rispetto e pochissime disconnessioni forzate.

Per accedere alla rete ho usato il dialer ppp wvdial sotto ubuntu e la connessione è sempre avvenuta in maniera rapida e precisa. MI sono però subito accorto che **c'è una piccola magagna** collegandosi in PPP tramite usb modem e **non usando Network-Manager** : Firefox parte sempre in modalità non in linea.

In Firefox 3 infatti è stato introdotto un link a dbus per ottenere da Network-Manager informazioni circa lo stato di rete dell'host. Se Network-Manager non rileva nessuna connessione attiva , Firefox se ne accorge e parte in modalità non in linea.

Questa è una grossa scocciatura. E' possibile ovviamente raggirare questo ostacolo in varie maniere , fra cui disinstallare Network-Manager . Io ho optato per il seguente workaround.

Aprire un terminale e digitare

[sourcecode language='css'] cd  /etc/dbus-1/system.d
 sudo nano -w NetworkManager.conf[/sourcecode]

Il contenuto del file NetworkManager.conf va editato sostituendo <allow send_interface="org.freedesktop.NetworkManager"/>
con <deny send_interface="org.freedesktop.NetworkManager"/>. L'occorrenza avviene per tre volte e in tutte e tre occorre cambiare il parametro .

[sourcecode language='xml'] 


                

                
                
        

                
                
        

                
                
        

                
                
                
        
	512

[/sourcecode]

Ora è sufficiente riavviare (o far ripartire il daemon dbus ) e abbiamo risolto il fastidioso "offline-mode" di Firefox 3 su Ubuntu .

Credits [Ubuntu Forums](http://ubuntuforums.org/showpost.php?p=5060822&postcount=6)
