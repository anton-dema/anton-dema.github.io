---
author: dema
comments: true
date: 2010-05-17 07:59:33+00:00
layout: post
slug: xen-su-debian-lenny-64-bit-no-frills-guide-parte-2
title: Xen su debian lenny 64 bit, no frills guide parte 2
wordpress_id: 1050
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

[Ci eravamo lasciati](http://dema.tv/2010/04/28/xen-su-debian-lenny-64-bit-no-frills-guide-parte-1/) con il riavvio di Dom0 nel nuovo ambiente di hypervisor, con tutti i controlli e gli accorgimenti del caso.

E' arrivato il momento di creare la prima virtual machine (DomU) linux paravirtualizzata.

<!-- more -->Per farlo ci serviremo dello strumento xen-tools, un set di scripts che automatizzeranno tutte le fasi di installazione per fare in modo di trovarci in brevissimo tempo a mettere le mani sul nuovo sistema operativo.

Per creare macchine virtuali possiamo usare dei dischi virtuali in formato immagine o usare LVM. Io tratterò unicamente la seconda opzione.

Se vi ricordate lo schema di partizioni usato nel precedente articolo, avevamo definito unicamente le partizioni di /boot e di / . Il resto del disco era rimasto non partizionato. Cominciamo quindi ad usare quello che resta del nostro volume come LVM per ospitare macchine virtuali nel nostro sistema.


#### Setup LVM


Una volta loggati in Dom0 usiamo lo strumento di partizione cfdisk lanciandolo con i seguenti parametri :

[sourcecode language="bash"]
cfdisk /dev/sda
 cfdisk (util-linux-ng 2.13.1.1)

                               Unità disco: sdb
                       Dimensioni: 104857600 byte, 104 MB
             Testine: 255   Settori per traccia: 63   Cilindri: 12

    Nome        Flag       Tipo part. Tipo FS          [Etichetta]    Dim. (MB)
 ------------------------------------------------------------------------------
                            Pri/Log   Spazio disponibile                  98,71

[/sourcecode]

Nell'esempio questo piccolo volume da 100Mb lo useremo per intero con LVM. Scegliamo [nuova]->[primaria]->[dimensione](tutto il disco)->[tipo]->[8e]; infine scegliamo [scrivi]
A questo punto dobbiamo inizializzare il volume fisico appena creato per l'uso con LVM.

[sourcecode language="bash"]
pvcreate /dev/sda1
Physical volume "/dev/sdb1" successfully created[/sourcecode]


Controlliamo se la creazione del volume è andata a buon fine

[sourcecode language="bash"]pvdisplay
  "/dev/sdb1" is a new physical volume of "94,10 MB"
  --- NEW Physical volume ---
  PV Name               /dev/sda1
  VG Name
  PV Size               94,10 MB
  Allocatable           NO
  PE Size (KByte)       0
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               WcO34w-ip6Y-g08q-lcfN-AMrp-HWDK-inzGa1
[/sourcecode]


Ora creiamo il volume group al quale uniremo il nostro volume fisico

[sourcecode language="bash"]
vgcreate VMS /dev/sda1
Volume group "VMS" successfully created
vgdisplay
--- Volume group ---
  VG Name               VMS
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               92,00 MB
  PE Size               4,00 MB
  Total PE              23
  Alloc PE / Size       0 / 0
  Free  PE / Size       23 / 92,00 MB
  VG UUID               VOUD0l-P7A2-WofC-xCx7-3cyt-obtE-54l0p3
[/sourcecode]


Bene, abbiamo creato il nostro Volume Group, all'interno del quale, tramite gli xen-tools, creare dei volumi logici per ospitare le macchine virtuali.


#### Xen-tools


Tramite un semplice file di configurazione﻿﻿﻿ ﻿ xen-tools.conf in /etc/xen-tools/ possiamo definire i parametri per la creazione automagica delle nostre DomU paravirtualizzate. 
Ecco un esempio del file xentools.conf
[sourcecode language="bash"]
lvm = VMS
install-method = debootstrap
size   = 4Gb      # Disk image size.
memory = 128Mb    # Memory size
swap   = 128Mb    # Swap size
# noswap = 1      # Don't use swap at all for the new system.
fs     = ext3     # use the EXT3 filesystem for the disk image.
dist   = lenny     # Default distribution to install.
#image  = sparse   # Specify sparse vs. full disk images.
dhcp = 1
passwd = 1
serial_device = hvc0
kernel      = /boot/vmlinuz-`uname -r`
initrd      = /boot/initrd.img-`uname -r`
mirror = http://ftp.it.debian.org/debian/
ext3_options   = noatime,nodiratime,errors=remount-ro
ext2_options   = noatime,nodiratime,errors=remount-ro
xfs_options    = defaults
reiser_options = defaults
role = udev
[/sourcecode]
Possiamo lasciarlo tranquillamente come quello sopra esposto per avere una virtual machine di base, se avremo bisogno di assegnare più ram o un secondo disco potremo agire in un secondo tempo. 
Tutto quello che ci rimane da fare quindi a questo punto è invocare lo script xen-create-image
[sourcecode language="bash"]
xen-create-image --hostname=testmeout
General Information
--------------------
Hostname       :  testmeout
Distribution   :  lenny
Partitions     :  swap            128Mb (swap)
                  /               4Gb   (ext3)
Image type     :  full
Memory size    :  128Mb
Kernel path    :  /boot/vmlinuz-2.6.26-1-xen-amd64
Initrd path    :  /boot/initrd.img-2.6.26-1-xen-amd64

Networking Information
----------------------
IP Address     : DHCP [MAC: 00:16:3E:4E:A7:44]


Creating swap on /dev/VMS/testmeout-swap
Done

Creating ext3 filesystem on /dev/VMS/testmeout-disk
Done
Installation method: debootstrap
Done

Running hooks
No role scripts were specified.  Skipping

Creating Xen configuration file
Done
Setting up root password
Enter new UNIX password:*****
Retype new UNIX password:***** 
passwd: password updated successfully
All done


Logfile produced at:
	 /var/log/xen-tools/testmeout.log
[/sourcecode]
La nostra nuova virtual machine è pronta. Apportiamo giusto due modifiche al file di configurazione e al primo boot del file /etc/sysctl.conf in DomU e siamo pronti per cominciare.
Avviamo la nuova macchina virtuale con  
[sourcecode language="bash"]
xm create testmeout.cfg -c 
[/sourcecode]
Alla fine della sequenza di boot logghiamoci come root e immettiamo la password che abbiamo creato quando xen-create-image ce l'ha chiesta.
Editiamo il file /etc/sysctl.conf della macchina virtuale e spegniamo l'host
[sourcecode language="bash"]
nano -w /etc/sysctl.conf
aggiungere questa linea alla fine del file di configurazione
xen.independent_wallclock=1
e spegniamo l'host 
halt
[/sourcecode]
Ora da Dom0 rimettiamo mano al file di configurazione /etc/xen/testmeout.cfg 
[sourcecode language="bash"]
nano -w /etc/xen/testmeout.cfg
aggiungere questa linea alla fine del file di configurazione generato da xen-create-image
extra="clocksource=jiffies"
[/sourcecode]

Ci siamo. Non ci resta che riavviare la nuova DomU con il comando 
[sourcecode language="bash"] xm create testmeout.cfg [/sourcecode] e fare in modo che parta in automatico con il riavvio dell'hypervisor Dom0
[sourcecode language="bash"]
ln -sf /etc/xen/testmeout.cfg /etc/xen/auto/testmeout.cfg[/sourcecode]

Nel prossimo post vedremo come creare macchine virtuali basate su HVM (full virtualized), quindi tipicamente sistemi operativi Windows. 

Credits:
[http://wiki.debian.org/Xen
](http://wiki.debian.org/Xen)[http://www.howtoforge.com/perfect_setup_xen3_debian](http://www.howtoforge.com/perfect_setup_xen3_debian)

Serie di posts Xen no frills guide:
[ Parte 1](http://wp.me/p2A8m-gj)
[ Parte 2](http://wp.me/p2A8m-gW)
[ Parte 3](http://wp.me/p2A8m-hW)

