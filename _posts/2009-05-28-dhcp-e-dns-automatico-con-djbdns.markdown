---
author: dema
comments: true
date: 2009-05-28 07:45:31+00:00
layout: post
slug: dhcp-e-dns-automatico-con-djbdns
title: DHCP e DNS automatico con DJbdns
wordpress_id: 815
categories:
- howto
tags:
- dhcp
- djbdns
- dns
- gentoo
- tinydns
---

Certe volte puo' tornare utile avere un dns all'interno della propria rete locale , in modo da avere una cache per la risoluzione degli host remoti e una risoluzione degli host della LAN.

E' anche molto utile aggiornare in tempo reale le tabelle del DNS con i record degli host che si aggiungono e ricevono gli indirizzi con DHCP.

In ambito UNIX tutto questo è possibile con Bind9 , apponendo le opportune modifiche al file named.conf in particolare nel parametro allow-update.

Ma io non sopporto bind e quindi avevo bisogno di una soluzione efficace con DjbDNS.

Ecco il setup completo partendo da una stock Gentoo Linux.

<!-- more -->


## DHCP


Per prima cosa installiamo il server dhcp

[sourcecode language='cpp']
emerge -av dhcpd [/sourcecode]

Configuriamo il server dhcp editando il file /etc/dhcp/dhcpd.conf

[sourcecode language='cpp']
nano -w /etc/dhcp/dhcpd.conf
ddns-update-style none;
option domain-name-servers 172.16.1.1;
option routers 172.16.1.1;
###sezione per server wins , se si opera senza , omettere
option netbios-name-servers 172.16.1.3;
default-lease-time 21600;
max-lease-time 21600;
log-facility local7;
subnet 172.16.1.0 netmask 255.255.255.0 {
range 172.16.1.100 172.16.1.254;
}[/sourcecode]

Facciamo ripartire il server ed aggiungiamo lo start al runlevel di default

[sourcecode language='cpp']

/etc/init.d/dhcpd restart
rc-update add dhcpd default [/sourcecode]


## DJBDNS


L'installazione è lontana solo un

[sourcecode language='cpp']
emerge -av djbdns[/sourcecode]

Configuriamo separatamente i due componenti distinti della suite djbdns. Cominciamo con Tinydns


## Tinydns


Ecco i passaggi in rapida successione 

[sourcecode language='cpp']
echo 127.0.0.1 > /service/tinydns/env/IP [/sourcecode]

[sourcecode language='cpp']
nano -w /service/tinydns/root/static.localdomain
..:127.0.0.1:a:259200
Zlocaldomain:localdomain.:localdomain.:2005100111:28800:7200:604800:3600:3600
&localdomain;::ns1.localdomain:3600
&localdomain;::ns2.localdomain:3600
@localdomain::mail.localdomain.:10
=localdomain:172.16.1.1:86400
+mail.localdomain:172.16.1.1:3600
+ns1.localdomain:172.16.1.1:3600
+ns2.localdomain:172.16.1.1:3600
+www.localdomain:172.16.1.1:3600
+srv1.localdomain:172.16.1.3:3600
+srv2.localdomain:172.16.1.4:3600
+srv3.localdomain:172.16.1.2:3600
+srv4.localdomain:172.16.1.136:3600[/sourcecode]

[sourcecode language='cpp']
cd /service/tinydns/root
wget http://files.demaitalia.com/djbdns/Makefile
[/sourcecode]

[sourcecode language='cpp']
touch db.localdomain[/sourcecode]

[sourcecode language='cpp']
cd /bin[/sourcecode]

[sourcecode language='cpp']
wget http://files.demaitalia.com/djbdns/djb_update.pl[/sourcecode]

Un po' di spiegazioni , specialmente per il Makefile che abbiamo scaricato.

Ecco il contenuto

[sourcecode language='cpp']
data.cdb: data
	/usr/bin/tinydns-data
	/etc/init.d/svscan restart

db.localdomain: static.localdomain dhcp.localdomain
	cat static.localdomain dhcp.localdomain > db.localdomain
	cat db.localdomain | sed 's/[t]//g' > db.cerca

data: db.*
	cat db.cerca > data[/sourcecode]

Questo makefile permette allo script perl djb_update.pl di aggiornare il file di database data.cdb che servirà a tinydns a risolvere gli host della nostra zona. Rispetto al makefile originale che invoca unicamente il comando tinydns-data , questo  invoca un refresh completo di djbdns ( dnscachex e tinydns ) , effettua un merge tra la tabella degli indirizzi statici e quelli dinamici letti dalle lease del dhcp e li formatta correttamente rimuovendo gli spazi in eccesso (tramite sed).

Lo script perl djb_update.pl che avremo piazzato in /bin dovremo renderlo eseguibile con _chmod +x /bin/djb_update.pl_ ed editare /etc/conf.d/local.start

[sourcecode language='cpp']
/bin/djb_update.pl >/dev/null[/sourcecode]

Per fare in modo che venga eseguito all'avvio del sistema.


## Dnscachex


Anche qui in rapida successione i comandi da impartire :

[sourcecode language='cpp']
echo 172.16.1.1 > /service/dnscachex/env/IP[/sourcecode]

[sourcecode language='cpp']
nano -w /service/dnscachex/root/servers/localdomain
127.0.0.1[/sourcecode]

[sourcecode language='cpp']
nano -w /service/dnscachex/root/servers/@
87.118.111.215
87.174.67.134
208.67.222.222
208.67.220.220[/sourcecode]

[sourcecode language='cpp']
/etc/init.d/svscan restart[/sourcecode]

Non ci siamo discostati di una virgola dal setup canonico di dnscachex .

Nella sezione /root/server aggiungiamo la nostra zona localdomain e facciamo puntare a 127.0.0.1 dove è in ascolto tinydns .

Per quanto riguarda tutto il resto , deleghiamo la risoluzione a server esterni ( ho indicato i server di [Fooldns ](http://fooldns.com/beta.html)dell'amico [Matteo ](http://www.lastknight.com/)e Opendns che non sta molto simpatico all'amico Matteo  :P)

Dnscachex agirà da helper dns tenendo in cache le risoluzioni di tutta la nostra rete locale , diminuendo considerevolmente il traffico in uscita sulla porta 53 udp e velocizzando anche la navigazione dei client.


## Finalizzazioni e Test 


Non ci rimane che cambiare i parametri nel file /etc/resolv.conf

[sourcecode language='cpp']
echo 172.16.1.1 > /etc/resolv.conf[/sourcecode]

Proviamo a vedere se funziona tutto :

[sourcecode language='cpp']
dig localdomain any 

; <<>> DiG 9.4.3-P2 <<>> localdomain any
;; global options:  printcmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 65225
;; flags: qr rd ra; QUERY: 1, ANSWER: 5, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;localdomain.                  IN      ANY

;; ANSWER SECTION:
localdomain.           3600    IN      SOA     localdomain. localdomain. 2005100111 28800 7200 604800 3600
localdomain.           3600    IN      NS      ns1.localdomain.
localdomain.           3600    IN      NS      ns2.localdomain.
localdomain.           86400   IN      MX      10 mail.localdomain.
localdomain.           86400   IN      A       172.16.1.1

;; Query time: 7 msec
;; SERVER: 172.16.1.1#53(172.16.1.1)
;; WHEN: Mon May 25 12:39:21 2009
;; MSG SIZE  rcvd: 139[/sourcecode]

Proviamo ad aggiungere una macchina alla rete . L'host si chiama toshalah

[sourcecode language='cpp']
dig toshalah.localdomain

 <<>> DiG 9.4.3-P2 <<>> toshalah.localdomain
;; global options:  printcmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 59448
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;toshalah.localdomain.         IN      A

;; ANSWER SECTION:
toshalah.localdomain.  287     IN      A       172.16.1.149

;; Query time: 3 msec
;; SERVER: 172.16.1.1#53(172.16.1.1)
;; WHEN: Thu May 28 09:11:18 2009
;; MSG SIZE  rcvd: 55[/sourcecode]


## E con Debian/Ubuntu ?


Yes we can ( maro' che effetto, suona già preistoria questo motto )

Il setup è perfettamente uguale , la parte più difficoltosa è l'installazione di djbdns.

Per saperne di più si puo' consultare per Debian [un mio post di qualche tempo fa](http://dema.tv/2008/09/25/un-dns-per-la-tua-rete-locale/) che tratta di djbdns , per Ubuntu si puo' seguire la guida di [Howtoforge per server 8.04](http://www.howtoforge.com/perfect-djbdns-setup-on-ubuntu8.04-amd64) ( non so se con la nuova Jaunty è cambiato qualcosa )


## Credits





	
  * [Michael K. Stella djb_update.pl script ](http://www.thismetalsky.org/projects/dhcp_dns.xml)


