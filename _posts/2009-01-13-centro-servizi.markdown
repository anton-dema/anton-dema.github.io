---
author: dema
comments: true
date: 2009-01-13 18:08:11+00:00
layout: post
slug: centro-servizi
title: Centro Servizi
wordpress_id: 580
categories:
- howto
tags:
- centro uffici
- dansguardian
- linux
- shorewall
- squid
- switch
- vlan
---

Per la serie post di utilità prima di tutto per la mia sempre più fallace memoria , e poi forse anche per qualcuno in cerca di una soluzione di routing per un setup simile.

Problema : **Predisporre la connettività per un centro servizi da 10 uffici
**

Soluzione : un server Linux , uno switch managed layer 2 da 16 porte, 11 switch unmanaged da 6 porte , due router adsl alice business .

L'articolo è un pippone tecnico parecchio lungo , quindi se proprio ti interessa puoi continuare la lettura :)

<!-- more -->

Graficamente possiamo riassumere come segue :

Cliccare per ingrandire:

[![salamone](http://dema.tv/wp-content/uploads/2009/01/salamone.png)](http://dema.tv/wp-content/uploads/2009/01/salamone.png)

Per poter garantire l'isolamento di ogni ufficio abbiamo la necessità di assegnare ad ogni nodo un network da instradare poi all'interno del nostro router.

Prendiamo per prima cosa in esame come operare a livello server linux e switch managed layer 2.

Per un compito del genere non era possibile inserire nel server firewall tante schede di rete per quante reti c'era bisogno , per un evidente motivo di spazio e slot PCI disponibili. La tecnologia VLAN ci è venuta incontro per accompiere a questo scopo.

Grazie a VLAN una singola interfaccia fisica ethernet puo' avere fino a 4096 interfacce virtuali con il loro indirizzo IP univoco e separate in network diversi eventualmente routabili tramite il nostro server linux.

Per implementare VLAN in Ubuntu i pacchetti necessari sono disponibili con il semplice comando

[sourcecode language='css']aptitude install vlan[/sourcecode]

Come secondo passo occorre definire gli indirizzi ip da assegnare ad ogni VLAN. Io avevo bisogno di 10 indirizzi di rete ed ho scelto blocchi in classe B editando il file /etc/network/interface come segue

[sourcecode language='css']

auto lo
iface lo inet loopback

#eth0 è il primo gateway WAN
auto eth0
iface eth0 inet static
address 192.168.50.2
netmask 255.255.255.0
broadcast 192.168.50.255

gateway 192.168.50.1

#eth1 è il secondo gateway WAN
auto eth1
iface eth1 inet static
address 192.168.51.2
netmask 255.255.255.0
broadcast 192.168.51.255

#eth2 è l'interfaccia che ospiterà le nostre VLAN

auto eth2
iface eth2 inet static
address 172.16.0.1
netmask 255.255.255.0
broadcast 172.16.0.255

#qui comincia la definizione delle VLAN

auto vlan2
auto vlan3
auto vlan4
auto vlan5
auto vlan6
auto vlan7
auto vlan8
auto vlan9
auto vlan10
auto vlan11

#VLAN2
iface vlan2 inet static
address 172.16.1.1
netmask 255.255.255.0
broadcast 172.16.1.255
vlan_raw_device eth2

#VLAN3
iface vlan3 inet static
address 172.16.2.1
netmask 255.255.255.0
broadcast 172.16.2.255
vlan_raw_device eth2

#VLAN4
iface vlan4 inet static
address 172.16.3.1
netmask 255.255.255.0
broadcast 172.16.3.255
vlan_raw_device eth2

#VLAN5
iface vlan5 inet static
address 172.16.4.1
netmask 255.255.255.0
broadcast 172.16.4.255
vlan_raw_device eth2

#VLAN6
iface vlan6 inet static
address 172.16.5.1
netmask 255.255.255.0
broadcast 172.16.5.255
vlan_raw_device eth2

#VLAN7
iface vlan7 inet static
address 172.16.6.1
netmask 255.255.255.0
broadcast 172.16.6.255
vlan_raw_device eth2

#VLAN8
iface vlan8 inet static
address 172.16.7.1
netmask 255.255.255.0
broadcast 172.16.7.255
vlan_raw_device eth2

#VLAN9
iface vlan9 inet static
address 172.16.8.1
netmask 255.255.255.0
broadcast 172.16.8.255
vlan_raw_device eth2

#VLAN10
iface vlan10 inet static
address 172.16.9.1
netmask 255.255.255.0
broadcast 172.16.9.255
vlan_raw_device eth2

#VLAN11
iface vlan11 inet static
address 172.16.10.1
netmask 255.255.255.0
broadcast 172.16.10.255
vlan_raw_device eth2 [/sourcecode]

Non rimane a questo punto altro che far ripartire la rete con /etc/init.d/networking restart.
Se tutto è andato per il verso giusto , dovremmo avere una configurazione di rete con ben 10 interfacce virtuali vlanX e tre interfacce fisiche ethX ognuna appartenente alla sua rete ed isolata dalle altre.

Per distribuire queste reti appena create abbiamo bisogno di uno switch managed Layer 2 e N-switch unmanaged per quante reti necessitiamo.

Nel particolare esempio di sopra io ho troncato sullo switch le porte dalla 1 alla 10 marcandole UNTAGGED nei parametri "ports to vlan" , mode=general , admit-all e con PVID della VLAN di appartenenza . Sull'ultima porta dello switch , fisicamente collegata alla eth2 , ho settato mode=general , admit-all e PVID 1 ; la marcatura "ports to VLAN" impostata su TAGGED .

Questo passaggio è un po' criptico , me ne rendo conto , e per questo inserisco alcuni screenshot relativi allo switch Linksys SRW2016. E' ovvio che lo stesso setup puo' essere fatto con un qualsiasi altro Layer2 managed .

![schermata1](http://dema.tv/wp-content/uploads/2009/01/schermata11.jpg)

![schermata2](http://dema.tv/wp-content/uploads/2009/01/schermata21.jpg)

![schermata3](http://dema.tv/wp-content/uploads/2009/01/schermata31.jpg)

Ora abbiamo bisogno di un po' di sano dhcp e delle buone regole di iptables.

Cominciamo con il dhcp. La scelta è ricaduta su dhcp3-server che nella nostra ubuntu server e lontana giusto un

[sourcecode language='css']aptitude install dhcp3-server[/sourcecode]

Il file di configurazione usato è il seguente

[sourcecode language='css']ddns-update-style none;
default-lease-time 86400;
max-lease-time 86400;

subnet 172.16.1.0 netmask 255.255.255.0 {
range 172.16.1.100 172.16.1.150;
option routers 172.16.1.1;
option domain-name-servers 172.16.1.1;
interface vlan2;
}

subnet 172.16.2.0 netmask 255.255.255.0 {
range 172.16.2.100 172.16.2.150;
option routers 172.16.2.1;
option domain-name-servers 172.16.2.1;
interface vlan3;
}

subnet 172.16.3.0 netmask 255.255.255.0 {
range 172.16.3.100 172.16.3.150;
option routers 172.16.3.1;
option domain-name-servers 172.16.3.1;
interface vlan4;
}

subnet 172.16.4.0 netmask 255.255.255.0 {
range 172.16.4.100 172.16.4.150;
option routers 172.16.4.1;
option domain-name-servers 172.16.4.1;
interface vlan5;
}

subnet 172.16.5.0 netmask 255.255.255.0 {
range 172.16.5.100 172.16.5.150;
option routers 172.16.5.1;
option domain-name-servers 172.16.5.1;
interface vlan6;
}

subnet 172.16.6.0 netmask 255.255.255.0 {
range 172.16.6.100 172.16.6.150;
option routers 172.16.6.1;
option domain-name-servers 172.16.6.1;
interface vlan7;
}
subnet 172.16.7.0 netmask 255.255.255.0 {
range 172.16.7.100 172.16.7.150;
option routers 172.16.7.1;
option domain-name-servers 172.16.7.1;
interface vlan8;
}

subnet 172.16.8.0 netmask 255.255.255.0 {
range 172.16.8.100 172.16.8.150;
option routers 172.16.8.1;
option domain-name-servers 172.16.8.1;
interface vlan9;
}

subnet 172.16.9.0 netmask 255.255.255.0 {
range 172.16.9.100 172.16.9.150;
option routers 172.16.9.1;
option domain-name-servers 172.16.9.1;
interface vlan10;
}

subnet 172.16.10.0 netmask 255.255.255.0 {
range 172.16.10.100 172.16.10.150;
option routers 172.16.10.1;
option domain-name-servers 172.16.10.1;
interface vlan11;
}
[/sourcecode]

A questo punto , dopo aver dato un _/etc/init.d/dhcp3-server restart_ potete cominciare a divertirvi a giocare al telefonista anni 30 , inserendo e disinserendo lo spinotto di rete nelle varie porte dello switch e vedere il vostro computer cambiare ogni volta indirizzo IP . Io mi sono entusiasmato , ma si sa sono un inguaribile nerd.

Passiamo alle regole di iptables. Come per ogni altro lavoro , io mi affido all'ottimo frontend offerto da shorewall . Vediamo di seguito come procedere per garantire :



	
  1. Connettività internet con due adsl e tramite transparent proxy

	
  2. Isolamento delle vlan tra di loro e verso il proxy


Come detto sopra il nostro link verso internet è fornito da due adsl , di telecom nello specifico , quindi dopo aver cambiato gli indirizzi ip dei due router in 192.168.50.1 e 192.168.51.1 cominciamo ad editare il primo file di configurazione di shorewall: _providers_.

[sourcecode language='css']

alice1    1       1       main            eth0            192.168.50.1  track,balance
alice2    2      2       main            eth1            192.168.51.1  track,balance
[/sourcecode]

Con questo semplice script abbiamo detto di inserire nella tabella di routing un round robbing robin verso la WAN.

Ora definiamo le zone del nostro firewall editando il file zones

[sourcecode language='css'] 

fw      firewall
net     ipv4
loc     ipv4
vlan2   ipv4
vlan3   ipv4
vlan4   ipv4
vlan5   ipv4
vlan6   ipv4
vlan7   ipv4
vlan8   ipv4
vlan9   ipv4
vln10   ipv4
vln11   ipv4

[/sourcecode]

Da notare che a shorewall non piacciono le zone definite con più di 5 caratteri , quindi VLAN11 diventa VLN11 :)

Il prossimo file è _interfaces_

[sourcecode language='css']

net     eth0    detect
net     eth1    detect
loc     eth2    detect  dhcp
vlan2   vlan2   detect  dhcp
vlan3   vlan3   detect  dhcp
vlan4   vlan4   detect  dhcp
vlan5   vlan5   detect  dhcp
vlan6   vlan6   detect  dhcp
vlan7   vlan7   detect  dhcp
vlan8   vlan8   detect  dhcp
vlan9   vlan9   detect  dhcp
vln10   vlan10  detect  dhcp
vln11   vlan11  detect  dhcp[/sourcecode]

Eth0 e eth1 sono le nostre interfacce wan , eth2 è la nostra interfaccia locale a cui sono attestate tutte le vlan

Ora un po di sano masquerading tramite l'editing del file _masq_

[sourcecode language='css']

eth0   192.168.51.2    192.168.50.2
eth1   192.168.50.2   192.168.51.2
eth0  eth2  192.168.50.2
eth1  eth2  192.168.51.2
eth2    vlan2
eth0    vlan2  192.168.50.2
eth1   vlan2    192.168.51.2
eth0    vlan3  192.168.50.2
eth1    vlan3   192.168.51.2
eth0    vlan4  192.168.50.2
eth1    vlan4   192.168.51.2
eth0    vlan5  192.168.50.2
eth1    vlan5   192.168.51.2
eth0    vlan6  192.168.50.2
eth1    vlan6   192.168.51.2
eth0    vlan7  192.168.50.2
eth1    vlan7   192.168.51.2
eth0    vlan8  192.168.50.2
eth1   vlan8    192.168.51.2
eth0    vlan9  192.168.50.2
eth1   vlan9    192.168.51.2
eth0    vlan10  192.168.50.2
eth1   vlan10   192.168.51.2
eth0    vlan11  192.168.50.2
eth1   vlan11    192.168.51.2[/sourcecode]

Già a questo punto le regole di masquerading permetterebbero il corretto funzionamento delle reti verso internet. Ma noi vogliamo un isolamento tra le reti a livello lan e soprattutto un filtraggio tramite proxy trasparente con controllo dei contenuti tramite dansguardian.

Mettiamo ora mano al file policy

[sourcecode language='css']

net     net     DROP
vlan2   loc     ACCEPT
loc     vlan2   ACCEPT
$FW     loc     ACCEPT
vlan2   $FW     ACCEPT
$FW     vlan2   ACCEPT
loc     $FW     ACCEPT
loc     net     ACCEPT
$FW     net     ACCEPT
all     all     REJECT  info
[/sourcecode]

Come possiamo vedere da queste regole solo la vlan2 ha accesso sia alla zona net ed al firewall , in quanto vlan2 verrà assegnata agli amministratori del centro uffici. La regola all all REJECT , posta come ultima della catena , impedisce qualsiasi connessione non indicata sopra.

Il prossimo file da editare è _rules_

[sourcecode language='css']

ACCEPT  net     fw      tcp     1022
ACCEPT  loc     vlan2   tcp     135,139
ACCEPT  loc     vlan2   icmp    8
ACCEPT  vlan2   fw      tcp     1:65500
ACCEPT  vlan2   fw      udp     1:65500
ACCEPT  vlan2   net     tcp     1:65500
ACCEPT  vlan2   net     udp     1:65500
ACCEPT  vlan3   fw      tcp     8080
ACCEPT  vlan3   fw      udp     53
ACCEPT  vlan3   net     tcp     443
ACCEPT  vlan4   fw      tcp     8080
ACCEPT  vlan4   fw      udp     53
ACCEPT  vlan4   net     tcp     443
ACCEPT  vlan5   fw      tcp     8080
ACCEPT  vlan5   fw      udp     53
ACCEPT  vlan5   net     tcp     443
ACCEPT  vlan6   fw      tcp     8080
ACCEPT  vlan6   fw      udp     53
ACCEPT  vlan6   net     tcp     443
ACCEPT  vlan7   fw      tcp     8080
ACCEPT  vlan7   fw      udp     53
ACCEPT  vlan7   net     tcp     443
ACCEPT  vlan8   fw      tcp     8080
ACCEPT  vlan8   fw      udp     53
ACCEPT  vlan8   net     tcp     443
ACCEPT  vlan9   fw      tcp     8080
ACCEPT  vlan9   fw      udp     53
ACCEPT  vlan9   net     tcp     443
ACCEPT  vln10   fw      tcp     8080
ACCEPT  vln10   fw      udp     53
ACCEPT  vln10   net     tcp     443
ACCEPT  vln11   fw      tcp     8080
ACCEPT  vln11   fw      udp     53
ACCEPT  vln11   net     tcp     443
REDIRECT  vlan3    8080     tcp      www
REDIRECT  vlan4    8080     tcp      www
REDIRECT  vlan5    8080     tcp      www
REDIRECT  vlan6    8080     tcp      www
REDIRECT  vlan7    8080     tcp      www
REDIRECT  vlan8    8080     tcp      www
REDIRECT  vlan9    8080     tcp      www
REDIRECT  vln10   8080      tcp      www
REDIRECT  vln11   8080     tcp       www
ACCEPT    $FW        net      tcp      www
[/sourcecode]

Vlan2 pieno accesso a internet e al firewall tutte le altre hanno accesso solo alla porta 8080 tcp del firewall , alla porta 53 udp sempre del firewall e alla porta 443 verso internet. Sulla porta 8080 TCP nel firewall rimane in ascolto Dansguardian che provvede poi a rigirare le richieste a squid sulla porta 3128 . Non mi dilungherò sul setup di questi due programmi .  La porta 53 UDP è aperta per le risoluzioni dei nomi e la 443 TCP per le connessioni in ssl , che non possono essere gestite da un transparent proxy.

Abbiamo finito . Con un _shorewall restart_ ricostruimo la nostra catena di iptables e tutto dovrebbe funzionare.

Questa è una configurazione statica che però male si addice ad un centro uffici , dove le stanze vengono affittate per soli due giorni e quindi scaduti i termini la connessione verso internet  deve cadere.

Io ho pensato e realizzato un accrocchio per segare le rotte delle vlan degli uffici in affitto , tramite un'estensione del programma gestionale che gira sulla rete dell'amministrazione.

Con una aggiunta al suo programma di contabilità , Mirko ha approntato uno schedulatore che a seconda delle prenotazioni invia in ssh al server i seguenti comandi :

[sourcecode language='css'] 

ip ro del 172.16.2.0/24[/sourcecode]

Quando deve tirare giù la vlan e

[sourcecode language='css']

ip ro add 172.16.2.0/24 dev vlan3 [/sourcecode]

Se dobbiamo riattivare la vlan .

Attualmente il centro servizi è gia in produzione con questo setup . Prossime implementazioni saranno una rete wireless con captive portal e un sistema di stampe centralizzate con credito prepagato a scalare.
