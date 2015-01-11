---
author: dema
comments: true
date: 2010-04-28 09:49:26+00:00
layout: post
slug: xen-su-debian-lenny-64-bit-no-frills-guide-parte-1
title: Xen su debian lenny 64 bit, no frills guide parte 1
wordpress_id: 1011
categories:
- howto
tags:
- debian
- dom0
- domu
- hypervisor
- linux
- virtual machines
- xen
---

Mi ero ripromesso di scrivere questa guida, per motivi pratici di supporto alla mia sempre fallace memoria e per motivi di condivisione con i miei colleghi in Cpline. C'è voluto un po' di tempo, ma finalmente eccomi qui con un modesto ma perfetto allo scopo Acer Altos G540 M2 pronto per essere trasformato in hypervisor per ospitare macchine (para)virtualizzate.

L'articolo è un barboso howto passo passo, quindi chi non fosse interessato puo' tranquillamente fermare la lettura qui


# <!-- more -->




## La scelta hardware


Per realizzare un hypervisor Xen possiamo veramente scegliere qualsiasi computer, a patto che soddisfi questo requisito:

Cpu con estensioni Intel VT o AMD-V

Semplice, non basta altro. Il modo più veloce per controllare se una cpu soddisfa i requisiti è quella di avviare con una live Linux distro e dare i seguenti comandi :

[sourcecode language="bash"]cat /proc/cpuinfo | grep vmx (per Intel)
cat /proc/cpuinfo | grep svm (per AMD)[/sourcecode]


## L'installazione di Debian amd64


Scegliamo di effettuare una installazione da netinstall che con soli 131 Mb di download della iso più lo scaricamento dei pacchetti base dalla repository APT ci permetterà di avere un sistema operativo di base aggiornato in breve tempo.

Io di solito utilizzo questo schema di partizioni :

100 Mb /boot ext2
10 Gb / ext3

Lo spazio rimanente lo userò a macchina pronta con LVM.

Una volta terminata l'installazione di default apportiamo alcuni ritocchi come l'immancabile installazione di SSH e la creazione di un file di swap, che non fa mai male.

[sourcecode language="bash"]apt-get install ssh
dd if=/dev/zero of=/var/spool/swapfile bs=1024 count=262144
mkswap /var/spool/swapfile
nano -w /etc/fstab
/var/spool/swapfile swap swap defaults 0 0
swapon -a[/sourcecode]


## L'installazione di Xen


Nel nostro ambiente Debian tutto quello che dobbiamo fare è dare questo semplice comando :

[sourcecode language="bash"]
apt-get install xen-hypervisor-3.2-1-amd64 xen-linux-system-2.6.26-1-xen-amd64 xen-utils-3.2-1 xenstore-utils xenwatch xen-shell xen-tools[/sourcecode]

Successivamente editiamo il file _/etc/modules_ inserendo la seguente linea

[sourcecode language="bash"]
loop max_loop=64[/sourcecode]

necessaria, qualora decidessimo di installare delle virtual machine con dischi basati su file immagine. Se abbiamo intenzione di usare unicamente virtual machines basate su LVM possiamo tralasciare questa modifica.

Di seguito diamo un ultimo ritocco a _/etc/xen/xend-config.sxp_ levando il commento a (network-script network-bridge) e commentando (network-script network-dummy). Inoltre inseriamo la linea (vif-script vif-bridge)

[sourcecode language="bash"]
(network-script network-bridge)
#(network-script network-dummy)
(vif-script vif-bridge)
[/sourcecode]

﻿In questo modo ci ritroveremo con un nuovo kernel con le patch di Xen e con tutti gli strumenti necessari  per riavviare con il nostro nuovo hypervisor.


## Riavvio e controlli


Riavviamo il nostro host hypervisor nuovo fiammante e dovremmo ritrovarci dopo la sequenza di boot al prompt di Linux.

Inserite le nostre credenziali verifichiamo che sia stato avviato il kernel xen con uname -a

[sourcecode language="bash"]
Linux vito2 2.6.26-1-xen-amd64 #1 SMP Fri Mar 13 22:30:40 UTC 2009 i686 GNU/Linux [/sourcecode]

Controlliamo che il demone di controllo xend sia avviato con un primo comando della serie _xm_

[sourcecode language="bash"]
xm list
Name                                        ID   Mem VCPUs      State   Time(s)
Domain-0                                     0   231     2     r-----  21609.8
[/sourcecode]

Per ultima cosa controlliamo il corretto settaggio delle schede di rete con _brctl show_

[sourcecode language="bash"]
bridge name     bridge id               STP enabled     interfaces
eth0            8000.001d6035f29d       no              peth0
[/sourcecode]

Se tutto è posto apponiamo ancora alcuni ritocchi ai file di configurazione e poi saremo pronti a creare la nostra prima macchina paravirtualizzata basata ovviamente su debian 64bit.

Il primo parametro da cambiare tra quelli di default è XENDOMAINS_SAVE. Si trova in _/etc/default/xendomains_ .

Nella configurazione sample troviamo il path dove salvare le virtual machines quando spegniamo l'hypervisor. Questo comportamento però ha causato secondo la mia esperienza dei malfunzionamenti in sede di riavvio, con le macchine virtuali che non rispondevano e rimanevano bloccate. E' sufficiente non indicare alcun path nel file di configurazione per fare in modo che le macchine virtuali in sede di spegnimento dell'hypervisor vengano a loro volta spente prima di procedere allo shutdown dell'host principale.

Per fare in modo che all'avvio dell'hypervisor si avviino anche le virtual machine che ci interessano occorre creare una directory in _/etc/xen_ chiamata _auto_ e all'interno di essa creare dei link simbolici ai file di configurazione delle macchine virtuali da avviare allo startup. Questo magari lo vedremo in dettaglio quando creeremo la nostra prima macchina virtuale linux.

La seconda questione da sistemare è l'orologio di sistema. Per via di un bug del kernel 2.6.26-1-xen ogni virtual machine che fa riferimento a xen come clocksource può dare problemi  di sincronia dell'ora e degradare l'orologio con conseguente instabilità del sistema.

La soluzione è editare alcuni file di sistema. Il primo in Dom0 (hypervisor) in /etc/sysctl.conf aggiungendo la linea

[sourcecode language="bash"]
xen.independent_wallclock=1[/sourcecode]

Per quanto riguarda le virtual machines (DomU) oltre ad aggiungere la medesima linea sempre nel file _/etc/sysctl.conf_ dovremo indicare nel file di configurazione il parametro _extra="clocksource=jiffies"_

Siamo pronti a questo punto a creare la nostra prima virtual machine.

[Lo vedremo nel prossimo post](http://dema.tv/2010/05/17/xen-su-debian-lenny-64-bit-no-frills-guide-parte-2/).

Credits :

[http://wiki.debian.org/Xen
](http://wiki.debian.org/Xen)[ http://www.howtoforge.com/perfect_setup_xen3_debian](http://www.howtoforge.com/perfect_setup_xen3_debian)

Serie di posts Xen no frills guide:
[ Parte 1](http://wp.me/p2A8m-gj)
[ Parte 2](http://wp.me/p2A8m-gW)
[ Parte 3](http://wp.me/p2A8m-hW)
