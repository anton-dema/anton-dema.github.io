---
author: dema
comments: true
date: 2008-03-21 18:20:10+00:00
layout: post
slug: trasformare-lasus-eeepc-in-un-hot-spot
title: Trasformare l'Asus eeePC in un hot spot
wordpress_id: 172
categories:
- access point
- asus eee
- eeepc
- hotspot
- madwifi
- nat
tags:
- access point
- asus eee
- eeepc
- hotspot
- madwifi
- nat
---

Niente di trascendentale e piuttosto rozzo come metodo.

Premesse :



	
  1. Non è uno script automatico punta e clicca

	
  2. Funziona con tutti i computer linux con i driver di Madwifi

	
  3. Presenta alcuni noiosi bug [StuckBeacon](http://madwifi.org/wiki/StuckBeacon)

	
  4. Le istruzioni qui di seguito sono riferite ad una stock Ubuntu con Gnome desktop environment.


Insomma **starete pensando , una chiavica** . In un certo senso sì , ma funzionicchia.

Cominciamo con  disabilitare il nm-applet di Gnome che crea casino nella gestione dell'interfaccia wifi.

Per fare questo andiamo su Sistema-->Preferenze-->Sessioni

![schermata-1.jpg](http://dema.tv/wp-content/uploads/2008/03/schermata-11.jpg)

E _deflagghiamo _il Network Manager Applet.

Ora apriamo il nostro editor preferito e buttiamo giù questo dirty script

[sourcecode language='cpp']

wlanconfig ath0 destroy;
wlanconfig ath1 destroy;
wlanconfig ath create wlandev wifi0 wlanmode ap;
iwpriv ath1 bgscan 0;
iwpriv ath1 mode 2;
sleep 5;
iwconfig ath1 essid minieee;
ifconfig ath1 192.168.10.1 up;
echo 1 > /proc/sys/net/ipv4/ip_forward;
iptables -t nat -A POSTROUTING -o ppp0 -j MASQUERADE;
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE;
iwpriv ath1 bintval 500[/sourcecode]
Salviamolo , chiamiamolo _apoint _e rendiamolo eseguibile _:chmod +x apoint_ .

Ora un'altra zozzeria per buttare giù il nostro access point
[sourcecode language='cpp']

wlanconfig ath0 destroy
wlanconfig ath1 destroy[/sourcecode]
Salviamo con nome _end _e rendiamo eseguibile con il comando _chmod +x end._

A questo punto abbiamo tutto quello che ci serve , ad eccezione del dhcp da rilasciare ai client.

Per questo possiamo usare dnsmasq e configurarlo come server dhcp sull'interfaccia wireless.__

_sudo apt-get update && sudo apt-get install dnsmasq_

I parametri da modificare in /etc/dnsmasq.conf sono
[sourcecode language='cpp']

interface=ath1

dhcp-range=192.168.10.50,192.168.10.150,255.255.255.0,12h[/sourcecode]
Ora prendiamo i due file _apoint _ed _end _e con i permessi di root spostiamoli in /usr/local/bin.Dopo un reboot apriamo una console e diamo questa sequenza di comandi :

    
    <i>sudo end </i> <i></i> <i>sudo apoint </i>


In questo modo **il vostro asus eee si trasformerà in un access point** , nattando le connessioni sia della porta ethernet eth0 che dell'eventuale modem ppp0.

Per buttare giù l'access point date da shell

_sudo end _

Per ritornare alla modalità client , disattivate il servizio dnsmasq :

_sudo update-rc.d -f dnsmasq remove_

e riabilitate la nm-applet di Gnome.

Sto sperimentando anche **la versione captive portal FON** , ma devono aver messo un controllo sul radiusnasid del chilli.conf  , in quanto non riesco ad andare oltre la login page.
