---
author: dema
comments: true
date: 2010-06-07 07:32:34+00:00
layout: post
slug: xen-su-debian-lenny-64-bit-no-frills-guide-parte-3
title: Xen su debian lenny 64 bit, no frills guide parte 3
wordpress_id: 1112
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

Eccoci giunti al momento di installare delle virtual machines full virtualized sul nostro hypervisor Xen.

Inizieremo con la preparazione di un file immagine per ospitare il sistema windows e con la creazione dell'immagine Iso per l'installazione.
<!-- more -->


### Immagine Iso d'installazione e creazione disco virtuale


Probabilmente il sistema operativo windows da installare sarà memorizzato su un dvd o su un cdrom. Per maggiore comodità creiamo un'immagine iso per poi usarla come sorgente dell'installazione.

Sotto windows possiamo creare l'immagine tramite tools adatti allo scopo ([imgburn](http://www.imgburn.com/) e forse un altro centinaio), con linux usiamo dd con questo semplice comando

[sourcecode language="bash"]
dd if=/dev/sr0 of=/tmp/winimage.iso[/sourcecode]

A questo punto dobbiamo creare una partizione o una  directory, se abbiamo a disposizione un discreto spazio disco, dentro la quale ospitare i file immagine delle macchine virtuali. Nei nostri esempi ho creato una directory /machines.

Creiamo la struttura delle directory:

[sourcecode language="bash"]
mkdir /machines/xen
mkdir /machines/xen/domains
mkdir /machines/xen/domains/srv2k8[/sourcecode]

Spostiamo l'immagine iso del cd d'installazione precedentemente creato in /tmp

[sourcecode language="bash"]
mv /tmp/winimage.iso /machines/xen/domains/srv2k8/[/sourcecode]

E creiamo un disco virtuale sparse da 12Gb

[sourcecode language="bash"]
dd if=/dev/zero of=/machines/xen/domains/srv2k8/disk.img bs=1M count=0 seek=12288[/sourcecode]


### Creazione file di configurazione HVM e installazione sistema operativo


Ora scriviamo il file di configurazione per la nuova macchina virtuale HVM. In /etc/xen creiamo un file chiamato srv2k8.cfg e scriviamo quanto segue.

[sourcecode language="bash"]
kernel = '/usr/lib/xen-3.2-1/boot/hvmloader'
builder = 'hvm'
memory = '1024'
device_model='/usr/lib/xen-3.2-1/bin/qemu-dm'

# Disks
disk = [ 'tap:aio:/machines/xen/domains/srv2k8/disk.img,hda,w',
'tap:aio:/machines/xen/domains/srv2k8/winimage.iso,hdc:cdrom,r'
]

# Hostname
name = 'srv2k8'

# Networking
vif = ['mac=00:16:3E:3C:F7:28,bridge=eth0']

# Behaviour
boot='d'
vnc=1
vncviewer=1
sdl=0
acpi=1
localtime=1[/sourcecode]

Siamo pronti quindi per iniziare l'installazione di windows. Dalla shell lanciamo la nuova macchina virtuale con _xm create srv2k8.cfg_.

Qui si presenta subito un problema: come faccio ad interagire con il display della macchina virtuale? Semplice, nel file di configurazione sopra abbiamo specificato il parametro _vnc=1 _ che farà in modo che sul nostro host principale venga lanciata una sessione vnc al fine di poter monitorare ed interagire con la nostra installazione.

Quindi è sufficiente collegarsi tramite un viewer vnc alla porta 5900 del nostro hypervisor ed eseguire un'installazione di windows come faremmo con un host fisico.

[caption id="attachment_1123" align="alignnone" width="450" caption="monitorare ed interagire con le macchine virtuali via vnc"][![](http://dema.tv/wp-content/uploads/2010/06/schermata.png?w=300)](http://dema.tv/wp-content/uploads/2010/06/schermata1.png)[/caption]

Una volta portata a termine l'installazione impostiamo l'accesso remoto via rdp al nuovo host windows, settiamo un indirizzo ip fisso per poter facilmente ricollegare la nostra macchina virtuale via remote desktop e infine effettuiamo lo shutdown windows.

Nuovamente nella shell di Dom0 andiamo ad editare il file di configurazione _/etc/xen/srv2k8.cfg_ cambiando i parametri di boot e rimuovendo il dispositivo tap:aio del cdrom.

[sourcecode language="bash"]
# Disks
disk = [ 'tap:aio:/machines/xen/domains/srv2k8/disk.img,hda,w',
]
# Behaviour
boot='c'[/sourcecode]


### Rifiniture


Riavviato il nuovo host windows, sempre con _ xm create srv2k8.cfg _ e collegati via dektop remoto, scarichiamo i GplPV drivers per windows. Questi drivers permettono di usare direttamente il backend di controller scsi e scheda ethernet di Dom0, incrementando le performance della nostra macchina virtuale.

Possiamo scaricare i drivers da [http://www.meadowcourt.org/downloads/](http://www.meadowcourt.org/downloads/) scegliendo quello adatto alla versione di windows installata.

I drivers vengono identificati secondo questo criterio :
gplpv_(chk/fre)_(platform)_(arch)_(version).msi

**chk** è una build per sviluppatori con strumenti di debug, **free** è la versione senza strumenti di debug.

**platform** indica la versione di windows:** wxp=Windows Xp, wnet=Windows Server 2003, wlh=Vista/Server2008**.

**arch** potrà essere o AMD64 o x86.

**version** è la release dei drivers.

Qui sotto una schermata di esempio nella quale si può notare che la versione wlh di gplpv si installa egregiamente anche su Windows Seven.

[![Installazione GplPV drivers](http://dema.tv/wp-content/uploads/2010/06/schermata-1.png?w=300)](http://dema.tv/wp-content/uploads/2010/06/schermata-11.png)

Abbiamo terminato. Non ci resta che creare il link simbolico dentro Dom0 in _/etc/xen/auto_

[sourcecode language="bash"]
cd /etc/xen/auto
ln -sf /etc/xen/srv2k8.cfg srv2k8.cfg
[/sourcecode]

In questo modo la DomU HVM windows si avvierà all'avvio dell'hypervisor e, grazie ai GplPV drivers, effettuerà un corretto shutdown quando verrà arrestato il sistema.

I prossimi passi saranno:
Installazione di Xen 4.0 su Debian Lenny + Libvirt + Virt-manager
Realizzazione di un HA Xen con DRBD e Heartbeat

Serie di posts Xen no frills guide:
[ Parte 1](http://wp.me/p2A8m-gj)
[ Parte 2](http://wp.me/p2A8m-gW)
[ Parte 3](http://wp.me/p2A8m-hW)
