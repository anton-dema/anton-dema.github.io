---
author: dema
comments: true
date: 2008-09-25 16:39:38+00:00
layout: post
slug: un-dns-per-la-tua-rete-locale
title: Un dns per la tua rete locale
wordpress_id: 390
categories:
- howto
tags:
- adblock
- djbdns
- dns
- fooldns
- spoofing
---

In questi ultimi tempi si è fatto un gran parlare del servizio DNS sia per via della sentenza che ha imposto ai provider italiani di impedire la risoluzione di  [thepiratebay.org](http://thepiratebay.org/) sia per il fastidioso problema dei banner pubblicitari e dei cookies traccianti che l'ottimo [Matteo Flora](http://www.lastknight.com) ha pensato di risolvere con il  suo [FoolDns](http://www.fooldns.com).

In questo post cercherò di illustrare in maniera semplice ed efficace come installare un server dns che si prenda in carico la risoluzione dei nomi e che filtri tutte le richieste ad hosts indesiderati.

Il post è piuttosto lungo , quindi consiglio la lettura solamente a chi è veramente interessato.

<!-- more -->

Per lo scopo dobbiamo recuperare un computer obsoleto , che magari avevamo dimenticato in soffitta o che qualcuno ha dismesso perché non più usabile con sistemi operativi moderni . L'unico requisito richiesto è una scheda di rete ed almeno 256 Mb di RAM.

Otteniamo un cd di installazione minimale di [Debian](http://www.debian.org/CD/netinst/) ed accertiamoci di avere una connessione ad internet stabile.
Una volta installato il sistema operativo , iniziamo ad aggiungere via internet quello di cui abbiamo bisogno. Prima però dobbiamo dare un indirizzo di rete fisso alla nostra macchina , una default gateway (di solito l'indirizzo del router verso internet ) e un dns provvisorio per la risoluzione dei nomi.

Ci logghiamo come root e editiamo il file /etc/network/interfaces con nano .

[sourcecode language='cpp']nano -w /etc/network/interfaces

# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
auto eth0
iface eth0 inet static
address 192.168.0.253
netmask 255.255.255.0
broadcast 192.168.0.255
gateway 192.168.0.1 [/sourcecode]

Ora facciamo puntare temporaneamente la macchina ad un server dns esterno per la risoluzione degli hosts internet :

[sourcecode language='cpp']
echo "nameserver 208.67.222.222" > /etc/resolv.conf [/sourcecode]

Facciamo ripartire la rete con:

[sourcecode language='css']
/etc/init.d/network restart [/sourcecode]

Controlliamo se riusciamo ad uscire su internet con un semplice ping :

[sourcecode language='css']
 debians:/etc/apt# ping maya.ngi.it
PING maya.ngi.it (88.149.128.3) 56(84) bytes of data.
64 bytes from maya.ngi.it (88.149.128.3): icmp_seq=1 ttl=54 time=58.3 ms [/sourcecode]

A questo siamo pronti per prelevare da internet i pacchetti di cui abbiamo bisogno.

Diamo una sistematina al nostro file /etc/apt/sources.list  e rendiamolo simile a questo :

[sourcecode language='css'] deb http://mi.mirror.garr.it/mirrors/debian/ stable main contrib non-free
deb-src http://mi.mirror.garr.it/mirrors/debian/ stable main contrib non-free
deb http://security.debian.org/ stable/updates main contrib non-free [/sourcecode]

Provvediamo ora ad installare il server DNS. Per questo tutorial la scelta è ricaduta sul pacchetto [djbdns](http://cr.yp.to/djbdns.html)

Ci logghiamo come root e diamo i seguenti comandi :

[sourcecode language='css']
aptitude update
aptitude install daemontools-installer [/sourcecode]

Subito dopo installiamo daemontools con il comando :

[sourcecode language='css'] build-daemontools  [/sourcecode]

Durante l'installazione di daemontools ci verranno chieste  alcune domande :

    
    <span class="system">Enter a directory where you would like to do this [/tmp/daemontools]</span> <span class="highlight"><-- ENTER</span>



    
    <em><span class="system">Which format would you like to use? [fD]</span> <span class="highlight"><-- ENTER</span></em>



    
    <em><span class="system">Press ENTER to continue...</span> <span class="highlight"><-- ENTER</span></em>



    
    <em><span class="system">Do you want to remove all files in /tmp/daemontools,
    except daemontools_0.76-9_i386.deb now? [Yn]</span> <span class="highlight"><-- ENTER</span></em>



    
    <em><span class="system">Do you want to install daemontools_0.76-9_i386.deb now? [Yn]</span> <span class="highlight"><-- ENTER</span></em>



    
    <em><span class="system">Do you want to purge daemontools-installer now? [yN]</span> <span class="highlight"><-- ENTER
    
    </span></em>


Accettiamo i valori di default premendo enter ad ogni occorrenza.

Ecco un altro pacchetto da installare :

[sourcecode language='css']aptitude install ucspi-tcp-src [/sourcecode]

Una volta scaricato installiamolo come già fatto per daemontools :

[sourcecode language='css'] build-ucspi-tcp [/sourcecode]

Anche qui ci verranno proposte alcune domande , alle quali rispondiamo come prima con i valori di default :

    
    <em><span class="system">Enter a directory where you would like to do this [/tmp/ucspi-tcp]</span> <span class="highlight"><-- ENTER</span></em>









    
     _qoptions = { tags:"IDG Tech Network" }; _qacct="p-25K88fxDSEn9Y";quantserve(); <em> <a href="http://www.quantcast.com/p-25K88fxDSEn9Y" target="_blank"><img src="http://dema.tv/wp-content/uploads/2008/09/p-25K88fxDSEn9Y.gif?tags=IDG%20Tech%20Network" style="display: none;" border="0" height="1" width="1" alt="Quantcast"/></a> </em>









    
    <em><span class="system">Press ENTER to continue...</span> <span class="highlight"><-- ENTER</span></em>



    
    <em><span class="system">Do you want to remove all files in /tmp/ucspi-tcp,
    except ucspi-tcp_0.88-10_i386.deb now? [Yn]</span> <span class="highlight"><-- ENTER</span></em>



    
    <em><span class="system">Do you want to install ucspi-tcp_0.88-10_i386.deb now? [Yn]</span> <span class="highlight"><-- ENTER</span></em>



    
    <em><span class="system">Do you want to purge ucspi-tcp-src now? [yN]</span> <span class="highlight"><-- ENTER</span></em>







Per finire preleviamo il pacchetto di djbdns e compiliamolo :






[sourcecode language='css'] aptitude install djbdns-installer [/sourcecode]

Lanciamo l'installazione al solito con :

[sourcecode language='css'] build-djbdns [/sourcecode]


Sempre le domande a cui rispondere con i valori di default :




    
    <em><span class="system">Enter a directory where you would like to do this [/tmp/djbdns]</span> <span class="highlight"><-- ENTER</span></em>



    
    <em><span class="system">Press ENTER to continue...</span> <span class="highlight"><-- ENTER</span></em>



    
    <em><span class="system">Do you want to remove all files in /tmp/djbdns,
    except djbdns_1.05-11_i386.deb now? [Yn]</span> <span class="highlight"><-- ENTER</span></em>



    
    <em><span class="system">Do you want to install djbdns_1.05-11_i386.deb now? [Yn]</span> <span class="highlight"><-- ENTER</span></em>



    
    <em><span class="system">Do you want to purge djbdns-installer now? [yN]</span> <span class="highlight"><-- ENTER</span></em>










Bene , siamo a buon punto , non ci resta che configurare i parametri per la nostra rete interna .




Creiamo la directory in cui risiederanno i files di tinydns , dnscache e axfrdns .




__



[sourcecode language='css']

mkdir /var/lib/svscan [/sourcecode]

__

__

_
_


Subito dopo configuriamo i tre demoni come segue :




__



[sourcecode language='css']

dnscache-conf dnscache dnslog /var/lib/svscan/dnscache 192.168.0.253
axfrdns-conf axfrdns dnslog /var/lib/svscan/axfrdns /var/lib/svscan/tinydns 127.0.0.1
tinydns-conf tinydns dnslog /var/lib/svscan/tinydns 127.0.0.1 [/sourcecode]

E creiamo dei link simbolici nella directory service :

[sourcecode language='css']

ln -s /var/lib/svscan/dnscache /service
ln -s /var/lib/svscan/axfrdns /service
ln -s /var/lib/svscan/tinydns /service [/sourcecode]


Abbiamo ultimato l'installazione di djbdns , non ci rimane che far ripartire i demoni




__



[sourcecode language='css']

/etc/init.d/djbdns restart [/sourcecode]

__
__


Abbiamo così a disposizione una cache dns fornita da dnscache , un authoritative dns server che risponde al nome di tinydns e un replication chiamato axfrdns . Per il nostro semplice setup sarà necessario sistemare i parametri di dnscache e di tinydns .




Cominciamo con dnscache .




Entriamo nella directory /service/dnscache/root e controlliamo che esistano due sottodirectory ip e servers. Entriamo in servers ed editiamo il file @ con n_ano -w @_




In questo file si dovrebbero trovare alcuni indirizzi ip di server dns di varie provenienze , tra cui Internic  e Apnic  che funzionano a dovere ma sono terribilmente laggati per ovvie ragioni di distanza. Possiamo a scelta inserire il dns server del nostro provider o in alternativa i soliti dns di OpenDNS.




Entriamo nella sottodirectory ip e dovremmo trovare un file vuoto di nome 127.0.0.1 . Per permettere l'accesso alla cache a tutti i computer della sottorete dobbiamo creare un file vuoto chiamato 192.168.0 e lo facciamo con il comando _touch 192.168.0._




A questo punto la nostra cache è perfettamente funzionante e subito dovremmo godere dei benefici di una risoluzione dei nomi quasi istantanea per le url di maggiore richiesta.




Ma uno dei nostri scopi è quello di tagliare fuori advertisement e cookies .




Per fare questo entriamo nuovamente in /services/dnscache/root/ e scarichiamo un piccolo script perl con



[sourcecode language='css']

wget http://qmail.jms1.net/djbdns/mkservers [/sourcecode]

__
__


Dobbiamo compilare una bella lista di url da bloccare e a questo scopo ci viene incontro Fooldns che la rilascia pubblicamente , anche formattata  per i nostri scopi .  Eccone un  breve estratto :




    
    tr.bt.alice.it:87.118.111.215
    partners.sprintrade.com:87.118.111.215
    adbard.net:87.118.111.215
    ads.arcuspubblicita.it:87.118.111.215
    adv.ilbanner.com:87.118.111.215
    stats.wordpress.com:87.118.111.215










Quindi una volta prelevata la lista con






[sourcecode language='css']

wget http://fooldns.com/rules.txt [/sourcecode]

apriamola e tramite nano facciamo una ricerca (ctrl W ) di 87.118.111.215 e una sostituzione (ctrl R ) di tutte le occorrenze (ctrl T) con 127.0.0.1.

Rinominiamola in servers.txt :

[sourcecode language='cpp']

mv rules.txt servers.txt [/sourcecode]

e lanciamo i seguenti comandi

[sourcecode language='cpp']chmod +x mkservers

./mkservers [/sourcecode]

__ In questo modo abbiamo compilato delle regole che dicono a dnscache di puntare a localhost per le url indicate.

Bene , con dnscache abbiamo terminato , ora dobbiamo passare a tinydns con un semplicissimo settaggio.  Entriamo in /service/tinydns/root ed editiamo il file data :

[sourcecode language='css']

nano -w /service/tinydns/root/data

..:127.0.0.1:a:259200
=.:127.0.0.1:8640 [/sourcecode]

Ora non ci rimane che compilare questo file in formato cdb dando semplicemente _make._Ultimo ma proprio ultimo step è far ripartire tutti i demoni con :

[sourcecode language='cpp']/etc/init.d/djbdns restart[/sourcecode]

__

__Se tutto è andato liscio abbiamo ottenuto con poco sforzo e poche risorse un dns tutto nostro da spoofare a piacimento.

Per poter far puntare tutti i computer della sottorete al nuovo server dns si puo' operare a manina o inserire il nuovo dns sul server dhcp della vostra rete LAN.

Credits [howtoforge](http://www.howtoforge.com/install-djbdns-nameserver-on-debian-etch) e [John M.Simpson](http://qmail.jms1.net/djbdns/blocking.shtml)

__Se doveste avere problemi , sono disponibile qui nei commenti per ogni chiarimento.__
